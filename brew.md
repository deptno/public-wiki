# mac 패키지 매니저

```sh
brew install PACKAGE
brew install --cask PACKAGE # website 의 바이너리 인스톨
brew tap homebrew/cask-fonts # 추가 저장소 지정
arch -arm64 brew install alacritty # 아키텍쳐 지정 eg. m1
brew uninstall PACKAGE
brew --prefix
brew info PACKAGE
brew search PACKAGE
```

## install old verison
+ https://github.com/Homebrew/homebrew-core/commits/master/Formula/k/k9s.rb
1. https://github.com/Homebrew/homebrew-core/commits/master/Formula/[first initial]/[package name].rb 로 접속해서 파일을 `[package name].rb] 로 다운로드한다`
2. `brew install [package name].rb` 를 실행 

## link
- [[macos]]
- [[terminal]]
