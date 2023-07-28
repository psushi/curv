[![Build Status](https://app.travis-ci.com/ZenGo-X/curv.svg?branch=master)](https://app.travis-ci.com/ZenGo-X/curv)
[![Latest version](https://img.shields.io/crates/v/curv-kzen.svg)](https://crates.io/crates/curv-kzen)
[![Docs](https://docs.rs/curv-kzen/badge.svg)](https://docs.rs/curv-kzen)
[![License](https://img.shields.io/crates/l/curv-kzen)](LICENSE)
[![dependency status](https://deps.rs/repo/github/ZenGo-X/curv/status.svg)](https://deps.rs/repo/github/ZenGo-X/curv)


## Note: 
This is a fork of Curv, with the following changes:
- Updated `secp256k1` from `0.20.0` -> `0.23.0` (to remove use of `rand@0.6` and `rand_os` which is known to cause issues with `wasm32-unknown-unknown`)
- Updated `rand` from `0.7` -> `0.8` 
- Removed use of `rand@0.6` 
- Updated `curve25519-dalek` from `3` to `4` (to use `rand@0.8`)
- Removed the bls12-381 curve support (to remove use of `ff_zeroize` which is known to cause issues with `wasm32-unknown-unknown`). Our use case only needs support for secp256k1
- Removed `pairing_plus`, `old_sha2` unused deps. 
- Refactored curv to adopt the changes. All tests passing ✅
- Acknowledge that `num-bigint` support is experimental and should not be used in production as it's not consider secure.




Curv
=====================================
Curv contains an extremely simple interface to onboard new elliptic curves. 
Use this library for general purpose elliptic curve cryptography. 

The library has a built in support for some useful operations/primitives such as verifiable secret sharing, commitment 
schemes, zero knowledge proofs, and simple two party protocols such as ECDH and coin flip. The library comes with 
serialize/deserialize support to be used in higher level code to implement networking. 

### Usage

To use `curv` crate, add the following to your Cargo.toml:
```toml
[dependencies]
curv-kzen = "0.9"
```

The crate will be available under `curv` name, e.g.:
```rust
use curv::elliptic::curves::*;
```

### Currently Supported Elliptic Curves  

|        Curve         |   low level library    |    curve description       |     
|-------------------------------|------------------------|------------------------|
|    **Secp256k1**    |        [rust-secp256k1](https://github.com/rust-bitcoin/rust-secp256k1)            |      [bitcoin wiki](https://en.bitcoin.it/wiki/Secp256k1)           |     
|    **P-256**    |        [RustCrypto](https://crates.io/crates/p256)            |      [NIST.FIPS.186.4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf)           |     
|    **Ed25519**    |        [cryptoxide](https://github.com/typed-io/cryptoxide/blob/master/src/curve25519.rs)            |      [BDLSY11](https://ed25519.cr.yp.to/ed25519-20110926.pdf)           |      
|    **Ristretto**    |        [curve25519-dalek](https://github.com/dalek-cryptography/curve25519-dalek)            |     [ristretto group](https://ristretto.group/)           |      
|    **BLS12-381**    |        [bls12-381](https://crates.io/crates/bls12_381)            |     [BLS12-381 For The Rest Of Us](https://hackmd.io/@benjaminion/bls12-381)           |     

### Security  
The library was audited by [Kudelski security](https://www.kudelskisecurity.com/) on Feb19. The report can be found 
[here](https://github.com/KZen-networks/curv/tree/master/audit). No critical issue were found and all issues found 
were fixed.

The code was reviewed independently by few other cryptographers. Special thanks goes to [Claudio Orlandi](http://cs.au.dk/~orlandi/) 
from Aarhus University. 

In general security of the library is strongly dependent on the security of the low level libraries used. We chose only 
libraries that are used as part of other big projects and went through heavy audit/review. 

The library is not immune to side channel attacks but considerable effort was given to try and catch as many such 
attacks as possible (see audit report). 

### Build
Use `cargo build` to build everything including curve implementations, cryptoprimitives, BigInt, etc.

### Big integer implementation
The library supports a couple of bigint implementations and can easily switch between them.
You can choose any one which you prefer by specifying a feature:
* **rust-gmp-kzen**, uses GMP bindings, requires GMP to be installed on a machine. Used by default.
* **num-bigint**, Rust's pure implementation of big integer. In order to use it, put in Cargo.toml:
  ```toml
  [dependencies.curv-kzen]
  version = "0.8"
  default-features = false
  features = ["num-bigint"]
  ```
  
  **_Warning:_** `num-bigint` support is experimental and should not be used in production. For this
  bigint implementation, we use prime numbers generator which is not considered secure.

### Examples
The library includes some basic examples to get you going. To run them: 
`cargo run --example EXAMPLE_NAME -- CURVE_NAME`
for example: `cargo run --example proof_of_knowledge_of_dlog -- secp256k1`

### Docs 
To build docs, use:
```bash
cargo doc
RUSTDOCFLAGS="--html-in-header katex-header.html" cargo doc --no-deps --open
```

### License
Curv is released under the terms of the MIT license. See [LICENSE](LICENSE) for more information.


### Development Process & Contact
This library is maintained by ZenGo-X. Contributions are highly welcomed! Besides GitHub issues and PRs, feel free to [reach out](mailto:github@kzencorp.com) by mail or join ZenGo X [Telegram](https://t.me/joinchat/ET1mddGXRoyCxZ-7) for discussions on code and research. 

