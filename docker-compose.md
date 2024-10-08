# docker-compose
- 여러 종류의 인스턴스를 동시에 띄워야하는경우, eg. server, client
- 각 service([[pod]]) 간에 name 으로 통신이 가능하다 eg. http://db:1111
- 하나의 서버만 재시작도 가능

## usage
- `docker-compose.yml` 파일 작성 후
  ```sh 
  docker-compose up
  docker-compose up --build # docker image 재 빌드 필요시
  docker-compose up --build [service name] # 특정 서비스만 재빌드 후 실행
  ```
- 이미 `docker-compose up` 으로 실행된 상태에서
  - `docker-compose up` 죽은 컨테이너만 재시작
  - `docker-compose up [service name]` 실행시 해당 서비스에 attach
  - `docker-compose up --build [service name]` 빌드 후 재실 후 attach

## example
- mlops 프로젝틀 하면서 설정했던 `docker-compose.yml`
```yaml
services:
  mlflow:
    # 제공되는 이미지 그대로 사용
    image: "ghcr.io/mlflow/mlflow:v2.16.2"
    ports:
      # host:container
      - "5001:5000"
    volumes:
      # host:container
      - "./mlflow:/mlruns"
      - "./mlartifacts:/mlartifacts"
    command: ["mlflow" , "ui" , "--host" , "0.0.0.0"]
  airflow-web-server:
    image: "apache/airflow:2.10.2-python3.10"
    # user 설정
    user: "0:0"
    ports:
      - "5002:8080"
    volumes:
      - "./airflow:/opt/airflow"
    command: ["airflow", "webserver"]
  airflow-scheduler:
    # dockerfile 을 통한 직접 빌드 시
    build:
      context: "."
      dockerfile: "Dockerfile.mlops_airflow"
    # dockerfile 을 통한 직접 빌드 시, 이미지 명이 된다
    image: "mlops_airflow:latest"
    volumes:
      - "./airflow:/opt/airflow"
      - "./src/datasets:/opt/airflow/datasets"
    # service 실행 순서 보장
    depends_on:
      - "airflow-web-server"
    command: ["airflow", "scheduler"]
  api:
    build:
      context: "."
      dockerfile: "Dockerfile.mlops_api"
    image: "mlops_api:latest"
    volumes:
      - "./mlartifacts:/mlartifacts"
    ports:
      - "8000:8000"
```

## link
- [[docker]]
- [[diary:2024-10-08]]
