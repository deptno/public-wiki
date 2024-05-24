# lvextend

## 용량 부족 상황
```sh 
$ df -h

Filesystem                         Size  Used Avail Use% Mounted on
tmpfs                              3.2G  1.8M  3.2G   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv   98G   93G     0 100% /
```
- 물리적 디스크는 용량이있으나 파티션을 구성할때 작게 되어 확장이 필요한경우

## 사용
```sh
sudo df -h # 파티션 확인
sudo lsblk # 로 보면 다 보임

sudo lvextend -L +10G /dev/mapper/[PARTITION] # 파티션에 10G 추가
sudo lvextend -L 10G /dev/mapper/[PARTITION ] # 파티션에 10G 로 설정
sudo lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv # 최대 용량으로 논리 볼륨 확장

sudo resize2fs /dev/mapper/partition # 설정된 논리 볼륨의 최대사이즈로 파일 시스템 확장
```

## link
- [[lvm]]
- [[ubuntu]]
- [[kubernetes]]
