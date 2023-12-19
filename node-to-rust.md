# node-to-rust

- https://vino.dev/blog
>  24일 코스로 [[node]] 숙련자들 위한 러스트 가이드

## Day 1: From nvm to rustup
## Day 2: From npm to cargo
| npm                    | cargo             |                    |
|------------------------|-------------------|--------------------|
| init                   | init              | 현재 디렉토리 기준 |
|                        | new               | 디렉토리 함께 생성 |
|                        | run               | build + execute    |
| install                | fetch             |                    |
| install [package_name] | add               |                    |
| uninstall              | rm                |                    |
| install --global       | install           | ~/.cargo/bin       |
| test                   | test              |                    |
| publish                | publish           |                    |
| run [start]            | run --exmaple xxx | [[@todo]]          |
| benchmarks             | bench             |                    |
| build                  | build             |                    |
| -                      | build --release   |                    |
| clean                  | clean             |                    |
| docs                   | doc --open        |                    |
| -                      | check             | tsc --noEmit       |
| -                      | clippy            | eslint             |

npm 과 달리 task runner 를 가지지 않았다. 때문에 여전이 [[makefile]] 이 사용된다.  
저자는 `just` 를 추천
```sh
cargo install just
```

### workspaces & [[monorepo]]
Cargo.toml
```toml
[workspace]
members = [
  "crates/*"
]

[dependencies]
other-project = { path = "../other-project"}
```

#### 추가적인 툴
- cargo-edit - 디펜던시 버전 변경
- [[cargo-workspaces]](cargo ws) - lerna like
- cargo-expand - macro
- tomlq - jq like

## Day 3: Setting up VS Code
core plugin
- rust-analyzer
- vadimcn.vscode-lldb
- bungcip.better-toml
- crates
- search-crates-io

## Day 4: Hello World (and your first two WTFs)
String vs &str

## Day 5: Borrowing & Ownership
## Day 6: Strings, part 1
&str -> String 전환을 위해 .to_owned() 를 사용한다.
- .to_string() 은 Display trait 을 지원하기 위한 메소드
- .into() 은 타겟타입으로의 전환을 위한 From,Into trait 의 구현
## Day 7: Syntax and Language, part 1
array는 컴파일 타임에 길이와 모든 원소가 초기화 되어야한다.
일반적으로 js 매칭되는건 VecDeque 으로 양방향 접근이 가능하다.
- Vec
- VecDeque: 양쪽에서 push, pop 이 가능하다.
## Day 8: Language Part 2: From objects and classes to HashMaps and structs
struct: 알려진 키 셋을 가진 데이터 셋의 경우
map(hashmap): 모르는 키셋 + 동일 타입

```rust
use std::collections::HashMap;

fn main() {
  let mut map = HashMap::new();
  map.insert("key1", "value1");
  map.insert("key2", "value2");

  println!("{}", map.get("key1").unwrap_or(&""));
  println!("{}", map.get("key2").unwrap_or(&""));
}
```
"" 의 타입은 &str
`map.get("key1")` 의 타입은 Option<&&str> 이 된다.
&str 이고 `get()` 의 경우 소유권 이전이 되면 map 에서 해당 value 가 제거되어야하기 때문에 borrowing(&) 이 되야한다.
## Day 9: Language Part 3: Class Methods for Rust Structs (+ enums!)
```rust
struct A {
  color: String
}
imple A {
  pub fn new() -> {
    Self {
      color: "red".to_owned(),
    }
  }
}
```
- `new()` 는 키워드가 아니라 컨벤션이다.
- `#[derive(Debug)]` 구문은 Trait 의 default 구현을 제공한다.
- 함수는 기본적으로 private 이다.
- 첫번째 인자를 self 혹은 &self 로 선언하면 인스턴스 메소드가 된다.
  - self: Self
  - &self: &Self
- self 를 인자로 선언하는 경우 오너쉽을 가지게 되므로 이후 인스턴스를 잃게 된다.
  - 자바스크립의 .map() 처럼 새로운 인스턴스를 만들어서 반환하는 경우
  - 소멸자와 같은 역할을 하는 경우
  - 메소드 체이닝
enums 은 union type 을 처리할 수 있다.

## Day 10: From Mixins to Traits
Trait 은 자바스크립트의 Mixins 이다.
메소드의 집합이라고 생각하도록 한다.

## Day 11: The Module System
- 모든 정의는 private이 default
- trait methods, enum variants 는 default 가 public
- visibility 는 모듈의 것이므로 모듈 내에서는 무관하게 참조가 가능하다.
- pub(create) 를 통해 crate 내부에서 접근 허용
- pub(super) 부모에게만 허용

