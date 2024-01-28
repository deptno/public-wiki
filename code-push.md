# code-push
> 문서가 혼재되어있어서 react-native-code-push 패키지 문서를 기본으로 보되  
> secret 설정등 빠진 부분은 app-center react native sdk 문서를 참조해야한다

+ https://learn.microsoft.com/ko-kr/appcenter/sdk/getting-started/react-native

## CLI 사용
```sh 
# configuration 배포
# TODO: description 남기도록 업데이트
appcenter codepush release-react --app [owner]/[app] --deployment Dev
# Dev 에서 확인된 배포를 Production 으로도 배포
# TODO: 버전이 안보이는데 위험한거같음 버전 명시해서 배포하는 걸로 추후 업데이트
appcenter codepush promote --app [owner]/[app] -s Dev -d Production
```

## 설정 기록
+ [[diary:2024-01-02]]
+ [[diary:2024-01-16]]

## link
- [[react-native]]
