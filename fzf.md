# fzf

+ https://github.com/junegunn/fzf
+ https://fortes.com/2022/make-git-better-with-fzf/

## syntax
| Token     | Match type                 | Description                          |
| --------- | -------------------------- | ------------------------------------ |
| `sbtrkt`  | fuzzy-match                | Items that match `sbtrkt`            |
| `'wild`   | exact-match (quoted)       | Items that include `wild`            |
| `^music`  | prefix-exact-match         | Items that start with `music`        |
| `.mp3$`   | suffix-exact-match         | Items that end with `.mp3`           |
| `!fire`   | inverse-exact-match        | Items that do not include `fire`     |
| `!^music` | inverse-prefix-exact-match | Items that do not start with `music` |
| `!.mp3$`  | inverse-suffix-exact-match | Items that do not end with `.mp3`    |

## env
```sh 
export FZF_DEFAULT_OPTS='--height 40% --layout=reverse --border'
export FZF_DEFAULT_COMMAND='fd --type f'
```

## usage
```sh 
git log --graph --date=short --pretty=format:'%cd\t%h\t%d\t%aL\t%s' --abbrev-commit | \
fzf --delimiter '\t' --preview 'git show {2} | delta' --bind 'ctrl-d:become(git diff {2} @)' --bind 'enter:become(git show {2} @)'
fzf --delimiter '\t' --preview 'git show {2} | delta' --bind 'ctrl-d:become(git diff {2} @)' --bind 'enter:become(git show {2} @)'
```

### bind
```sh 
# --bind [key]:[fzf-command](shell-command)
--bind 'ctrl-r:reload(ps -ef)'
--bind 'ctrl-/:toggle-preview'
--bind 'ctrl-y:execute(echo -n {2..} | pbcopy)+abort'
--bind 'ctrl-y:execute-silent(echo -n {2..} | pbcopy)+abort'
--bind 'ctrl-d:become(git diff {2} @)'
--bind 'ctrl-/:change-preview-window(50%|hidden|)'

# 한번에 정의
--bind 'enter:become(vim {}),ctrl-e:become(emacs {})'
# with --multi
--multi --bind 'enter:become(vim {+})'
```

## link
- [[terminal]]
- [[vim]]
- [[git]]
- [[lazygit]]
