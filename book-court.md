# book-court

## 정보
+ https://yeyak.seoul.go.kr/web/search/selectPageListDetailSearch.do
- 테니스 공공예약
  - 현재 209개
  - 각 테니스 코트별로 공지사항 팝업이 존재
- [[diary:2024-08-18]] 기준 209개 코트

## 시나리오
### 페이지 요약
|        | 장소이름 | 이용대상 | 코트번호 | 시간구분 | 접수기간 | 이용기간 | 취소기간 | 요금 | 예약방법 | 문의전화 | 선별방법 | 지도 | 접수상태 |
|:------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:----:|:--------:|:--------:|:--------:|:----:|:--------:|
| 리스트 |    o     |    o     |    o     |    o     |    o     |    o     |          |      |          |          |          |      |          |
|  상세  |    o     |    o     |    o     |          |    o     |    o     |    o     |  o   |    o     |    o     |    o     |  o   |    o     |

### 월곡 코트 요약
| 코트 | 평일       | 야간       | 주말       |
| :-:  | :--------: | :--------: | :--------: |
| 1    | o          |            | o          |
| 2    | o          |            | o          |
| 3    | o          | o          | o          |
| 4    | o          | o          |            |

### 예약 링크
- [월곡 평일 1](https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?&rsv_svc_id=S240718001853917092&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=월곡&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=)
- [월곡 평일 2](https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?&rsv_svc_id=S240717235037514282&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=월곡&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=)
- [월곡 평일 3](https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?&rsv_svc_id=S240718000356396218&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=월곡&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=)
- [월곡 평일 4](https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?&rsv_svc_id=S240718002451787670&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=월곡&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=)
- [월곡 야간 3](https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?&rsv_svc_id=S240721214015268157&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=월곡&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=)
- [월곡 야간 4](https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?&rsv_svc_id=S240721214926047494&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=월곡&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=)
- [월곡 주말 1](https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?&rsv_svc_id=S240721215449104237&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=월곡&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=)
- [월곡 주말 2](https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?&rsv_svc_id=S240721215726053356&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=월곡&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=)
- [월곡 주말 3](https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?&rsv_svc_id=S240721215928268420&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=월곡&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=)

### 코트 리스트 저장
+ https://yeyak.seoul.go.kr/web/search/selectPageListDetailSearchImg.do?code=T100&dCode=T108
- 코트에 대한 이미지와 정보를 저장하는 역할
- 현재 오픈 여부
- 예약 기간 등이 포함
- output
  - 코트
    - [ ] 장소이름
    - [ ] 이용대상 - 주로 제한 없음
    - [ ] 코트번호
    - [ ] 시간구분(낮/밤/주말/구분없음)
    - [ ] 접수기간
    - [ ] 이용기간 - 주로 접수기간과 동일

### 코트 상세
+ https://yeyak.seoul.go.kr/web/reservation/selectReservView.do?rsv_svc_id=S231228105044310997&code=T100&dCode=T108&sch_order=1&sch_choose_list=&sch_type=&sch_text=&sch_recpt_begin_dt=&sch_recpt_end_dt=&sch_use_begin_dt=&sch_use_end_dt=&svc_prior=N&sch_reqst_value=
- 지도
- 이용기간
- 접수기간
- 선별 방법(선착순)
- 취소기간(이용일전)
- 이용요금(유료/무료)
- 예약방법(인터넷)
- 문의전화

## link
- [[project]]
