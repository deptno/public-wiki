# image.nvim
+ https://github.com/3rd/image.nvim
- 터미널 이미지 출력

## 설치
- `imagemagick` 시스템에 설치
  ```sh 
  brew install imagemagick
  ```
  - 설치 후 경로 설정 필요
    - `.zshrc` 에 추가 `export DYLD_LIBRARY_PATH="$(brew --prefix)/lib:$DYLD_LIBRARY_PATH"`
- `lazy.nvim` 사용시 `luarocks.nvim` 추천됨
- [[tmux]] 사용시, 관련 추가 설정 필요
  ```sh 
  :set -gq allow-passthrough on
  ```

## link
- [[neovim-lua-plugins]]
