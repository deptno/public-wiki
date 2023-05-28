# rsync

폴더간 동기화, 원격에서도 사용이 가능

```sh
rsync -avh source_dir dest_dir
rsync -avh source_dir/* dest_dir

rsync -avz -e ssh /path/to/source-folder user@remote-server:/path/to/destination-folder # remote
```

-a 원본 상태 유지
-v verbose
-h human readable
-z zip

## related
- [[terminal]]
- [[cp]]
