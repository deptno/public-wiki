# tts
> Text To Speach

https://github.com/coqui-ai/TTS

```sh 
pipenv --python 3.9
pip install TTS
tts --model_name tts_models/multilingual/multi-dataset/xtts_v2 \
   --text "한글말" \
   --speaker_idx "Ana Florence" \
   --language_idx ko \
   [--use_cuda true]
```

## link
- [[ai]]
- [[openai-whisper]]
