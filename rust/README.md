# TrustMark — Rust implementation

<div style={{display: 'none'}}>

An implementation in Rust of TrustMark watermarking, as described in [**TrustMark - Universal Watermarking for Arbitrary Resolution Images**](https://arxiv.org/abs/2311.18297) (`arXiv:2311.18297`) by [Tu Bui](https://www.surrey.ac.uk/people/tu-bui)[^1], [Shruti Agarwal](https://research.adobe.com/person/shruti-agarwal/)[^2], and [John Collomosse](https://www.collomosse.com)[^1] [^2].

[^1]: [DECaDE](https://decade.ac.uk/) Centre for the Decentralized Digital Economy, University of Surrey, UK.

[^2]: [Adobe Research](https://research.adobe.com/), San Jose, CA.

</div>

This crate implements a subset of the functionality of the TrustMark Python implementation, including encoding and decoding of watermarks for all variants in binary mode. The Rust implementation provides the same levels of error correction as the Python implementation.

Text mode watermarks and watermark removal are not implemented.

Open an issue if there's something in the Python version that want added to this crate!

## Quick start

### Download models

In order to encode or decode watermarks, you'll need to fetch the model files. The models are distributed as ONNX files.

From the workspace root (the `rust/` directory), run:

```
cargo xtask fetch-models
```

This command downloads models to the `models/` directory. You can move them from there as needed.

### Run the CLI

As a first step, you can run the `trustmark-cli` which is defined in this repository.

From the workspace root, run:

```sh
cargo run --release -p trustmark-cli -- -m ./models encode -i ../images/ghost.png -o ../images/encoded.png
cargo run --release -p trustmark-cli -- -m ./models decode -i ../images/encoded.png
```

The argument to the `-m` option is the path to the models downloaded; if you moved them, pass the relative file path as the option value.

### Use the library

Add `trustmark` to your project's `cargo` manifest with:

```
cargo add trustmark
```

A basic example of using `trustmark` is:

```rust
use trustmark::{Trustmark, Version, Variant};

let tm = Trustmark::new("./models", Variant::Q, Version::Bch5).unwrap();
let input = image::open("../images/ghost.png").unwrap();
let output = tm.encode("0010101".to_owned(), input, 0.95);
```

## Running the benchmarks

### Rust benchmarks

To run the Rust benchmarks, run the following from the workspace root:

```
cargo bench
```

### Python benchmarks

To run the Python benchmarks, run the following from the workspace root:

```
benches/load.sh && benches/encode.sh
```
