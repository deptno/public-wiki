# rust

## 공부
- [[node-to-rust]]

## 정리 필요

### module|모듈
```rust
// https://doc.rust-lang.org/rust-by-example/mod/visibility.html
// 상위 모듈(parent or ancestor module) 에만 노출
pub(in create::my_mod) fn private_function() {
    println!("private_function");
}
// == private fn
pub(self) fn function_name() {
    println!("function_name");
}
// parent
pub(super) fn function_name() {
    println!("function_name");
}
// crate
pub(crate) fn function_name() {
    println!("function_name");
}
```

모듈의 구조는 파일구조와 같다.
`mod a;` 선언된 경우 `a.rs` 혹은 `a/mod.rs` 파일을 참조한다.

### crete
컴파일 단위

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

### 속성 attribute
- #![allow(dead_code)] - 안쓰는 코드 경고 하지 않음
- #![allow(unused)] - 안쓰는 변수 경고 하지 않음
- #[allow(non_camel_case_types)] - 경고 안함

### use
```rust
use std::io;
```

### type|타입 
- primitive type
  - bool
  - char
  - integer
    - **2023-08-27** 명시적 overflow 핸들링(release 기본은 `wrapping_`)을 위해 아래 메소드를 사용할 수 있다.
      - wrapping_* - maximum value + 1 == minimum value
      - checked* - Option<T>
      - overflowing_* - (value, is_overflow)
      - saturating_* - 한계를 넘어가게 되면 maximum or minimum 값을 리턴
  - float
  - unit - 튜플이지만 primitive type 이다.
- compound type
  - array
  - slice, array 와는 다르게 컴파일타임에 length 를 알 수 없다.
  - tuple
  - struct
  - enum
  

### casting|캐스팅 
- i <-> u 정수간의 변환은 해당 값을 표현하는 형식에 따라 그대로 적용된다.
- floating <-> integer 변환은 floating 변수가 표현하는 근사값을 내림하여 정수 변환한다. `saturating cast`
  - https://doc.rust-lang.org/rust-by-example/types/cast.html

primitive 타입은 cast 를 통해서 변경된다.
custom 타입은 trait `From` 이나 `Into` 를 통해서 변경된다.

### 특성|trait 
- Copy
  - copy trait 이 구현되어 있는 경우, 변수 대입 이후에도 유효하다.
  - drop trait 이 구현되어 있는 경우, copy trait annotation 을 할 수 없다.
  - copy 가능한 타입
    - 정수형 타입
    - 부동 소수점 타입
    - bool
    - char
    - copy 가능한 타입으로만 묶인 튜플
#### 형변환
- From
  - String::from("hello") 와 같이 from 을 구현하여 타입 변환을 가능하게 한다.
- Into
  - From 의 짝으로 필요한 시점에 From 을 호출하도록한다.  
  - `into` 메서드를 호출 하는 순간 대입할 변수의 타입에 맞춰서 `from` 을 역으로 호출한다.
  ```rust
  let x: i32 = 5;
  let y: u32 = x.into(); // u32::from(x) 와 같은 표현
  ```
- TryFrom
  From 과 같으나 실패할 수 있는 경우에 사용된다. 따라서 리턴 타입은 Result<T, E> 이다.
- TryInto
  TryFrom 의 짝으로 사용된다.
  ```rust
  x.try_into()
  ```
- ToString
  - std::fmt::Display 을 구현하면 `to_string` 호출시 이를 통해 `String` 으로 변환된다.
- FromStr
  - `parse` 메서드 호출시 해당 타입에 구현되어있는 `FromStr` 을 통해 변환된다.
  ```rust
  let parsed: i32 = "5".parse().unwrap();
  let turbo_parsed = "10".parse::<i32>().unwrap();
  ```
- 


### ownership|소유권
#### & 참조
참조는 소유권을 갖지 않는다.
- 불편 참조는 여러개가 가능하다.
- 가변 참조는 하나만 가능하다.
- 불변 참조가 존재하는 경우 가변 참조는 불가능하다. -> 값이 변경되면 불변 참조의 값도 변경되어 물결성이 깨지기 때문

#### 타입 변환
&String[..] === &str

#### impl
- enum
  - 첫인수로 &self https://doc.rust-lang.org/rust-by-example/custom_types/enum.html
  - 첫인수로 self https://doc.rust-lang.org/rust-by-example/custom_types/enum/testcase_linked_list.html
#### ref 
`&` borrow 와 같다.
```rust
  let c = 'Q';
  let ref ref_c1 = c;
  let ref_c2 = &c;
```
destructure 를 통해서 변수를 가져오는 경우에도 소유권은 이동된다. 
`ref` 키워드를 prefix 로 사용하면 소유권 이동을 막을 수 있다. stack value 에서는 효과가 없다.

### variable|변수 
```rust
let x = 5; # 불변
let x = 5.0; # 이전 변수는 가려진다(shadowing), 이전 타입 무시 가능. 사용되지 않는 변수는 컴파일러가 제거한다.

const CONSTANT: u32 = 100_000; # 항상 타입 지정

let mut y = String::new(); # 가변
y = 1; # 타입 에러
```

### function|함수 
```rust
fn main() {
    println!("Hello, world!");
}
fn get_name() -> String;
pub fn get_name<T: AType>(a: T) -> T;
```

- 연관 함수 associated : 타입에 연관
- 메서드 method : 인스턴스에 연관
  - sugar
    - stack : (&self) : (self: &Self)
    - heap : (self) : (self: Self)
    stack, heap 에따라 method 의 인자 타입이 달라지는지는 확인필요 [[@todo]]

