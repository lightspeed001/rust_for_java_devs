# rust_for_java_devs
## Rust For Java Developers (TLDR)

## Rust for Java Devs (TLDR)

### Rust Syntax for Java Developers

- Rust and Java share some similarities (like curly braces and semicolons), but Rusts syntax and paradigms differ significantly. Below is a comparison of key concepts.
__1. Variables and Mutability__

- Java: Variables are mutable by default unless declared **final**.
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

- No return needed fo rthe last expression (Rust uses expressions not statements)
- Type annotations come after the variable name (a: i32)

---

__3. Ownership and Borrowing__

- Rust enforces memory safety without a garbage collector via ownership
- Each value has a single owner
- Ownership can be moved (transferred) or borrowed (references)

Example: Ownership Transfer

```rust
let s1 = String::from("hello")
let s2 = s1; // Ownership moves to s2, s1 is no longer valid
//println!("{}", s); // Error: value borrowed here after move
```

Example: Borrowing (References)
```rust
let s String::from("hello");
print_string(&s); // Pass a reference (borrow)
println!("{}", s);

```
---

__4. Structs (insted of classes)__

- Rust doesnt have classes but has structs and traits (similsr to interfaces).

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
 
 Rusts **enum** is more powerful than Java's (can hold data)
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

- Pattern Matching (Like **switch** but better):
```rust
match status {
status::Active => println!("Active"),
Status::Inactive => println!("Inactive"),
}
```
---
__6. Error Handling__

- Java: Uses exceptions (**try-catch**).
- Rust: Uses **Result<T, E>** (no exception).

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
1. Ownership and Borrowing: Memory safety without GC.
2. Zero-Cost Abstractions: High-level constructs compile to efficient machine code.
3. Explicitness: Types, mutability and error handling are explicit.
4. Fearless Concurrency: Thread safety is enforced at compile time.
5. Pattern Matching: Powerful **match** for control flow.

__Key differences from Java__:
- No inheritence (traits are used instead).
- No **null** (use **Option<T>**).
- No exceptions (use **Result<T, E>**).
- No global state (everything is scoped).

### Summary

* Rust is not OOP but it supports structured programming with structs and traits.
* Memory safety is guaranteed at compile time via ownership.
* Syntax is similar to Java but with stricter rules (immutability by default explicit types).



