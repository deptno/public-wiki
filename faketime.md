# faketime

프로그램 실행시 시간을 강제로 주입한다.

## 설치
```sh
brew install libfaketime
```

## 실행
```sh
faketime 'last friday 5 pm' /bin/date                                                        
faketime '2008-12-24 08:15:42' /bin/date                                                     
faketime -f '+2,5y x10,0' /bin/bash -c 'date; while true; do echo $SECONDS ; sleep 1 ; done' 
faketime -f '+2,5y x0,50' /bin/bash -c 'date; while true; do echo $SECONDS ; sleep 1 ; done' 
faketime -f '+2,5y i2,0' /bin/bash -c 'date; while true; do date; sleep 1 ; done'            
```
