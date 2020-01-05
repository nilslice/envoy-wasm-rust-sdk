# Rust bindings to Envoy ABI

⚠️ This is NOT the official Rust bindings to `Envoy ABI`.

💡The goal of this repo is to try out Rust bindings auto-generated by [WASI toolchain](https://github.com/bytecodealliance/wasi/tree/master/crates/generate-raw) out of `witx` definitions.

## Components

* [src/](./src/) - Rust bingings to `Envoy ABI`
  * [lib.rs](./src/lib.rs) - manually defined entry point into the library
  * [error.rs](./src/error.rs) - manually defined `Error` struct similar to [WASI](https://github.com/bytecodealliance/wasi/blob/master/src/error.rs)'s
  * [types_generated.rs](./src/types_generated.rs) - Rust types auto-generated out of [envoy-abi/v1alpha/witx/typenames.witx](../envoy-abi/v1alpha/witx/typenames.witx)
  * [envoy_host_generated.rs](./src/envoy_host_generated.rs) - Rust functions auto-generated out of [envoy-abi/v1alpha/witx/envoy_host.witx](../envoy-abi/v1alpha/witx/envoy_host.witx)

## How To

### How To Build

```shell
cargo build --release
```

### How To Regenerate Rust bindings

⚠️ At the moment, you need to use a fork of [bytecodealliance/wasi](https://github.com/yskopets/wasi).

1. Clone this repo into `$RUST_SDK_HOME`
1. Clone [yskopets/wasi](https://github.com/yskopets/wasi) into `$WASI_HOME` and checkout `feature/upgrade-witx` branch
3. Run inside `$WASI_HOME`
   ```shell
   cargo run -p generate-raw \
     $RUST_SDK_HOME/envoy-abi/v1alpha/witx/typenames.witx \
   > $RUST_SDK_HOME/envoy-rust-abi/src/types_generated.rs
   ```
4. Run inside `$WASI_HOME`
   ```shell
   cargo run -p generate-raw \
     $RUST_SDK_HOME/envoy-abi/v1alpha/witx/envoy_host.witx \
   > $RUST_SDK_HOME/envoy-rust-abi/src/envoy_host_generated.rs
   ```

⚠️ Notice that a few manual changes are still necessary to the files generated above.