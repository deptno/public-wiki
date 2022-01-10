# setup terminal

[[macos]]

## brew
> 애플실리콘(M1) 프로세서를 사용하는 경우에는 `brew` 대신 `arch -arm64 brew` 를 사용한다.

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile\
    eval "$(/opt/homebrew/bin/brew shellenv)"
brew install neovim
brew install hstr 
brew install alacritty
brew install tmux
brew install bottom
brew install exa
brew install fzf
brew install gpg
brew install task
brew install universal-ctags

brew tap homebrew/cask-fonts
brew install --cask font-hack-nerd-font
brew install --cask google-chrome
brew install --cask finicky
brew install --cask webstorm
brew install --cask android-studio
brew install --cask notion
brew install --cask blender
```

## python3

```
python3 -m pip install --upgrade pip
```

## tmux
```sh
git clone --depth=1 https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

> fork 가 존재하는데 .gitconfig 설정상의 이슈로 ssh 셋업이 먼저 이루어져야함

## ohmyzsh
```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k\
git clone --depth=1 https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

## xcode
```sh
xcode-select --install
```

## rust
```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## .dotfiles
```sh
git init
git remote add origin https://github.com/deptno/.config.git
get fetch origin
git switch master
git branch --set-upstream-to=origin/master master
git submodule update
git submodule foreach git switch master
```

## ssh
### ssh@github
#### generate
```sh
ssh-keygen -t ed25519 -C "deptno@gmail.com"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
pbcopy < ~/.ssh/id_ed25519.pub # github 등록
```
#### copy from remote ssh
```sh
scp USER@HOST:/path/to/remote/pub.key ~/.ssh/
```

### ssh@alias
```sh
$ vim ~/.ssh/config
host ALIAS
  hostname HOSTNAME_OR_IP
```

## ssh-copy-id
### ssh@publickey
```sh
ssh-copy-id -i ~/.ssh/id_ed25519.pub HOSTNAME
```

## gpg
> password 등록시에는 `GPG_TTY=$(tty)` 가 필요, 외에도 tmux 와 다소 충돌이 있어서 사용하지 않기로 함
```sh
gpg --full-generate-key # RSA(0)
gpg --list-secret-keys
gpg --armor --export IDIDIDIDIDIDIDIDIDIDIDIDIDIDIDIDIDIDIDID | pbcopy # github 등록
```

## vim
### plugin@builtin package manager
#### neovim
```sh
mkdir -p .local/share/nvim/site/pack/_undefined/start # neovim

git clone -b dev --single-branch git@github.com:vimwiki/vimwiki
git clone --single-branch git@github.com:mhinz/vim-startify
git clone --single-branch git@github.com:preservim/tagbar
git clone --single-branch git@github.com:neovim/nvim-lspconfig
git clone --single-branch git@github.com:rust-lang/rust.vim
git clone --single-branch git@github.com:simrat39/rust-tools.nvim
git clone --single-branch git@github.com:hrsh7th/nvim-cmp
git clone --single-branch git@github.com:dracula/vim

git clone --single-branch git@github.com:tpope/vim-fugitive
git clone --single-branch git@github.com:tpope/vim-rhubarb
git clone --single-branch git@github.com:tpope/vim-surround

git clone -b main --single-branch git@github.com:glepnir/galaxyline.nvim
git clone --single-branch git@github.com:yamatsum/nvim-nonicons
git clone --single-branch git@github.com:kyazdani42/nvim-web-devicons
```

#### vim8
```sh
mkdir -p .vim/pack/_undefined/start
cd .vim/pack/_undefined/start
```

#### zshrc 수정
```sh
mkdir -p /usr/local/bin
export PATH=$HOME/bin:/usr/local/bin:$PATH
```
명령어를 덮어쓰기 위해서 필요한 디렉토리를 생성한 후 path를 추가한다.

```sh
ln -s nvim bin/vi
ln -s nvim bin/vim
```
[[path|/usr/local/bin]] 를 먼저 둠으로써 [[vi]], [[vim]] 등을 [[neovim|nvim]] 로 대체하여 사용한다. 
[[path|.gitconfig]] 등에서 사용하는 editor 설정은 alias 로는 작동하지 않기 때문.

### wiki
```sh
git clone git@github.com:deptno/deptno.github.io.wiki
git clone git@github.com:deptno/wiki # private
```

### ruby
시스템 루비(2.6.8 로가정)를 그대로 사용하고 [[env|$GEM_HOME]] 만 수정한다.

[[chruby]] 를 사용하고 있지 않은 경우 디렉토리 생성이 필요할 수 있다.
```sh
mkdir -p ~/.gem/ruby/2.6.8
```

```sh
echo "export GEM_HOME=~/.gem/ruby/2.6.8" >> ~/.zshrc
source ~/.zshrc
```

[[react-native]] 프로젝트를 진행하는 경우는 아래와 같은 커맨드로 이어진다.
```sh
gem install bundler
bundle install
bundle exe pod install
```

### 설정
- [[webstorm]]

## related
- [[vim]]
- [[neovim]]
- [[macos]]
- [[ruby]]
