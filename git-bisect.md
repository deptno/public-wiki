# git-bisect

good 과 bad 를 마킹한뒤 이 사이를 이진 검색으로 계속해서 좁혀간다.  
script 의 성공과 실패 여부를 판단 할 수 있도록 작성되어야한다.

- https://git-scm.com/book/ko/v2/Git-도구-Git으로-버그-찾기

```sh
git bisect start
git bisect bad
git bisect good [sha1 or tag 되는 버전]
git bisect reset # 취소시
git bisect run ./script.sh # 성공 실패를 통해서 문제가 발생한 커밋을 찾아준다
git bisect visualize ---oneline # 마킹과 함께 로그를 보여준다.
```

## bisect run 의 결과
```sh
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx is the first bad commit
commit xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Author: xxxxxxxxxx <xxxxxxxxxxxxxxxxxxx@users.noreply.github.com>
Date:   Fri Mar 11 00:00:00 2022 +0900

    log

 ...xxx.tsx   |  11 -
 ...xx.tsx    | 305 +++++----------------
 2 files changed, 68 insertions(+), 248 deletions(-)
bisect run success
```

`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx is the first bad commit` 을 보고 에러를 추정할 수 잇다.


## link
- [[git]]
