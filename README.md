# rust_for_java_devs

### Rust Syntax for Java Developers :robot:

- Rust and Java share some similarities (like curly braces and semicolons), but Rust's syntax and paradigms differ significantly. Below is a comparison of key concepts.

__1. Variables and Mutability__

- Java: Variables are mutable by default unless declared `final`.

```java
int x = 5; //mutable
final int y = 10; //immutable
```
- Rust: variables are immutable by default. Use mut to make them mutable.

```rust
let x = 5; // immutable
let mut y = 10; // mutable
y = 15; // allowed
//x = 6; //Error: cannot assign twice to immutable variable
```
---

__2. Functions__

- Java:
  
```java
public int add(int a, int b) {
return a + b;
}
```

- Rust:
  
```rust
fn add(a: i32, b: i32) -> i32{
a + b
}
```

- No return needed for the last expression (Rust uses expressions not statements)
- Type annotations come after the variable name (a: i32)

---

__3. Ownership and Borrowing__

- Rust enforces memory safety without a garbage collector via ownership
- Each value has a single owner
- Ownership can be moved (transferred) or borrowed (references)

_Example: Ownership Transfer_

```rust
let s1 = String::from("hello")
let s2 = s1; // Ownership moves to s2, s1 is no longer valid
//println!("{}", s); // Error: value borrowed here after move
```

_Example: Borrowing (References)_

```rust
let s String::from("hello");
print_string(&s); // Pass a reference (borrow)
println!("{}", s);

```
---

__4. Structs (instead of classes)__

- Rust doesnt have classes but has structs and traits (similar to interfaces).

- Java:
```java
class Person {
private String name;
public Person(String name) {this.name = name;}
public void greet() {System.out.println("Hello, "+ name);}
}
```
- Rust:
```rust
struct Person {
name: String
}

impl Person {
fn new(name: String) -> Self {
Person {name}
}

fn greet(&self) {
println!("Hello, {}", self.name);
}
}

```
---
 __5. Enums and Pattern Matching__
 
 Rusts `enum` is more powerful than Java's (can hold data)
 
 - Java
 
 ```java
 enum Status {ACTIVE, INACTIVE}

 ```
- Rust

```rust
enum Status {
Active,
Inactive,
}

// With data
enum Result {
Ok(i32),
Err(String),
}
```

- Pattern Matching (Like `switch` but better):
  
```rust
match status {
status::Active => println!("Active"),
Status::Inactive => println!("Inactive"),
}
```
---

__6. Error Handling__

- Java: Uses exceptions (`try-catch`).
- Rust: Uses `Result<T, E>` (no exception).

```rust
fn divide(a: i32, b: i32) -> Result<i32, String> {
if b == 0 {
Err(String::from("Cannot divide by zero"))
} else {
Ok(a/b)
}
}

match divide(10, 0) {
Ok(result) => println!("Result: {}", result),
Err(e) => println!("Error: {}", e),
}
```
---

### The _"Rust Way"_ of Programming

Rust is not OOP (though it supports traits, which are similar to interfaces), instead it emphasizes:
1. **Ownership and Borrowing**: Memory safety without GC.
2. **Zero-Cost Abstractions**: High-level constructs compile to efficient machine code.
3. **Explicitness**: Types, mutability and error handling are explicit.
4. **Fearless Concurrency**: Thread safety is enforced at compile time.
5. **Pattern Matching**: Powerful `match` for control flow.

__Key differences from Java__:
  
- No inheritence (traits are used instead).
- No `null` (use `Option<T>`).
- No exceptions (use `Result<T, E>`).
- No global state (everything is scoped).

__Summary__

* Rust is not OOP but it supports structured programming with structs and traits.
* Memory safety is guaranteed at compile time via ownership.
* Syntax is similar to Java but with stricter rules (immutability by default explicit types).

---

### More Rust Specific Features with Practical examples :bulb:

__1. Ownership & Borrowing (Core Rust Concept)__

Rust's memory safety is enforced via ownership rules:
- Each value has a single owner
- Ownership can be moved or borrowed (via references).

_Example: Ownership Transfer_

```rust
fn main() {
let s1 = String::from("hello");
let s2 = s1; // Ownership moves to s2, s1 is now invalid
//println!("{}", s1); // Error: value borrowed here after move
println!("{}", s2); // works
}
```
_Example: Borrowing (References)_

```rust
fn main(){
int s = String::from("hello");
print_string(&s); // Pass a reference (borrow)
println!("{}", s) // s is still valid

let mut s_mut = String::from("mutable");
modify_string(&mut s_mut); // Mutable borrow
println!("{}", s_mut) // New "modified"
}

fn print_string(s: &String) {
println!("{}", s);
}

fn modify_string(s: &mut String) {
s.push_str(" modified");
}
```
---

__2. Pattern Matching__ (`match`)

Rust's `match` is like a supercharged `switch` that can destructure enums, structs and more.