## Day 12: Strings, Part 2
함수 인자는 &String, String 이 아닌 &str 로한다.
- String 은 Borrow<str> 이 구현되어있어 자동 컨버팅이 된다.
- Display 구현함으로써 `.to_string()` 을 얻을 수 있다.

- `Borrow<str>` - 더 많은 가정 + 실패할 수 있다.
- [[@todo]] `AsRef<str>` - 더 적은 가정 + 실패할 수 없다.

## Day 13: Results & Options
std::prelue 에 선언된 것들은 `use` 선언 없이 사용이 가능하다.
- Some
- None

- `unwrap()` 은 Err(T), None 이 오는 경우 panic 을 발생시킨다.
- `unwrap_or(v)` 실패 없이 v 를 기본값으로 리턴
- `unwrap_or_else(|| {})` 실패 없이 함수를 통한 값을 리턴
- `unwrap_or_default()` `Default` trait 을 구현한 경우에 사용됨

매직 value 대신 Option, Result 를 이용하자.

## Day 14: Managing Errors
서로 다른 타입의 오류를 반환 하는 함수 작성하기
1. Box<dyn Error>
  - Box 는 수명 만큼 존재하는 어떤 값의 주소
  - 아키텍쳐의 비트가 크기가 된다.
  - Result 는 Err 에 제한이 없으므로 Error trait 을 구현하지 않은 오류가 발생하면 실패한다.
2. custom Error type
  - `impl std::error::Error for MyError {}`
  - **supertraits** trait 이 의존하는 trait
  - `From`, `Into`, `TryFrom`, `TryInto` 구현 필요
  - From 은 타겟 타입을 보고 자동으로 Into 를 호출한다.
  - `Try*` 은 실패가 가능하다. 리턴 타입은 `Result` 가 된다.
3. Use crate
  - thiserror - 커스텀 에러, lib 에서 주로 사용
  ```rust
  #[derive(thiserror::Error, Debug)]
  enum MyError {
    #[error("Environment variable not found")]
    EnvironmentVariableNotFound(#[from] std::env::VarError),
    #[error(transparent)]
    IOError(#[from] std::io::Error),
  }
  ```
  - anyhow - 원본 에러를 그대로 노출, bin 에서 사용

## Day 15: Closures
- Fn
- FnMut
- FnOnce - **move** 로 소유권을 잃는 경우

[[@todo]] impl [trait] 은 함수 인자 혹은 리턴 값 이외에는 사용이 불가능하다.
때문에 dyn [trait] 으로 저장이 필요하다.
dyn [trait] 은 unsized 이며 러스트는 이를 좋아하지 않는다.

## Day 16: Lifetimes, references, and 'static
[[@todo]] `T: 'static`, `&'static` 은 다르다
`T: 'static` 은 영원히 지속되는 변수를 의미하지 않는다.

## Day 17: Arrays, Loops, and Iterators
- 배열은 알려진 크기와 초기화되어 있어야한다.
- 자바스크립트의 Array 는 Vec 과 매칭된다.
- VecDeque 는 양쪽에서 입출력이 가능하다.
  - Vec -> vec![]
- `for prop in hashmap.keys() { }` -> `.keys()` 는 임의 순서로 조정이 불가능
- `for i in 0..max { }`
- `while ((data = obj.dowork())) { }`
- `while let Some(data) = obj.dowork() { }`
- `loop { break; }`
- `'outer: loop { break 'outer; }`
- `let value = loop { break "return value"; }`


Iterator, Array, Vec -> `::iter()`
- Iterator 는 lazy 하게 동작 `collect()` 등의 반환 함수를 사용할때 iterator 들의 메소드 들이 사용
- Vec::iter() 로 생성 or Vec::iter_mut() -> Iterator 를 리턴
- Iterator.collect() 로 반환 -> 타겟에 타입을 지정해야함 eg, `let a: Vec<_> = ...`
- Iterator.next() 반복자에서 단일 값을 얻을때 사용
- Iterator.filter() 2중 참조
- Iterator.find() == Iterator.filter().next(), js 와 다르게 find() 를 여러번 호출 가능
- Iterator.forEach() iterator 즉시 **소비**, **일반 루프가 선호**된다.

- Vec|Array.join(",") js 와 동일
- Vec.push() `Vec`만 가능
- Vec.pop() `Vec`만 가능
- VecDeque.push_front() `VecDeque`만 가능
- VecDeque.pop_front() `VecDeque`만 가능

`.collect()` 를 통해 특정 데이터 타입을 반환하지 말고, Iterator 자체를 반환해서 지연평가와 유연성을 확보하도록 한다.

