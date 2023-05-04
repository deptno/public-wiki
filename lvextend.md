# lvextend

```sh
sudo df # 파티션 확인

sudo lvextend -L +10G /dev/mapper/partition # 파티션에 10G 추가
sudo lvextend -L 10G /dev/mapper/partition # 파티션에 10G 로 설정

sudo resize2fs /dev/mapper/partition # 설정된 파티션 크기의 최대치로 리사이즈
```

## related
- [[lvm]]
