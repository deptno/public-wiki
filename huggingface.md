# huggingface
+ huggingface.co

## nlp
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

##### Fine-tuning a model with the Trainer API or Keras


## link
- [[python]]
- [[jupyter]]
- [[practical-deep-learning]]
- [[practical-deep-learning]]
