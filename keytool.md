# keytool

> `keystore` 를 생성 하고 확인하는데 사용

## 사용
```sh
# 생성
keytool -genkey -v -keyalg RSA -keysize 2048 -validity 1000 -keystore [NAME.keystore] -alias [ALIAS_NAME]
# 정보 확인(SHA1, SHA256 등)
keytool -keystore [NAME.keystore] -list -v
keytool -keystore app/debug.keystore -list -v # react-native 기준
```
- [[react-native]] 의 경우 `[PROJECT_ROOT]/android/app` 경로에서 생성할 것
- [[react-native]] 의 경우 프로젝트 생성시 `debug.keystore` 파일은 함께 생성된다

## link
- [[android]]
- [[fcm]]
- [[react-native]]
