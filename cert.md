## ios
### Certificates, Identifiers & Profiles
- https://developer.apple.com/account/resources/devices/list (Devices)
  등록된 기기만 설치가 가능

### 설치된 인증서 보기
```sh
security find-identity -v -p codesigning
  1) xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx "Apple Development: NAME (xxxxxxxxxx)"
  2) xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx "Apple Development: NAME2 (xxxxxxxxxx)"
  3) xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx "Apple Distribution: Co. (xxxxxxxxxx)"
     3 valid identities found

```

### 에러
- [[fastlane]]
  ```sh
  [16:17:10]: ▸ error: Provisioning profile "match Development com.a.b.c" doesn't include signing certificate "Apple Development: XXXX (xxxxxxxxxx)". (in target 'YYYYYYApp' from project 'YYYYYYApp')
  ```
  keychain(키체인 접근) -> 로그인 -> 관련 인증서 삭제 -> 재빌드


