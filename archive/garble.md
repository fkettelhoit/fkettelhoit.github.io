# The Garble Programming Language

[github.com/sine-fdn/garble-lang](https://github.com/sine-fdn/garble-lang)

Garble is a simple statically typed programming language with a Rust-like syntax for [Multi-Party Computation](https://en.wikipedia.org/wiki/Secure_multi-party_computation) with [garbled circuits](https://en.wikipedia.org/wiki/Garbled_circuit). Garble programs look like Rust programs, but are compiled to boolean circuits consisting only of AND, XOR and NOT gates. All programs are guaranteed to terminate and always run in a constant time, which is a fancy way of saying that the underlying circuits do not have (unbounded) loops or (real) jumps / branches. This makes for a [weird programming model](https://garble-lang.org/computational_model.html), because Garble programs don't execute instructions on a typical von Neumann architecture with memory and CPU.

Apart from its unusual compilation target, Garble is deliberately _boring_ and implements a simple subset of Rust (basically Rust without references, generics and unbounded loops). Garble stays as close to Rust's syntax and semantics as it can, even implementing Rust's [pattern matching algorithm](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_pattern_analysis/usefulness/index.html).

Here's what Garble looks like:

```
enum Bucket {
    Above,
    Within,
    Below,
}

fn bucket(p: i32, min: i32, max: i32) -> Bucket {
    if p > max {
        Bucket::Above
    } else if p < min {
        Bucket::Below
    } else {
        Bucket::Within
    }
}

pub fn main(x: i32, y: i32, z: i32) -> [Bucket; 3] {
    let avg = (x + y + z) * 1000 / 3;
    let range_in_percent = 10;
    let min = avg * (100 - range_in_percent) / 100;
    let max = avg * (100 + range_in_percent) / 100;
    [
        bucket(x * 1000, min, max),
        bucket(y * 1000, min, max),
        bucket(z * 1000, min, max),
    ]
}
```

For more, see the [Garble website](https://garble-lang.org).
