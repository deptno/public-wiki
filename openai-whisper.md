# openai-whisper
> 음석 인식 모델, STT(Speech To Text)

## 실행
```sh 
pipenv --python 3.9
pipenv install openai-whisper
whisper [AUDIO FILE] --language Korean [--model MODEL] [--output_format FORMAT]
```

- 첫 실행시 모델이 없으므로 다운로드시간이 소요된다
- 실행후에 json, txt 등의 output 파일이 생성된다

### option
- `output_format` - 기본값은 `all`
- `model` - {tiny,base,small,medium,large}, 기본값은 small
- `output_dir` - 생성되는 파일의 위치 지정
- `model_dir` - 모델 위치 지정, 다운로드 위치인지는 테스트 안해봄
- `language` - 인식할 언어 한국어는 `ko`, `Korean`
- `initial_prompt` - 상황에 대해 묘사를 해주면 좀 더 번역이 잘되는 것 같기도하다
  - 영어만 인식하는 것으로 보이며 이름 같은 고유명사를 넣어주니 인식했다.

## cuda
- medium 으로 테스트
| 원본 용량 | 5600x 3080+10G | M1Max 64G |
|----------:|---------------:|----------:|
|       1MB |           22초 |   4분16초 |
|     150MB |       10분54초 |           |


## [[apple]] silicon
```sh 
pip install lightning-whisper-mlx
```

## link
- [[python]]
- [[pipenv]]
- [[ai]]
