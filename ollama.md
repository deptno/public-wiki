# ollama
> 로컬에서 LLM 실행

```sh
brew install ollama
```

## model 생성
+ https://github.com/ollama/ollama/blob/main/docs/modelfile.md

### 예제
+ https://huggingface.co/MLP-KTLim/llama-3-Korean-Bllossom-8B-gguf-Q4_K_M/discussions/1#664328f8094be67723ba44ad

## 에러
### llama-3-Korean-Bllossom-8B-gguf-Q4_K_M
- [[diary:2024-05-23]]
- 토큰이 세는 듯한 이상동작
+ https://huggingface.co/MLP-KTLim/llama-3-Korean-Bllossom-8B-gguf-Q4_K_M/discussions/1#664328f8094be67723ba44ad
```
# blah blah blah 
{||imStart|}질문{ |imEnd| }
# blah blah blah 
```

### wsl
- [[wsl]] 에서 포트 접속 안되는 문제
```sh
ss -tuln | grep [PORT]
```
  - `127.0.0.1` 으로 되어있는 경우 `0.0.0.0` 으로 설정 필요
  - `sudo systemd edit ollama`
```
[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"
```
  - `sudo systemd restart ollama`

## link
- [[llama]]
- [[huggingface]]
- [[wsl]]
