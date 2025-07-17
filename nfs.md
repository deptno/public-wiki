# nfs | Network File System

ReadWriteMany

volume 과 달리 filesystem 은 RWM 가능, 여러 곳에서 마운트

## usage
```sh
mount -t nfs [ip]:[volume/path] [local/mount/path]
umount [local/mount/path]
```

## port
- tcp 2049
- udp 111

- 이와 별개로 connection 이 맺어지는 경우에는 random connection(1024-65535) 으로 이해했고 때문에 [[kubernetes]] 에서 external service 화 하기가 어려움
- 위의 의견은 StorageClass 없이 pod 에서 직접 붙는 경우로 가정함

## server
- `/etc/exports` 수정
- `sudo exportsfs -ra` 로 적용, re-export all

## link
- [[terminal]]
- [[mount]]
- [[umount]]
- [[kubernetes]]
- [[synology]]
