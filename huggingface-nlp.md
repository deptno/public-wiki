# huggingface nlp

## nlp
### [[wip]]
- encoder -> 분석
  - BERT(Bidirectional *Encoder* Representations from Transformers)
- decoder -> 생성
- encoder, decoder 모델 -> 요약
- 

### transformers
- 흐름
  - input -> tokenizer -> {input_ids, attension_mark}
  - {input_ids, attension_mark} -> model -> {hidden_state}
  - {hidden_state} -> head -> logits(tensor, 정규화되지 않은 점수)
  - logits -> *softmax* or 기타 활성함수 -> 확률

#### model
##### AutoTokenizer
> `input_ids` 를 생성한다
- `return_tensors` 를 지정하지 않으면 리스트로 모두 리턴된다

##### AutoModel
- feature == hidden state
  - 768 ~ 3072 혹은 그 이상의 고차원
- input -> feature -> head(모델의 다른 부분)
- { input_ids } 만이 model 의 **필수** input
- `AutoModel` 을 사용하면 `BertModel` 등의 상위 버전으로 checkpoint 로 부터 모델을 알아서 불러온다
- method
  - `from_pretrained`
  - `save_pretrained`

#### tokenizer
- 알고리즘
  - word base
    - 단어 또는 구분자로 잘라서 토큰화
    - 단어사전수가 커짐
  - character base
    - word baes 토크나이저에서 `UNK`nown 토큰을 줄이는 것이 목표
    - 문자의 구성요소인만큼 `UNK` 토큰을 줄일 수 있다
    - 단어 사전이 작다 -> 토큰은 많아짐
    - 그러나 토큰당 가지는 의미가 적다. -> 중국어같은 문자는 조금 다름
  - subword base
    - 위 두 알고리즘의 장점 조합
    - 뭉치이지만 자주쓰이고 독립적인 것들은 토큰으로 유지
      - `tokenization` -> `token` + `ization`
- `AutoTokenizer` 를 사용하면 `BertTokenizer` 등의 상위 버전으로 checkpoint 로 부터 모델을 알아서 불러온다
- method
  - `from_pretrained`
  - `save_pretrained`

#### fine-tunning a pre-trained model
##### Processing the data
- `token_type_ids` 한 로우의 데이터 셋에 여러 문장이 포함된 경우를 구분한다
  - `bert-base-uncased` checkpoint 에서의 토크나이저는 제공되나 다른 checkpoint 에서는 해당 checkpoint 의 니즈에 따른다 
  - 모델과 토크나이저는 동일 체크포인트를 사용하므로 해당 속성의 존재여부에 관심가질 필요는 없다
- tokenizer 는 rust로 작성되어 `[dataset].map(tokenizer_function, batched=True)` 를 주면 병렬로 빠르게 처리가 가능하다, `num_proc` - 프로세서 수 조절도 가능한 것 같다
- dataset 은 **apach arrow** 를 사용하여 기본적으로 디스크를 사용하고 토크나이저를 사용하여 올릴대만 메모리가 사용된다
- dataset 에서 사용되는 `.map` 함수는 단순 맵이 아니라 기존 데이터를 두고 리턴되는 데이터가 추가되는 형태(reduce)인것으로 보인다. (테스트 결과)
- 데이터를 동시에(`.map`) 토큰화를 하면 속도가 향상된다
  - 하지만 이 경우에 각 로우에 해당하는 시퀀스(문자열, 데이터 whatever) 가 다르므로 패딩이 필요하다
  - 패딩을 max_length(전체 시퀀스중의 최대길이) 혹은 model 이 받아들일 수 있는 최대 길이로 커팅된다
  - 여기서 불필요한 패딩이 최악의 케이스로 표준화되면서 퍼포먼스 손실이 일어난다
  - 이를 해결하기 위해서 dynamic padding 이 사용된다
  - dynamic padding(동적 패딩) 은 배치로 토크나이징을 할때 배치사이즈의 맥스로 통일시킨다
  - 전체 데이터셋의 부분집합인 배치별로 패딩길이가 최적화되어 큰폭의 성능 개선이 가능하다
    ```python
    from transformers import datacollatorwithpadding
    
    data_collator = datacollatorwithpadding(tokenizer=tokenizer)
    ```
  - cpu 와 gpu 에서 성능향상이 있으므로 거의 필수적으로 적용되지만 google tpu 에서는 고정 텐서 shape를 요구하므로 구글 코렙등의 환경에서는 사용이 불가능하다
