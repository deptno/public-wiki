# cargo
rust package manager

## package layout
- https://doc.rust-lang.org/cargo/guide/project-layout.html

## commands
| npm                    | cargo             |                             |
|------------------------|-------------------|-----------------------------|
| init                   | init              | 현재 디렉토리 기준          |
|                        | new               | 디렉토리 함께 생성          |
|                        | run               | build + execute             |
| install                | fetch             |                             |
| install [package_name] | add               | cargo install cargo-edit    |
| uninstall              | rm                | cargo install cargo-edit    |
| install --global       | install           | ~/.cargo/bin                |
| test                   | test              |                             |
| publish                | publish           |                             |
| run [start]            | run --exmaple xxx | [[@todo]] -p [project_name] |
| benchmarks             | bench             |                             |
| build                  | build             |                             |
| -                      | build --release   |                             |
| clean                  | clean             |                             |
| docs                   | doc --open        |                             |
| -                      | check             | tsc --noEmit                |
| -                      | clippy            | eslint                      |

## cargo-edit
명령어 확장 셋
```sh
cargo install cargo-edit
```
### 명령어
```sh
cargo add package_name
cargo rm package_name
```

# related
- [[rust]]
