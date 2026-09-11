# rust_for_java_devs
## Rust For Embedded Systems and Edge AI

---
- Rust is a good choice for Edge AI and Embedded Systems due to it's performance, memory safety, and zero-cost abstractions.
Below are **key Rust programming approaches** and examples applicable to these domains:

1. __Bare-Metal & No-Std Programming__

Edge and embedded systems often run without an OS (bare-metal) or with minimal runtime (`no_std`). 
Rust supports this via:
  - `#[no_std]`: Disbables the standard library for embedded targets.
  - `#[panic_handler]`: Custom panic handlers for embedded systems.
  - `#[alloc_error_handler]`: Custom allocator error handling.

_Example: Minimal_ `no_std` _Program_

```rust
#![no_std]
#![no_main]

use core::panic::PanicInfo;

#[panic_handler]
fn panic(_info: &PanicInfo) -> ! {
loop {}
}

#[no_mangle]
pub extern "C" fn _start() -> ! {
// Entry point for bare-metal systems
loop {}
}
```

---
### Key Crates for Edge AI & Embedded Rust :hammer_and_wrench:

- `embedded-hal`: Hardware abstraction
- `rtic`: Real-time scheduling
- `tch-rs`: PyTorch inference
- `onnxruntime-rs`: ONNX model inference
- `heapless`: `no_std` data structures
- `probe-rs`: Debugging & flashing
- `cortex-m`: ARM Cortex-M support

---

### Conclusion :pushpin:

Rust's perormance, safety and ecosystem make it ideal for:
- Bare-metal & `no_std` programming
- Real-time & low-latency systems
- Edge AI inference (TinyML, ONNX, PyTorch)
- Hardware abstraction & peripheral control
- Memory-safe concurrency
