# retrospect | 회고

## 목록
- [[diary:2023-12-31]] 2023
- [[diary:2024-01-25]] [[react-native]] 앱개발 
- [[diary:2024-12-31]] 2024

## 관련 명령어
```sh
# 수정하는 파일 중 존재하는 파일 리스팅
git log --since="2024-01-01" --until="2024-12-31" --pretty=format: --name-only | sort | uniq | xargs -I{} git ls-files --error-unmatch {} 2>/dev/null
```

## link
- [[git]]
