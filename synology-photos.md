# Synology Photos

## migration
### Google Takeout
takeout 서비스를 이용하게 되면 metadata가 json 으로 따로 빠져서 생성되기 때문에 이를 머지하는 과정이 필요하다.

- https://stackoverflow.com/questions/65210140/how-to-merge-json-and-google-takeout-photos-to-get-the-right-dates-back
```sh
exiftool -r -d %s -tagsfromfile "%d/%F.json" "-GPSAltitude<GeoDataAltitude" "-GPSLatitude<GeoDataLatitude" "-GPSLatitudeRef<GeoDataLatitude" "-GPSLongitude<GeoDataLongitude" "-GPSLongitudeRef<GeoDataLongitude" "-Keywords<Tags" "-Subject<Tags" "-Caption-Abstract<Description" "-ImageDescription<Description" "-DateTimeOriginal<PhotoTakenTimeTimestamp" -ext "*" -overwrite_original -progress --ext json .
```
샐행하고자하는 폴더에서 실행한다.

## exiftool
1. Synologo Photo 에서 사용하기 위해서는 https://exiftool.org 에서 다운로드
2. tar 파일을 scp 로 nas에 업로드한다.
3. 압축을 해제한다.
```sh
tar -xf ---.tar .
```
4. 이동 한고 symlink 를 걸어준다.
```sh
sudo mv Image-ExifTool-12.44 /usr/share/applications/
sudo ln -s /usr/share/applications/Image-ExifTool-12.44/exiftool /usr/local/bin
```
5. `exiftool` 로 실행후 실패하는 경우 [[perl]] 이 없는 경우라면 [[Synology]] UI에서 패키지로 이동하여 [[perl]] 을 설치 후 실행한다.

## [error](error)

###  업로드 보류됨
> 공간이 부족합니다

- 개인 사진이 올라가는 곳(homes 폴더 예상)의 용량 제한에 걸린 경우

1. 제어판
2. 공유 폴더
3. `homes` 공유 폴더 편집
4. 고급
5. 공유 폴더 할당량 활성화 수정

## link
- [[synology]]
