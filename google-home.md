# google-home

## home-assistant 와 연결
- local google 디바이스를 제외한 나머지는 모두 [[home-assistant]] 에서 연결 후 사용
  - google home mini
  - google nest

## error
### home-assistant
- home-assistant 와 연결해서 디바이스들을 불러오면 조명이 두개씩 불러와지는 이슈
  - philips-hue 조명
    - hue -> hue bridge -> [[smartthings]] -> [[home-assistant]] -> google-home 으로 이어짐
      ```mermaid
      flowchart LR
        hue --> hue-bridge --> smartthings --> home-assistant --> google-home
      ```
    - 일단 2개여도 사용은 무리가 없음
    - 문제는 디바이스가 [[smartthings]] 에 추가된후 이를 google-home 에서 추가로 연결하는 경우 디바이스가 다시 중복으로 생김
    - [[home-assistant]] 의 `entity_config`를 통해 설정 저장이 가능한지 검토 필요
```yaml
google_assistant:
  project_id: PROJECT_ID
  service_account: !include config.json
  report_state: true
  entity_config:
    switch.kitchen:
      name: CUSTOM_NAME_FOR_GOOGLE_ASSISTANT
      aliases:
        - BRIGHT_LIGHTS
        - ENTRY_LIGHTS
    light.living_room:
      expose: false
      room: LIVING_ROOM
```

## link
- [[goolge-assistant]]
- [[home-assistant]]
- [[smarthome]]
