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

## link
- [[python]]
- [[pipenv]]
- [[ai]]