- 요약
  - "glue" dataset 의 8/10 다중 문자열을 가지고 학습되었으며 그 간의 관계를 갖는 데이터셋이다
  - 이를 학습한 모델의 경우 토크나이저는 `token_type_ids` 를 함께 리턴하여 문자열이 여러개임을 구분한다
  - 토큰화는 배치를 사용해서 가속화 할 수 있다.
  - 토큰화는 동적 패딩을 사용해서 가속화 할 수 있다.

#### Fine-tuning a model with the Trainer API or Keras

#### a full training
- accelerator
  - gpu 분산환경에서 작업하기 위해 필요
  - accelerator 를 통해 래핑된 오프젝트로 작업
  - `python` 명령어대신 `accelerator` 를 통해 작업한다

#### Fine-tuning, Check!
- 파트 정리
- Auto{Tokenizer,Model}.from_pretrained([checkpoint]) 를 통해 시작
  - Auto* 를 사용하는 경우 huggingface hub 로 부터 해당 model card 를 읽어들여서 설정을 처리한다
- 그리고 데이터 셋을 불러와서(`from datasets`) 이를 기반으로 `Trainer` 를 생성하고 학습시킨다
  - `TrainerArguments` 는 `Trainer` 를 위해 trainer, evaluation 을 위한  hyperparameters 를 제공한다
- `dataset` 은 *apache arrow* 로 작성되었으며 작업시에만 메모리를 사용한다
  - `map` 메소드
    - 동시 배치 처리가 가능하다
    - 작업은 캐싱된다
- 학습 속도 향상을 위해서 동적 패딩이 사용되며, 이는 batch 안에서의 `max_length` 를 활용하는 방식이다
  - tpu 는 동적 패딩을지원하지 않는다.
- 학습시키면서 평가나 프로그래스를 볼 수있도록 설정이 가능하다 (`evaluator`)
- `accelerator` 를 사용하면 multiple gpu 를 활용한 학습이 가능하다
- `AutoModelForSequenceClassification` 와 같은 모델을 생성하면서 `bert-base-uncased` 와 같은 checkpoint 를 `from_pretrained` 로 불러들이면
  - 워닝이 발생한다
  - pre-trained 모델에서 sequence classification 을 위한 헤드는 버려진다
  - 새로운 헤드가 장착되므로 여기에 학습을 시키면 된다

### 4. SHARING MODELS AND TOKENIZERS

#### The Hugging Face hub  

#### Using pretrained models
- `pipeline` 함수를 사용할때는 task, checkpoint 짝이 맞아야한다
  - 아닌경우 head가 task에 맞지않음으로 정상동작하지 않는다
- `pipeline` 함수가 아니어도 model을 바로 생성할수 있다.
  - `from transformers import ATokenizer, AForMaskedLM` 의 형식
  - 하지만 이건 모델 종속적이므로 추천하지 않고 대신 `Auto*` 형태의 사용을 추천한다
    - `from transformers import AutoTokenizer, AutoModelForMaskedLM`
- 모델 사용할대는 model card(huggingface 모델 페이지의 정보)를 확인해라

#### Sharing pretrained models
- `huggingface-cli` 에 대한 내용
- [[tbd]]

#### Buliding a model card
> 읽어보면 도움이 될만한 내용들
- 모델에 대한 정보
- 한계
- 라이센스
- 학습방법

#### Part 1 completed!
- 여기까지 왔으면 text classification 에서 single, paris 문장에 대한 파인튜닝이 가능해야한다

### 5. THE 🤗 DATASETS LIBRARY
#### Introduction
- 파인튜닝시 데이터셋 사용
  - 로드 from hugging face hub
  - `Dataset.map()` 통한 전처리
  - metric 계산