#### 클로저 closure
인자는 기본적으로 참조 빌리기(& borrow) 시작하여 필요성이 요구될 때만 더 낮은 단계로 간다.
https://doc.rust-lang.org/rust-by-example/fn/closures/capture.html

non-copy 타입에 대해서 클로저를 선언하면 기본적으로 &로 참조된다.  
하지만 mutable 행동(std::mem::drop 등을 사용)을 취하면 `move` 된다.  
혹은 명시적으로 `move` 를 할 수 있다.

`move` 된 이후는 소유권이 이동되었기 때문에 캡쳐된 변수를 더이상 사용할 수 없다.

##### 입력 파라메터로서의 클로저 as input parameters
3가지 trait 중 하나로 표현되어야 한다.
- Fn (&T) - capture by reference
- FnMut (&mut T) - captures by mutable reference
- FnOnce (T) - captures by value

FnOnce(via `move`) 는 Fn, FnMut 의 superset 이므로 언제나 사용이 가능하지만 상황에 맞는 가장 큰 제약을 주어야한다.

#### 구문
결괏값이 없다. 세미콜론으로 끝난다.

#### 표현식
결괏값이 있고 때문에 대입이 가능하다. 세미콜론으로 끝나지 않는다. `함수 리턴시 주의`
- if 는 세미콜론으로 끝나는듯.
- loop 도 break 문과 함께 값 리턴이 가능

### 주석 comment
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
- cfg!
- print!
- println!
- eprint!
- eprintln!

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

#### std::str
```rust
fn main() {
    let raw_str = r"Escapes don't work here: \x3F \u{211D}";
    println!("{}", raw_str);

    let quotes = r#"And then I said: "There is no escape!""#;
    println!("{}", quotes);

    let longer_delimiter = r###"A string with "# in it. And even "##!"###;
    println!("{}", longer_delimiter);
}
```
토큰으로 사용될 `#` 갯수는 제한이 없음

#### std::collections:HashMap

#### std::collections:HashSet
HashMap<T, ()>
##### methods
- union
- difference
- intersection
- symmetric_difference - xor

#### std::rc::Rc  
여러 소유권이 필요한 경우를 위해 reference count 사용된다.
```rust https://doc.rust-lang.org/rust-by-example/std/rc.html
use std::rc::Rc;

fn main() {
    let rc_examples = "Rc examples".to_string();
    {
        let rc_a: Rc<String> = Rc::new(rc_examples);
        {
            let rc_b: Rc<String> = Rc::clone(&rc_a);
            // Two `Rc`s are equal if their inner values are equal
            println!("rc_a and rc_b are equal: {}", rc_a.eq(&rc_b));
            
            // We can use methods of a value directly
            println!("Length of the value inside rc_a: {}", rc_a.len());
            println!("Value of rc_b: {}", rc_b);
            println!("--- rc_b is dropped out of scope ---");
        }
    }
}
```
#### std::rc::Rc  
atomic refernece counted, 

#### std::sync::mpsc
multiple producer single consumer
producer(tx) 가 여럿인 것은 허용된다.

```rust
use std::sync::mpsc::{Sender, Receiver};

let (tx, rx): (Sender<i32>, Receiver<i32>) = mpsc::channel();
let thread_tx = tx.clone();
```
```rust
tx.send(1).unwrap(); // non-blocking send
rx.recv().unwrap(); // blocking recv
```

#### std::path::Path
- posix::Path
- windows::path
상황에 맞게 사용된다.
`to_str` 메서드는 utf-8 이 아닌경우 실패할 수 있다(Option).

#### std::fs::File 
#### std::process::{Command, Stdio}
#### std::Child
실행중인 자식 프로세스의 핸들을 노출한다.
- stdin
- stdout
- stderr
```rust
let process = match Command::new("wc")
                            .stdin(Stdio::piped())
                            .stdout(Stdio::piped())
                            .spawn() {
    Err(why) => panic!("couldn't spawn wc: {}", why),
    Ok(process) => process,
};
```
#### std::os::unix
#### std::env
- env::args
#### foreign function interface : ffi
```rust
#[link(name = "mylib")]
extern {
  fn a(z: Complex) -> Complex;
}
```
C 바인딩을 지원, `unsafe`

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

### ?
`FromResidual` 이 구현된 타입에서 사용 가능, 제공되는 타입으로는 아래가 있음
- Result<T, E>
- Option<T>

함수 결괏값으로 Error가 리턴되면 `해당 함수`를 `호출하는 함수`에서 리턴 처리한다.

### dyn Trait, Trait Object

### smart-pointer|스마트 포인터
- Box<T>: usize 만 스택에 남기고 heap 에 저장한다.  
- Rc<T>: 다중 소유권을 허용한다. Rc.clone()
- Arc<T>: Rc의 thread save 버전
- Mutex<T>: 공유 메모리에 접근을 제어한다.

### array, slice, vec
- arrray: [u8; 3] primitive 타입 fixed length
- slice: &[..] array의 참조로 존재, ㅏㅏㅏk
- vec: growing array, 항상 힙에 저정된다.

### question
- [[@todo]] match { Err(ref e) => ... }, ref를 붙이는 이유
- iter vs into_iter

장
---

## 용어
- 프레류드 (Prelude)  
  모든 러스트 프로그램에 정의된 특별한 영역으로 해당 프로그램 또는 크레이트가 외부로 노출하는 타입을 명시한 모듈

## link
- [[rust-vim]]
- [[book/the-rust-programming-language|러스트 프로그래밍 공식 가이드]]
- [[valgrind]]
- [[functional]]
- cargo
