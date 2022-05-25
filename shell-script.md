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

## shell
- [[zsh]]
