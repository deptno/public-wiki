# shell-script|쉘스크립트

- https://jhnyang.tistory.com/146

## example
```sh
function fx() {
  local first=$1
  local sencod=$2
  
  echo $1
}
array_data = (
  "a"
  "b"
  "c"
)

for item in "${array_data[@]}"
do
  echo fx $item
done
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

## shell
- [[terminal]]
- [[zsh]]
- [[sed]]
- [[tr]]