_Example: Matching on Enums_
```rust
enum Coin {
Penny,
Nickel,
Dime,
Quarter,
}


fn value_in_cents(coin: Coin) -> u8 {
match coin {
Coin::Penny => 1,
Coin::Nickel => 5,
Coin::Dime => 10,
Coin::Quarter => 25,
}
}

fn main(){
let coin = Coin::Quarter;
println!("Value: {}", value_in_cents(coin)); // 25
}

```
_Example: Destructuring Structs_

```rust
struct Point {
x: i32,
y: i32,
}

fn print_point(p: Point) {
match p {
Point {x, y:0} => println!("On the x-axis at {}", x),.
Point {x:0, y} => println!("On the y-axis at {}", y),
Point {x, y} => println!("At ({}, {})", x, y)
}
}

fn main() {
let p = Point {x: 5, y: 0};
print_point(p); // "On the x-axis at 5"
}
```
---

__3. Error Handling__ (`Result` and `Option`)

Rust avoids exceptions by using `Result<T, E>` (for recoverable errors) and `Option<T>` (for nullable values).

_Example_: `Result` _for Error Handling_

```rust
use std::fs::File;

fn main() {
let file_result = File::open("nonexistent.txt");

match file_result {
Ok(file) => println!("File opened: {:?}", file),
Err(e) => println!("Error opening file: {}", e),
}
}
```
_Example_: `Option` _for Nullable Values_

```rust
fn divide(a: f64, b: f64) -> Option<f64> {
if (b == 0.0) {
None
} else {
Some(a/b)
}
}

fn main() {
int result = divide(10.0, 2.0);
match result {
let result = divide(10.0, 2.0);
match result {
Some(x) => println!("Result: {}", x),
None => println!("Cannot divide by zero"),
}
}
}
```
---

__4. Traits (_Like Interfaces, but more Powerful_)__

Traits define shared behaviour (similar to Java interfaces but with more flexibility).

_Example: Defining and Implementing a Trait_

```rust
trait Greet {
fn greet(&scif) -> String;
}

struct Person {
name: String,
}

impl Greet for Person {
fn greet(&self) -> String {
format ("Hello, {}", self.name)
}
}

fn main(){
let person = Person {name: "Alice".to_string()};
println! ("{}", person.greet()); //"Hello, Alice!"
}
```

_Example: Trait Bounds (Generic Constraints)_

```rust
trait Area {
fn area(&self) -> f64;
}

struct Circle {
radius: f64,
}

impl Area for Circle {
fn area(&self) -> f64 {
std::f64::consts::PI * self.radius * self.radius
}
}

fn print_area<T: Area>(shape: T) {
println!("Area: {}", shape.area());
}

fn main(){
let circle = Circle {radius: 5.0};
print_area(circle); // Area: 78.53981633..
}
```
---

__5. Lifetimes (Borrow Checker)__

Lifetimes ensure that references are valid for as long as they are used.

_Example: Explicit Lifetime Annotation_

```rust
fn longest<'a>(s1: &'a str, s2: &'a str) -> &'a str {
if s1.len() > s2.len() {s1} else {s2}
}

fn main(){
let s1 = String::from("long string");
let s2 = "short";
let result = longest(s1.as_str(), s2);
println!("longest: {}", result); // "long string"
}
```
---

__6. Macros (Metaprogramming)__

Rust macros allow code generation at compile time.

_Example_: `println!` _Macro_
```rust
macro_rules! say_hello {
() => {
println!("Hello, world!");
};
}

fn main() {
say_hello!(); // "Hello, world!"
}
```

_Example: Custom Vector Macro_

```rust
macro_rules! vec_macro {
($($x:expr), *) => {
{
let mut temp_vec = Vec::new();
$(temp_vec.push($x);) * temp_vec
}
};
}

fn main() {
let y = vec_macro![1, 2, 3];
println!("{:?}", v) // [1, 2, 3]
}
```
---

__7. Unsafe Rust (For Low-Level Control)__

Rust allows unsafe blocks for operations that bypass safety (eg. raw pointers, FFI)

_Example: Derefencing a Raw Pointer_
```rust
fn main() {
let mut num = 5;
let raw_ptr = &mut num as *mut i32;

unsafe {
*raw_ptr += 1;
println!("{}", *raw_ptr); // 0
}
}
```
---

__8. Concurrency (Fearless Parallelism)__

Rust's ownership model makes concurrency safer.

_Example: Spawning Threads_

```rust
use std::thread;
fn main() {
let handle = thread::spawn(|| {
println!("Hello from a thread!");
});

handle.join().unwrap(); // Wait for the thread to finish
}
```

_Example: Message Passing_ (`std::sync::spec`)

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
// create a channel (mpsc = multiple producer, single consumer)
let (sender, receiver) = mpsc::channel();

