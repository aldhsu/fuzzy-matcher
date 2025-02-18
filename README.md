[![Crates.io](https://img.shields.io/crates/v/fuzzy-matcher.svg)](https://crates.io/crates/fuzzy-matcher)

# Fuzzy Matcher

Fuzzy matching algorithm(s) in Rust!

## Usage

In your Cargo.toml add the following:

```toml
[dependencies]
fuzzy-matcher = "*"
```

Here are some code example:

```rust
use fuzzy_matcher::skim::{fuzzy_match, fuzzy_indices};

assert_eq!(None, fuzzy_match("abc", "abx"));
assert!(fuzzy_match("axbycz", "abc").is_some());
assert!(fuzzy_match("axbycz", "xyz").is_some());

let (score, indices) = fuzzy_indices("axbycz", "abc").unwrap();
assert_eq!(indices, [0, 2, 4]);
```

- `fuzzy_match` only return scores while `fuzzy_indices` returns the matching
    indices as well.
- Both function return None if the pattern won't match.
- The score is the higher the better.

## More example

`echo "axbycz" | cargo run --example fz "abc"` and check what happens.

## About the Algorithm

### Skim

The skim is currently used by [skim](https://github.com/lotabout/skim), a
fuzzy finder.

### Clangd

- The algorithm is based on [clangd's FuzzyMatch.cpp](https://github.com/MaskRay/ccls/blob/master/src/fuzzy_match.cc).
- Also checkout https://github.com/lewang/flx/issues/98 for some variants.
- The algorithm is `O(mn)` where `m, n` are the length of the pattern and
    input line.
- Space complexity is `O(mn)` for `fuzzy_indices` and `O(2n)` for
    `fuzzy_match` which will compress the table for dynamic programming.
