# proguard
- bytecode 에 대한 tree shaking, minify 를 행하는 것으로 추측

## 벤치마크
- [[react-native]] 초기 프로젝트에서 측정
| option                    |      MB |
|---------------------------|--------:|
| debug                     |  150.91 |
| release                   |   53.16 |
| release+proguard          |   49.86 |
| release+splitted          |  max 20 |
| release+proguard+splitted |  max 17 |

## link
- [[android]]
- [[react-native]]
