# llama

+ https://til.simonwillison.net/llms/llama-7b-m2

1. meta의 llama 페이지에서 downdoad url 요청
2. github.com 에서 download.sh 실행
  - email 로전달된 링크 삽입
3. llama.cpp clone
  - `pip install -r requirements.txt` 를 하게되면 pytorch 의 cuda 버전에러 뱉음
    - 따로 설치 해서 해결
  - python convert.py [download 받은 llama 모델 폴더]
    - guff 파일 생성됨
  - *optional* `./quantize file.guff 2`
    - 양자화라고하는데 f16 -> int8 로 무언가를 변환하면서 리소스 효율을 상승시킨다
  - `./main [[guff_location.guff]] -p '질의어'`

## link
- [[pipenv]]
