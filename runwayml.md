# runwayml

## prompt
- 공통
  - 부정키워드, 의문형, 명령문 사용 불가
- gen3
  - 카메라 시점 서술
    - [camera movement]: [scene description]. [additional details].
    - I2V 인경우 화면 설명대시 명확하고 간결하게 움직임을 서술
  - 키워드
    - 광각 + 배경
    - 일물 근접 샷 + 피부 질감
    - 
  - 예
    - 트랜지션
      - Continuous hyperspeed FPV footage: The camera seamlessly flies through a glacial canyon to a dreamy cloudscape.
- gen4
  - I2V 만지원
  - 간결한게 강력
  - 고품질 이미지 사용할 것
  - 프롬프트는 움직임에 집중
  - option
    - Subject Motion (주제 움직임)
    - Camera Motion (카메라 움직임)
    - Scene Motion (장면 움직임)
    - Style Descriptors (스타일 설명어)
  - 요소
    - 대상 움직임
      - `the subject`
      - `he`
      - `she`
    - 장면 움직임
      - 직접 -> 강조
      - 간접 -> 자연스러움
    - 카메라 움직임
      - `Locked`
      - `Hanheld`
      - `Dolly`
      - `Pan`
      - `Tracking`
    - 스타일 명령어
      - motion speed
        - `hyperspeed`
        - `slow-motion` 
      - movement style
        - `live action`
        - `smooth animation`
        - `stop motion`
      - aesthetic style
        - `cinematic`
        - `painterly`
        - `reamlike`


## link
- [[ai]]
