# RSA

[![crates.io](https://img.shields.io/crates/v/rsa.svg)](https://crates.io/crates/rsa) [![Documentation](https://docs.rs/rsa/badge.svg)](https://docs.rs/rsa) [![Build Status](https://github.com/rustcrypto/RSA/workflows/CI/badge.svg)](https://github.com/RustCrypto/RSA/actions) ![minimum rustc 1.44](https://img.shields.io/badge/rustc-1.44+-blue.svg)

A portable RSA implementation in pure Rust.

:warning: **WARNING:** This crate has been audited by a 3rd party, but a full blog post with the results and the updates made since the audit has not been officially released yet. See [#60](https://github.com/RustCrypto/RSA/issues/60) for more information.

## Example

```rust
use rsa::{PublicKey, RSAPrivateKey, PaddingScheme};
use rand::rngs::OsRng;

let mut rng = OsRng;
let bits = 2048;
let priv_key = RSAPrivateKey::new(&mut rng, bits).expect("failed to generate a key");
let pub_key = RSAPublicKey::from(&priv_key);

// Encrypt
let data = b"hello world";
let enc_data = pub_key.encrypt(&mut rng, PaddingScheme::new_pkcs1v15(), &data[..]).expect("failed to encrypt");
assert_ne!(&data[..], &enc_data[..]);

// Decrypt
let dec_data = priv_key.decrypt(PaddingScheme::new_pkcs1v15(), &enc_data).expect("failed to decrypt");
assert_eq!(&data[..], &dec_data[..]);
```

## Status

Currently at Phase 1 (v) :construction:.

There will be three phases before `1.0` :ship: can be released.

1. :construction:  Make it work
    - [x] Prime generation :white_check_mark:
    - [x] Key generation :white_check_mark:
    - [x] PKCS1v1.5: Encryption & Decryption :white_check_mark:
    - [x] PKCS1v1.5: Sign & Verify :white_check_mark:
    - [ ] PKCS1v1.5 (session key): Encryption & Decryption
    - [x] OAEP: Encryption & Decryption
    - [x] PSS: Sign & Verify
    - [x] Key import & export
2. :rocket: Make it fast
    - [x] Benchmarks :white_check_mark:
    - [ ] compare to other implementations :construction:
    - [ ] optimize :construction:
3. :lock: Make it secure
    - [ ] Fuzz testing
    - [ ] Security Audits


## Minimum Supported Rust Version (MSRV)

All crates in this repository support Rust 1.44 or higher. In future
minimally supported version of Rust can be changed, but it will be done with
a minor version bump.

## License

Licensed under either of

 * [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)
 * [MIT license](http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