#### What if my dataset isn’t on the Hub?
- `csv`(`tsv` 포함), `text`, `json`, `jsonl`, `pandas`(`pkl`) 지원
- local / remote file 지원
- from datasets import load_dataset
- 다양한 방법으로 train/test 데이터 분할을 지원한다
- 압축파일도 지원한다
- 경로를 path 대신 url 을 주면 리포트 파일을 로딩한다

#### Time to slice and dice
- 데이터 클린업

##### 샘플링
- `Dataset`, `DatasetDict` 의 메소드
- `Datset.shufffle().select(range(1000))` 을 통해 테스트해볼수있다.

##### 정규화
- 수상한 컬럼이지만 id 처럼 보이는 컬럼이 발견되면 `.unique` 메소드를 통해 id 인지 확인한다
  - id 가 확인되면 `DatasetDict.rename_column` 을 통해서 적합한 이름으로 변경
- `.map` 을 통해 대소문자 통일
- `.filter` 를 통해 `None` 제거
  - [ ] [[@todo]] 걍 제거해버리면 되는건가;
- `html` 코드가 포함된 데이터의 경우 `html` 패키지의 `unescape` 로 변경가능
  - `I&#039;m` -> `I'm`

##### 새 컬럼 생성
- `Dataset.map` or `Dataset.add_column`
- `Dataset.sort` 로 특정 컬럼의 값을 통해 정렬가능
- `.num_rows` 를 통해 카운팅

##### `.map`
- `batched=True`
  - batch 의 기본값은 1000
  - `batched=True` 가 설정되면 map 함수로 들어오는 딕셔너리 인자의 값들은 리스트가 된다
  - 때문에 리스트 컴프리헨션등을 사용해서 한방에 처리해야한다
  - batch 도 가속화되지만 `.map` 함수내에서 사용되는 함수내 리스트 컴프리헨션도 for loop 보다 속도면에서 우월하다
  - 토크나이저를 fast 버전을 사용할 수 없는경우(rust backend 사용 x) `num_proc` 을 조정해서 속도를 높일 수 있다.
  - 내용이 많아서 배치 최적화는 그때가서 읽어봐야할 듯 [[tbd]]
  - 파이썬 멀티프로세싱(`num_proc`) 을 fast tokenizer + batched=True 와 사용하지 말것을 권고

#### 6. tokenizer
- `pipeline` 함수 없이 토크나이징하는 것에 대한 설명 있음

##### Building a tokenizer, block by block
- BIRT
  - pre_tokenizers 들은 compose 가능
  - pre_tokenizers.Whitespace - 공백 및 구두점
    - pre_tokenizers.WhitespaceSplit - 공백에서만 split
    - pre_tokenizers.Punctuation - 구두점에서만 split
  - trainer 사용시에는 스페셜 토큰들을 모두 제공해야한다
  - 이를 가지고 저장([[json]]) 후 불러와서 사용하려는 경우
    - `PreTrainedTokenizerFast` 로 래핑해야하며 이때 스페셜 토큰들은 명시해야한다
      - wrapping 후에는 허브에 올리든 `save_pretrained` 를 통해서 사용가능하다
- BPE(GPT-2 토크나이저)
  - `unk_token` 을 사용하지 않는다
  - GPT-2 는 `normalizer` 를 사용하지 않아 스킵
  - 토크나이저 생성시 `models.BPE()` 로 인자가 들어간다
  - 토크나이저 생성시 `pre_tokenizers.ByteLevel` 프리토크나이저가 사용된다
  - 스페이스는 토큰에 포함
  - 역시 huggingface transformers 에서 사용하려면 wrapping 필요

#### 7. MAIN NLP TASKS
##### Token classification
- `AutoTokenizer` 로 model checkpoint -> tokenizer
- 학습시길 dataset 불러옴
- 학습시킬 데이터를 토큰화 해보면 데이터가 가지고있는 레이블링 데이터(? NER 정보등)의 수와 맞지 않게됨(토큰수가 달라지며)
- 이를 맞추는 작업이 proprocessing 
  - 토큰을 맞추기 위해 스페셜 토큰을 -100 값으로 채우고
  - 배치시에 길이를 맞추기 위해 패딩 채우는 작업
- -100 은 loss 함수에서 무시하게되어 부하를 줄이는 역할도 있음

