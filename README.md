# cf-reqwest

[![crates.io](https://img.shields.io/crates/v/cf-reqwest.svg)](https://crates.io/crates/cf-reqwest)
[![Documentation](https://docs.rs/cf-reqwest/badge.svg)](https://docs.rs/cf-reqwest)
[![MIT/Apache-2 licensed](https://img.shields.io/crates/l/cf-reqwest.svg)](./LICENSE-APACHE)
[![CI](https://github.com/cloudflare/cf-reqwest/workflows/CI/badge.svg)](https://github.com/cloudflare/cf-reqwest/actions?query=workflow%3ACI)

An ergonomic, batteries-included HTTP Client for Rust.

- Async and blocking `Client`s
- Plain bodies, JSON, urlencoded, multipart
- Customizable redirect policy
- HTTP Proxies
- HTTPS via system-native TLS (or optionally, rustls)
- Cookie Store
- WASM

## About this fork

This is a fork of the great [reqwest](https://github.com/seanmonstar/reqwest) library with some features that were not accepted to the upstream:

- Custom request connectors (https://github.com/seanmonstar/reqwest/pull/1786)

We advice to use the upstream version of the library, unless you need any of those features.

## Example

This asynchronous example uses [Tokio](https://tokio.rs) and enables some
optional features, so your `Cargo.toml` could look like this:

```toml
[dependencies]
reqwest = { version = "0.12", features = ["json"] }
tokio = { version = "1", features = ["full"] }
```

And then the code:

```rust,no_run
use std::collections::HashMap;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let resp = cf_reqwest::get("https://httpbin.org/ip")
        .await?
        .json::<HashMap<String, String>>()
        .await?;
    println!("{resp:#?}");
    Ok(())
}
```

## Requirements

On Linux:

- OpenSSL with headers. See https://docs.rs/openssl for supported versions
  and more details. Alternatively you can enable the `native-tls-vendored`
  feature to compile a copy of OpenSSL.

On Windows and macOS:

- Nothing.

Reqwest uses [rust-native-tls](https://github.com/sfackler/rust-native-tls),
which will use the operating system TLS framework if available, meaning Windows
and macOS. On Linux, it will use the available OpenSSL or fail to build if
not found.

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall
be dual licensed as above, without any additional terms or conditions.
