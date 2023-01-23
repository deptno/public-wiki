# forward-auth

user -> ingress -> backend service

user -> ingress -> forward auth service
                <- forward auth service
                -> backend service 
                
- X-Forward-[] 헤더들과 cookie 를 가지고 인증을 앞단에서 선 처리

- traefik tutorial
  + https://geek-cookbook.funkypenguin.co.nz/docker-swarm/traefik-forward-auth/


https://radarr.example.com/_oauth
https://radarr.example.com/_oauth

## relatead
- [[traefik]]
- [[ingress]]
