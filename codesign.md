# codesign

[[@todo]] description

alacritty 빌드 이후 실행시고 퍼미션이 필요한 디렉토리([[path|~/Documents]]) 등에 진입시  
퍼미션이 저장되지 않고 계속적으로 물어보는 이슈를 해결했다.

```sh
codesign --remove-signature /Applications/Alacritty.app && \
sudo codesign --force --deep --sign - /Applications/Alacritty.app
```

## related
- [[macos]]
- [[alacritty]]
- [[rust]]
