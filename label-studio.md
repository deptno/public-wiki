# label studio

## 설치
- [[kubernetes]]
  - `namespace` 생성
  - [[postgresql]]
    - `role` 을 위한 [[kubernetes]] secret 생성
    - role 생성
    - role 권한 부여 [[sql]]
      ```sql
      CREATE USER label_studio WITH PASSWORD 'YOUR_PASSWORD';
      CREATE DATABASE label_studio 

      GRANT ALL PRIVILEGES ON DATABASE label_studio TO label_studio;
      GRANT CREATE SCHEMA public ON label_studio
      ````
  - [[helm]]
    ```sh
    helm repo add heartex https://charts.heartex.com/
    helm repo update heartex

    # values.yaml 수정
    ```
## NER
+ https://labelstud.io/playground?config=%3CView%3E%0A%20%20%3CLabels%20name%3D%22label%22%20toName%3D%22text%22%3E%0A%20%20%20%20%3CLabel%20value%3D%22PER%22%20background%3D%22red%22%2F%3E%0A%20%20%20%20%3CLabel%20value%3D%22ORG%22%20background%3D%22darkorange%22%2F%3E%0A%20%20%20%20%3CLabel%20value%3D%22LOC%22%20background%3D%22orange%22%2F%3E%0A%20%20%20%20%3CLabel%20value%3D%22MISC%22%20background%3D%22green%22%2F%3E%0A%20%20%3C%2FLabels%3E%0A%20%20%3CText%20name%3D%22text%22%20value%3D%22%24text%22%2F%3E%0A%3C%2FView%3E
- input format 은 `text` 속성을 가진 `jsonl` 일 것, 확장자 자체는 `json` 을 지원
 
## link
- [[ner]]
