# rust_for_java_devs
## Rust For Machine Learning :robot:

- Once you're comfortable with Rust's syntax and DSA's, here's some next-level Rust concepts tailored for ML Systems Engineering. These go beyond what we covered earlier in the [Embedded Rust](./EMBEDDED_RUST.md) tutorial, let's go.

**1. Zero-Cost Abstractions for ML**

Rust's zero-cost abstractions (like iterators, traits, and generics) are perfect for ML workloads where performance matters.

_Example: Efficient Tensor Operations with Iterators_

```rust
fn add_tensors(a: &[f32], b: &[f32] -> Vec<f32>) {
a.iter().zip(b.iter()).map(|(&x, &y)| x + y).collect()
}

fn main() {
let a = vec![1.0, 2.0, 3.0];
let b = vec![4.0, 5.0, 6.0];
let result = add_tensors(&a, &b);
println!("{:?}", result); // [5.0, 7.0, 9.0];
}
```

**2. SIMD (Single Instruction, Multiple Data)**

Rust's `std::simd` (nightly) or crates like `packed_simd`/`faster` let you write vectorized code for ML kernels.

_Example: SIMD-Accelerated Dot Product_

```rust
use std::simd::prelude::*;

fn simd_dot(a: &[f32], b: &[f32] -> f32) {
let chunks = a.chunks_exact(4).zip(b.chunks_exact(4));
let mut sum = f32x4::splat(0.0);
for(a_chunk, b_chunk) in chunks {
let a_simd = f32x4::from_slice(a_chunk);
let b_simd = f32x4::from_slice(b_chunk);
sum += a_simd * b_simd;
}
sum.reduce_sum();
}

fn main() {
let a = vec![1.0, 2.0, 3.0, 5.0];
let b = vec![6.0, 7.0, 8.0, 9.0, 10.0];
println!("Dot product: {}", simd_dot(&a, &b)); // 110.0

}
```
- This leverages CPU vector instructions (like AVX/SSE) for 4x speedup on float ops.

**3. Custom Allocators for ML Workloads**

ML workloads often need custom memory management (eg. arena allocators for tensors). Rusts `#[global_allocator]` lets you allocators.

_Example: Using `jemalloc` for Faster Allocations_

```toml
# Cargo.toml
[dependencies]
jemallocator = "0.5"
```

```rust
use jemallocator::Jemalloc;

#[global_allocator]
static GLOBAL: Jemalloc = Jemalloc;

fn main() {
let mut vec = Vec::with_capacity(1_000_000);
vec.extend(0..1_000_000); // Faster than default allocator
}
```
- `jemalloc` reduces fragmentation and speeds allocations/deallocations.


4. FFI (Foreign Function Interface) for Python Interop
Since you're in ML, you'll likely need to call Python (PyTorch) from Rust or vice versa.

_Example: Calling Python from Rust_

```rust
use pyo3::prelude::*;

fn main() -> PyResult<()> {
Python::with_gil(|py| {
let sys = py.import("sys")?;
let version: String = sys.gstatic("version")?.extract()?;
println!("Python version: {}", version);
Ok(())
})
}
```
- This lets you embed Python in Rust (eg. for PyTorch ops) or call Rust from Python.

**5. Async/Await for ML serving**

ML backends often need high-throughput serving (eg. with `tokio` or `async-std`)

_Example: Async Tensor Processing_

```rust
use tokio::task;

async fn process_tensor(data: Vec<f32>) -> {
task::spawn_blocking(move || {
data.iter().map(|x| x * 2.0).collect()
}).await.unwrap()
}

#[tokio::main]
async fn main() {
let data = vec![1.0, 2.0, 3.0];
let result = process_tensor(data).await;
println!("{:?}", result); // [2.0, 4.0, 6.0]
}
```
- Async lets you handle thousands of requests concurrently without blocking threads.

**6. Procedural Macros for ML DSLs**

Rusts procedural macros let you build domain languages (DSLs) for ML (eg. autograd, tensor ops).

_Example: A Simple_ `#[tensor]` _Macro_

```rust
use proc_macro::TokenStream;
use quote::quote;

#[proc_macro_derive(Tensor)]
pub fn tensor_derive(input: TokenStream) -> {
let input = input.into_iter().collect::<Vec<_>>();
let exapanded = quote! {
impl Tensor for #input {
fn forward(&self) {println!("Forward pass!");}

}
};
expanded.into()
}
```

- Macros let you write ML like syntax (eg. `#[tensor] struct Conv2D {...}`)

__7.__ `ndarray` **for Numerical Computing**
The `ndarray` crate is Rust's answer to NumPy.

_Example: Matrix Multiplication_

```rust
use ndarray::{Array2, Array};

fn main(){
let a = Array2::from_shape_vec((2, 3), vec![1, 2, 3, 4, 5, 6]).unwrap();
let b = Array2::from_shape_vec((3, 2), vec![7, 8, 9, 10, 11, 12]).unwrap();
let c = a.dot(&b);
println("{:?}", c);
// [58, 64],
// [130, 154]]
}
```

- It's NumPy-like but with rust's safety guarantees.