## Day 18: Async
러스트는 러스트의 Promise 인 `Futures` 를 가지고 있다. 그러나 executor, reactor(node 의 event loop) 가 존재하지 않는다.  
이에 대한 폴리필은 아래와 같다.
- tokio
- smol
- async-std

```rust
fn sync_fx() -> String {
}
async fn async_fx() -> String { // impl Future<Output = String>
}
#[tokio::main]
async fn main() {
  let msg = async_fx().await;
}
```
**node** 와 달리 `.await` 을 하기전에는 실행되지 않는다.

비동기 클로저
```rust
let closure = || async {
    println!("hello");
};
```

쓰레드로 전송되어 사용되는 경우 인자에 경계를 주어야한다.
- 'static - 언제사용될지 모르기 때문
- Send or Sync - Threadsafe

## Day 19: Starting a large project
**cargobook** 은 `Cargo.lock` 을 라이브러리에서는 생략하라고 조언함(버전관리에서로 이해)
`./Cargo.toml`
```toml
[workspace]
members = ["crates/*"]
```
`./crates/my-lib/Cargo.toml`
`./crates/cli/Cargo.toml`
```toml
...
[dependencies]
my-lib = { path = "../my-lib" }
```

```sh
cargo run crates/cli
cargo run -p cli
```

test
```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        let result = 2 + 2;
        assert_eq!(result, 4);
    }
}
```
내부 테스트는 이와 같은 방식으로**만** 한다.
tests 디렉토리를 만들어서 외부에서 테스트하는 경우 `pub` 접근지시자로 설정된 경우에만 접근가능하다.

`#cfg(flag)` flag 그로 on/off
`[#test]` unit test -> `cargo test` 로 실행된다.
assert 함수
- assert!
- assert_eq!
- assert_ne!

crate 이름은 하이픈(`-`)을 사용하지만 코드에서 참조시에는 언더바(`_`) 로 참조해야된다.

## Day 20: CLI Arguments & Logging
- structopt
- log
- env_logger

```sh
RUST_LOG=debug cargo run -p cli
RUST_LOG=package_name=debug cargo run -p cli
```

```rust
use structopt::{clap::AppSettings, StructOpt};

#[derive(StructOpt)]
#[structopt(
    name = "name",
    about = "about",
    global_settings(&[
      AppSettings::ColoredHelp
    ]),
)]
struct CliOptions {
    #[structopt(parse(from_os_str))]
    pub(crate) file_path: PathBuf,
}

fn main() {
    let options = CliOptions::from_args();
}
```

```sh
cargo run -p cli -- --help
```
cli 에 인자를 넣어서 실행하려면 `--` 후에 인자를 입력한다.

`#[derivte(StructOpt)]`

## Day 21: Building and Running WebAssembly
에러 형식을 변환하는 방법
1. From, Into, TryFrom, TryInto
2. .map_err
```rust
fn fx() -> Result(Self, CustomError) {}
  #...
  .map_err(|e| CustomError::Something(e))?
}
```

웹어셈블리(WebAssembly, 이하 wa)를 지원하므로 해당 포맷으로 포맷된 파일을 읽어서 기능을 수행 할 수 있다.
wa 는 아직 완벽하지 않다. 아직 표준 스펙이 없으므로 rpc 와 같은 컨셉으로 처리를 진행한다.

- wapc_crate
- wapc
- wasmtime
- wasmtime-proivder
- https://wapc.io

## Day 22: Using JSON
- serde
- serde_json - json
- rmp_serde massage - pack

## Day 23: Cheating The Borrow Checker
멀티 스레드 코드를 작성한다면 쓰레드간 데이터 이동을 위해 아래 두가지 Trait이 존재한다.
- Send - 불변 참조
- Sync - 가변 참조

- Rc - Reference Count 여러 참조가 존재하고 그 참조가 모두 제거됐을때 메모리를 해제한다.
- Arc - Atomic Reference Count, `Rc` 의 `Send` 버전이다.

- Mutex - 한 시점에 하나의 읽고 쓰기만 가능.
- RwLock - Mutex 보다 무겁다. 여러 읽기가 가능하며 한 시점에 하나의 쓰기만 가능하다.

- `parking_lot` - `Mutex`, `RwLock` 보다 빠르며 `Result` 를 리턴하지 않는다.

`Mutex`, `RwLock` 등을 사용할때 락을 얻은 **블록이 사라지면** 락이 자동으로 해제된다.
조심해야할점을 락을 얻은 후에 `.await` 등의 비동기 동작을 하게 되면 불필요한 락을 잡고 있게된다.

Tokio 는 자체 **Sync** crate 를 가지고 있다.

## Day 24: Crates & Tools




## link
- [[node]]
- [[rust]]
- [[monorepo]]
