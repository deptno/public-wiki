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
```
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

## 조건문
```
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
### 인자 > 환경변수 > 기본값
```sh 
if [ -n "$1" ]; then
  NS=$1
fi
NS=${NS:="default-ns"}
```

## shell
- [[terminal]]
- [[zsh]]
- [[sed]]
- [[tr]]
