# rust

## 설치
```sh
curl https://sh.rustup.rs -sSf | sh
```

### 툴 
#### rustup
```sh
rust update
rustup self uninstall
rustup docs --book
```

#### rustc
```sh
rustc --version
rustc main.rs
```

#### cargo
> cargo 는 러스트의 패키지 매니저

```sh
cargo --version
cargo new hello_cargo # git 도 함께 초기화된다.
cargo run # 
cargo build
cargo build --release
cargo check # 빌드보다 빠르다, 컴파일 확인
cargo update # Cargo.lock 을 무시하고 miner 업데이트, mager 업데이트는 파일 자체를 수정한다.
```

```toml
name = "hello_cargo"
version = "0.1.0"
authors = ["deptno <deptno@gmail.com>"]
edition = "2018"

[dependencies]

rand = "0.6.1"
```
[[cargo.toml]], [[toml]] 포맷

## 언어
`cargo new` 를 통해 패키지가 생성되면 src/{main,lib}.rs 파일을 통해 실행파일 혹은 라이브러리 빌드 엔트리를 작성할 수 있다(둘다 가능).

### use
```rust
use std::io;
```

### 변수 variable
```rust
let x = 5; # 불변
let x = 5.0; # 이전 변수는 가려진다(shadowing), 이전 타입 무시 가능. 사용되지 않는 변수는 컴파일러가 제거한다.

const CONSTANT: u32 = 100_000; # 항상 타입 지정

let mut y = String::new(); # 가변
y = 1; # 타입 에러

```

### 함수 function
```rust
fn main() {
    println!("Hello, world!");
}
fn get_name() -> String;
pub fn get_name<T: AType>(a: T) -> T;
```

#### 구문
결괏값이 없다. 세미콜론으로 끝난다.
#### 표현식
결괏값이 있고 때문에 대입이 가능하다. 세미콜론으로 끝나지 않는다. `함수 리턴시 주의`
- if 는 세미콜론으로 끝나는듯.
- loop 도 break 문과 함께 값 리턴이 가능

### 주속
//

### 데이터 타입
#### 스칼라
- integer {i,u}{8,16,32,64,size}
  - 리터럴  
    - decimal - 98_222
    - hex - 0xff
    - octal - 0o77
    - binary - 0b1111_0000
    - byte(u8) - b'A'
- floating point numbers  
  - f64 - default
  - f32
- boolean  
  - true
  - false
- characters  
  utf-8
  - 'z'
  - '불' - 3 bytes

#### 컴파운드 타입
- 튜플
  - (i32, f64, u8) - 다른 타입도 가능
  - a.0 으로 접근 가능
- 배열
  - [i32; 10] - 같은 타입만 가능
  - [3; 5]
  - a[0] 으로 접근 가능

##### 정수 오버플로
타입의 값을 넘어가는 경우 Warpping 타입을 통해 255 -> 0 으로 동작한다. --release 빌드 시에는 명시적으로 선언하지 않으면 자동적으로 선언된다.
#### 참

프렐류드에 포함되지 않는 타입을 사용하려면 `use` 를 사용해야 한다.

### 반복문
```rust
for i in 0..10 {
    println!("{}", i);
}
for i in (1..10).rev() {
    println!("{}", i);
loop {
    println!("loop");
    break;
}
while i < 10 {
    println!("{}", i);
    i += 1;
}
```

### 매크로
```rust
println!("hello, world!");
```
`!` 가 붙은 함수는 매크로다.

```rust
let foo = 5 // 불변
let mut foo = 5 // 가변
```

### 연관 함수, associated function, static method
```rust
String::new
```
`new` associated function 이라고 불리며 타 언어의 static method 와 같다.

### std
```rust
use std::io;
let mut guess = String::new();
io:stdin().read_line(&mut guess).expect("Failed to read line");
let guess: u32 = guess.trim().parse().expect("Please type a number!");

use std::cmp::Ordering;
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}


```
#### 매크로 macro
```rust
println!("hello, world!");
println!("{}", "hello, world!");
println!("{} {}", "hello", "world");
```
### enum
- Result  
  - method  
    - expect(value: &str)
- Ordering

---

## 용어
- 프레류드 (Prelude)  
  모든 러스트 프로그램에 정의된 특별한 영역으로 해당 프로그램 또는 크레이트가 외부로 노출하는 타입을 명시한 모듈

## related
- [[rust-vim]]
- [[book/the-rust-programming-language|러스트 프로그래밍 공식 가이드]]
