# code-push
> 문서가 혼재되어있어서 react-native-code-push 패키지 문서를 기본으로 보되  
> secret 설정등 빠진 부분은 app-center react native sdk 문서를 참조해야한다

+ https://learn.microsoft.com/ko-kr/appcenter/sdk/getting-started/react-native

## CLI 사용
```sh 
# configuration 배포
appcenter codepush release-react --app [owner]/[app] --deployment Dev [--description "네비게이션을 포함한 구조 완전 변경"] [-m]
# Dev 에서 확인된 배포를 Production 으로도 배포
# TODO: 버전이 안보이는데 위험한거같음 버전 명시해서 배포하는 걸로 추후 업데이트
appcenter codepush promote --app [owner]/[app] -s Dev -d Production
```

### release-react
- `m` `mandatory` 필수 버전업 으로 지정
- `description` 디스크립션
- `t` `target-binary-version` 업데이트를 받을 버전 지정

## 설정 기록
+ [[diary:2024-01-02]]
+ [[diary:2024-01-16]]

## link
- [[react-native]]
