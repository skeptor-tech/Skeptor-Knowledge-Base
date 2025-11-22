# Rust Learning and Tooling Primer

## Tags

- languages/rust
- learning/resources
- tooling/toolchains
- observability/logging
- ai/machine-learning

## Overview

Practical references for bootstrapping Rust on a new machine, structuring daily
toolchain usage, and exploring curated resources that reinforce the language's
systems focus and growing machine learning ecosystem.

## Learning resources

- [Rust By Example](https://doc.rust-lang.org/rust-by-example/custom_types/structs.html)
  walks through syntax and ownership by experimentation; keep it nearby to tease
  apart patterns like enums with data and trait-driven polymorphism.
- [Rust on Exercism](https://exercism.org/tracks/rust) adds mentor feedback and
  test-first exercises so beginners can practice lifetimes, iterators, and async
  ergonomics with guardrails.
- [Beginners Series: Rust](https://github.com/microsoft/beginners-series-rust)
  is a Microsoft Learn video/code pairing that scaffolds from `cargo new` to
  async services—ideal for pairing with journaling or lunch-and-learn sessions.
- [Awesome Rust Machine Learning](https://github.com/vaaaaanquish/Awesome-Rust-MachineLearning)
  catalogs crates for tensor math, differentiable programming, and model
  serving; skim it when scouting libraries beyond `tch-rs` or `burn`.

## Toolchain setup (rustup + cargo)

1. Install the official toolchain manager:

   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

2. Verify the compiler, then make sure releases stay current:

   ```bash
   rustc --version
   rustup update
   ```

3. Scaffold and build a release binary with Link Time Optimization enabled to
   shrink artifacts:

   ```bash
   cargo new hello
   cat <<'EOF' >> hello/Cargo.toml
   [profile.release]
   lto = true
   EOF

   cd hello
   cargo build --release
   strip target/release/hello
   ```

4. Switch channels when you need bleeding-edge features (`rustup default
   nightly`) or opt into per-project `rust-toolchain.toml` pins for reproducible
   builds.

5. Use `cargo search crate_name` before adding dependencies to confirm
   maintenance cadence, MSRV, and optional features.

## Logging stack

Rust's logging ecosystem revolves around the `log` facade, which exposes macros
(`info!`, `warn!`, `trace!`) that fan out to whichever backend you initialize at
runtime. Pair it with `simplelog` for quick CLI projects or prototypes:

```toml
[dependencies]
log = "0.4.17"
simplelog = "0.12.0"
```

```rust
use log::{error, info};
use simplelog::{
    ColorChoice, CombinedLogger, Config, LevelFilter, TermLogger, TerminalMode, WriteLogger,
};
use std::fs::File;

fn main() {
    CombinedLogger::init(vec![
        TermLogger::new(
            LevelFilter::Warn,
            Config::default(),
            TerminalMode::Mixed,
            ColorChoice::Auto,
        ),
        WriteLogger::new(
            LevelFilter::Info,
            Config::default(),
            File::create("app.log").unwrap(),
        ),
    ])
    .unwrap();

    error!("Visible on stderr");
    info!("Captured only in app.log");
}
```

Swap `simplelog` with `env_logger`, `tracing`, or `log4rs` as observability
needs grow; the facade calls stay identical.

## Machine learning crates

- [rust-bert](https://github.com/guillaume-be/rust-bert) wraps Transformer
  architectures (BERT, GPT, Marian) with `tch-rs`, enabling text generation and
  translation workloads with CPU/GPU acceleration.
- [Awesome Rust Machine Learning](https://github.com/vaaaaanquish/Awesome-Rust-MachineLearning)
  doubles as an index for classical ML, differentiable programming, and data
  tooling crates—bookmark it when hunting for optimizers or ONNX runtimes.

