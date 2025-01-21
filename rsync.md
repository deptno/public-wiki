# rsync
- 폴더간 동기화, 원격 사용 가능
- 양방향 사용가능하나 명령어 당은 단방향

```sh
rsync -avh source_dir dest_dir
rsync -avh source_dir/* dest_dir

rsync -avz -e ssh /path/to/source-folder user@remote-server:/path/to/destination-folder # remote
rsync -avz --exclude=".*" . ssh_alias:/path/to/destination-folder
```

## option
- `-a` 원본 상태 유지
- `-v` verbose
- `-h` human readable
- `-z` zip
- `--progress` 전송 상태 표시

## link
- [[terminal]]
- [[cp]]