//spawn a thread that sends messages
let sender_thread = thread::spawn(move || {

let messages = vec! [
"Hello from thread 1",
"Greetings from thread 2",
"Goodbye from thread 3",
];

for msg in messages {
sender.send(msg).unwrap();
println!("Sent: {}", msg);
thread::sleep(Duration::from_secs(1));
}

});

// Main thread received messages
for received in receiver {
println!("Received: {}", received);
}

//wait for the sender thread to finish
sender_thread.join().unwrap();
}
```
> _**Key Points**_:

- **Channel Creation**: `mpsc::channel()` creates a communication channel with sender and receiver
- **Thread Spawning**: The sender is moved into a new thread.
- **Message Passing**: `sender.send()` transmits data while the receiver iterates over incoming messages.
- **Ownership**: The `move` keyword transfers ownership of the sender to the new thread.

* _Alternative with Multiple Senders_

```rust
use std:sync::mspc;
use std::thread;

fn main() {
let (sender, receiver) mpsc::channel();

//spawn two sender threads
for i in 0..2 {
let sender = sender.clone();
thread::spawn(move || {
sender.send(format!("Message from thread {}", i)).unwrap();
});
}
// drop the original sender to allow the exit
drop(sender);

for received in receiver{
println!("Recived: {}", received);
}
}
```
* This demonstrates rust's ownership system ensuring safe concurrent access.

---

__9. Iterators (Lazy Evaluation)__

Rust iteratos are lazy and chainable

_Example: Chaining Iterators_

```rust
fn main() {
let numbers = vec![1, 2, 3, 4, 5];
let sum: i32 = numbers.iter()
.filter(|&x| x % 2 == 0) // Keep even numbers
.map(|x| x * 2)
.sum();

println!("Sum: {}", sum); // 12 (2 + 4 + 6)
}
```
---

__10.__ `#[derive]` __(Automatic Trait Implementation)__

Rust can auto-implement traits like `Debug`, `Clone`, and `PartialEq`.

_Example: Deriving Traits_

```rust
#[derive(Debug, Clone, PartialEq)]

struct Point {
x: i32,
y: i32
}

fn main() {
let p1 = Point {x: 1, y: 2};
let p2 = p1.clone();
println!("{:?}", p1); // Debug output
println!("Are they equal? {}", p1 == p2); // true

}
```
---

### Final Thoughts

These features make Rust **unique**:
- **Memory safety without GC** (ownership/borrowing)
- **Pattern Matching** (powerful `match`)
- **Zero-cost abstractions** (traits, iterators)
- **Fearless concurrency** (threads, channels)
- **Metaprogramming** (macros)

__Official Documentation & Core Guides__ :nerd_face:

- [The Rust Programming Language](https://doc.rust-lang.org/book/): _Affectionately known as **"The Book"**, this is the absolute **gold-standard** introduction to Rust._
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/): _A massive collection of runnable code examples that teaches Rust through practical snippets rather than heavy theory._
- [The Rustonomicon](https://doc.rust-lang.org/nomicon/): _The ultimate, dark-arts guide to the advanced and unsafe corners of Rust programming._
- [The Rust Reference](https://doc.rust-lang.org/reference/): _The detailed, formal reference manual for the language syntax, constructs, and memory model._

_**Intermediate & Advanced Open Books**_ :muscle:

- [Comprehensive Rust](https://google.github.io/comprehensive-rust/): _A thorough, fast-paced Rust course developed and used internally by Google's Android team._
- [Rust Design Patterns](https://rust-unofficial.github.io/patterns/intro.html): _An open-source book dedicated to idioms, design patterns, and anti-patterns unique to Rust_
- [Effective Rust](https://effective-rust.com/title-page.html): _35 pecific ways to improve your Rust code, available completely free online (with a print version available for purchase)_


__Join the Community__ :people_hugging:
  
- [Rust Discord](https://discord.com/invite/rust-lang-community): _One of the largest hubs for real time chatter, for general help web development and compiler deep dives_
- [Tokio Discord Server](https://discord.com/invite/tokio): _For asynchronous application developers, this server is very active._
- [Rust Users Forum](https://users.rust-lang.org/): _The oficial forum to ask coding questions, debug problems, share crates, or make project announcements_
- [Rust Internals Forum](https://users.rust-lang.org/): _The official venue for discussing the language design itself, compiler features, and active RFCs._
- [Rust Reddit Community](https://www.reddit.com/r/rust/): _A massive and highly active hub for sharing blog posts, ecosystem news, tutorials, amd community project updates._
- [Rust Foundation Official Page](https://rustfoundation.org/): _Follow the Rust Foundations official page for industry developments, grant structures, and global events like RustConf._
- [Meetup Rust Groups](https://www.meetup.com/topics/rust/): _Find local groups or online-accessible user groups near you to network in person._

_Think I missed something here? Hit me up on_ [Bluesky](https://bsky.app/profile/lightspeed001.bsky.social) _or_ [LinkedIn](https://www.linkedin.com/in/edmund-rantsimele-08a13b300/). _Or raise an issue_ :smiley:
