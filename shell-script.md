# shell-script|쉘스크립트

- https://jhnyang.tistory.com/146

```sh 
set -e # error 가 나면 스크립트 에러와 함께 종료시킨다
some error command || true # 해당 커맨드는 에러지만 무시할 수 있다
```

## example
```sh
function fx() {
  local first=$1
  local sencod=$2
  
  echo $1
}
array_data=(
  "a"
  "b"
  "c"
)

for item in "${array_data[@]}"
do
  echo fx $item
done

for item in {1..5}

# 삼항 연산자
TAG=$([ "$NS" = "production" ] && echo "latest" || echo "dev")
```

문자열 안에서 변수를 참조할 때 문자열은 홋 따옴표가 아닌 쌍따옴표여야 한다
```sh
touch script.sh
chmod +x script.sh
cat <<EOF > script.sh
echo $1
echo "helll $1"
EOF
$ script.sh arg1
arg1
arg1
```

## 할당
```sh
#!/usr/bin/env bash

VAR=${1:-default_value}
VAR=${1:+laternative_value}
VAR=${1:?error messaage if not set}
```

이외에 `:-` 와 비슷한 `:=` 가 있다.
변수값 대신 default 값을 제공하는 것은 동일하지만 변수 할당도 동시에 이루어진다

```sh 
VAR=

echo "${VAR:-default_value}"  # default_value
echo $VAR                     # 
echo "${VAR:=default_value}"  # default_value
echo $VAR                     # default_value
```

## 추출
### parameter expansion
`VAR%%@*`
VAR 변수값 뒤에서부터 `@`를 찾을때까지 문자 제거

```sh
email="a@b.com"
username=${email%%@*}

echo "username: $username"  # 출력: username: a
```

## 조건문
```sh
if [ ! -z "$ENV_VAR"] # 혹은 [ -n "$ENV_VAR"]
then
  echo $ENV_VAR
elif
  echo "empty"
fi
```

## 환경변수
- IFS | internal field separator
  - 어떤 결과를 가지고 `for x in $MULTILINE_VALUE` 형태의 코드를 작성하게 되면 공백마다 변수 `x` 에 들어가게된다
  - 이때 라인을 구분자로 쓰고 싶다면 `IFS=$'\n'` 형태로 입력해주면된다

```sh 
IFS=$'\n'
for script in $(cat shell_script_lines.sh); do
  $script;
done;
```

## 인자
- `$?` 은 최근 shell 의 결괏값(err_code) `0` 인경우 문제 없이 종료된 케이스
- 최근 값 저장이라는게 버그 프룬이므로 아래와 같이 저장된 형태로 사용한다
```sh 
echo 'save exit code'
latest_result=$?

if [ $latest_result -eq 0 ]; then
  # do something
fi
```

### 인자 > 환경변수 > 기본값
argument 는 `$1` 부터
```sh 
if [ -n "$1" ]; then
  NS=$1
fi
NS=${NS:="default-ns"}
```

## [[tip]]
### [[kubernetes]] 환경을 그대로 사용하기 위한 환경변수([[env]]) 주입
- 로컬 스크립트와 배포되었을때 참조하는 yaml 간의 격차 해소를 위해 yaml 을 기준으로 주입한다
- [[yaml]] 파일을 기준으로 해도 되고 실제환경의 yaml 을 가져와도 무방
```sh 
echo $NS
container_envs=$(
  cat ../kubernetes/ns/$NS/$NAME/dp-$NAME.yaml \
    | yq -ojson \
    | jq -r '.spec.template.spec.containers[0].env | map(select(.value)) | map(.name + "=" + .value) | .[]'
)
container_secrets=$(
  cat ../kubernetes/ns/$NS/$NAME/dp-$NAME.yaml \
    | yq -ojson \
    | jq -r '.spec.template.spec.containers[0].env | map(select(.valueFrom)) | map(.name + "=$(kubectl get secret -n $NS " + .valueFrom.secretKeyRef.name + " -ojsonpath=\"{.data." + .valueFrom.secretKeyRef.key + "}\" | base64 -d)") | .[]'
)

IFS=$'\n'

for s in $container_envs; do export $s; done
for s in $container_secrets; do eval "export $s"; done
```

#### 사용 예
- `namespace` 를 `$NS` 로 주입받고 그 환경으로 실행한다 로컬에서 portforward 등을 통해 이름이 달라지는 내부 dns 명을 사용한다면 해당 부분은 추가적으로 아래 `export` 를 통해서 덮어 쓴다

### [[kubernetes]] 에 권한이 있는지 미리 체크
- [[kubernetes]] 에 배포를 한다던지 할때 권한을 미리 체크한다
- `set -e` 옵션을 통해서 에러 발생시 스크립트를 중단하도록
```sh 
set -e

PERMISSION="$(kubectl auth can-i -n $NS create svc/portforward)"
if [[ "no" == $PERMISSION ]]; then
  echo "no permission"
  exit 1
fi
if [[ "error: You must be logged in to the server (Unauthorized)" == "$PERMISSION" ]]; then
  echo "not authorized"
  exit 1
fi
set +e
```

#### 사용 예
- [[dockerfile]] 을 이미지 모두 생성 뒤에 푸시를 못하는 불상사를 앞단에 끊어낸다
- 로컬에서 사용시 디비등 portforward 등이 필요한 경우 이에대한 권한을 미리 체크한다

## link
- [[terminal]]
- [[zsh]]
- [[sed]]
- [[tr]]
- [[kubectl]]
