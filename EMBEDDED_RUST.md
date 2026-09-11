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
----

__2. Hardware Abstraction & Peripheral Access__

Rust provides crates for direct hardware control:
  - `embedded-hal`: Standard traits for GPIO, I2C, SPI, PWM, etc.
  - `svd2rust`: Generates Rust APIs from CMSIS-SVD (ARM Cortex-M).
  - `stm32f4xx`: Hardware abstraction for STM32 microcontrollers.

_Example: Blinking an LED (STM32)_

```rust
#![no_std]
#![no_main]

use cortex_m_rt::entry;
use stm32f4xx_hal::{pac, prelude::*};

#[entry]
fn main() -> ! {
let dp = pac::Peripherals::take().unwrap();
let gpioa = dp.GPIOA.spit();

let mut led = gpioa.pa5.into_push_pull_output();
loop {
led.set_high();
delay(500);
led.set_low();
delay(500);
}
}

fn delay(ms: u32) {
// Simple delay implementation
for _ in 0..ms * 1000 {
cortex_m::asm::nop();
}
}
```
----

__3. Real-Time & Low-Latency Systems__

Rust's predictability and lack of hidden allocations make it suitable for real-time systems:
	- `rtic`(Real-Time Interrupt-driven Concurrency): A framework for deterministic scheduling.
	- `cortex-m-rtic`: RTIC for ARM Cortex-M.

_Example: RTIC Blinky_

```rust
#![no_std]
#![no_main]

use rtic::app;
use stm32f4xx_hal::{pac, prelude::*};

#[app(device = stm32f4xx_hal::pac, peripherals = true)]
mod app {
use super::*;

#[shared]
struct Shared {}

#[local]
struct Local {
led: gpio::Pin<Output>,
}

#[init]
fn init(cx: init::Context) -> (Shared, Local) {
let dp = cx.device;
let gpioa = dp.GPIOA.split();
let led = gpioa.pa5.into_push_pull_output();

(Shared {}, Local {led})
}

#[idle(local = [led])]
fn idle(cx: idle::Context) -> ! {
loop {
cx.local.led.set_high();
delay(500);
cx.local.led.set_low();
delay(500);
}
}
}

fn delay(ms: u32) {
for _ in 0..ms * 1000 {
cortex_m::asm::nop();
}
}
```
----

__4. Edge AI and ML Inference__
For AI at the edge, Rust offers:
- `tch-rs`: Rust bindings for PyTorch (TorchScript)
- `onnxruntime-rs`: ONNX Runtime for inference.
- `embedded-ml`: Lightweight ML for microcontrollers

_Example: Running a TinyML Model (Tensorflow Lite)_

```rust
use tflite::Interpreter;

fn run_inference() {
let model_data = include_bytes!("model.tflite");
let interpreter = Interpreter::new(model_data).unwrap();

// Allocate tensors
interpreter.allocate_tensors().unwrap();

// Fill input tensor
let input = interpreter.input(0).unwrap();
input.copy_from_slice(&[0.1, 0.2, 0.3, 0.4]);

// Run inference
let input = interpreter.input(0).unwrap();
input.copy_from_slice(&[0.1, 0.2, 0.3, 0.4]);

// Run inference
interpreter.invoke().unwrap();

//Get output
let output = interpreter.out(0).unwrap();
println!("Output: {}", output);
}
```
----

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

Rust's performance, safety and ecosystem make it ideal for:
- Bare-metal & `no_std` programming
- Real-time & low-latency systems
- Edge AI inference (TinyML, ONNX, PyTorch)
- Hardware abstraction & peripheral control
- Memory-safe concurrency
