  - [ ] [[../openclaw]]
    + [[diary:2026-04-01]]
    + https://m.blog.naver.com/ryurime88/224191974863
      - 여기잘 정리됨
    - quickstart
    - chatgpt codex gpt5.4, oauth
      - 로그인은 다른 호스트에서 하고 callback url만 복사해서 해당 호스트에서 실행하면 인증 완료됨
    - discord
      - oauth2 - 관련 문서 참조
      - bot - 관련 문서 참조
      - allow list
        - 채널 몰라서 일단 패스후 설정예정 `~/.openclaw/openclaw.json`
      - skill
        - apple-notes
        - apple-reminders
        - blowwatcher
        - clawhub
        - gh-issues
        - github
        - model-usage
        - openai-whisper
        - openhue
        - peekaboo
        - session-logs
        - summarize
        - video-frames
      - pnpm
        - tmux
      - no api key
      - enable all hooks
      - hatch in tui
    - trouble shotting
      - disconnected (1008): pairing required
        + https://github.com/openclaw/openclaw/issues/4531
      - approve 를 본컴에서해줭함 명령마다
      - control ui 를 해당 호스트에서만 해야함
        - lan으로 확장하려했더니 ws때문에 https 이슈가 있어보임
        - reverse proxy 설정 필요
          - 일이 커지고, 외부접근을 하려던 목적이 아님, vps안에있어야하는데 인증서 발급하려면 외부접근열어야하는 이슈
        - origin not allowed
          + https://github.com/openclaw/openclaw/issues/29809
        - control ui requires device identity
          - local net 안에서 사용할꺼라 걍 우회
          + https://github.com/openclaw/openclaw/issues/32473#issuecomment-4106981960

