# binary-heap-plus-rs

[![Build Status](https://travis-ci.org/sekineh/binary-heap-plus-rs.svg?branch=master)](https://travis-ci.org/sekineh/binary-heap-plus-rs)
[![Build status](https://ci.appveyor.com/api/projects/status/oewb6667ul5pl05d?svg=true)](https://ci.appveyor.com/project/sekineh/binary-heap-plus-rs)

Enhancement over Rust's `std::collections::BinaryHeap`.

It supports the following features and still maintains backward compatibility.
- Max heap
- Min heap
- Heap ordered by closure
- Heap ordered by key generated by closure

You can change the line

```
use std::collections::BinaryHeap;
```

to like below.

```
use binary_heap_plus::*;
```

Your code will compile as before unless you use unstable APIs.

This crate requires Rust 1.26 or later.

# Added muthods

## `BinaryHeap::new_xxx()`

- (original) `::new()`     // creates a max heap
- `::new_min()` // creates a min heap
- `::new_by(f)` // creates a heap ordered by the given closure `f`
- `::new_by_key(g)` // creates a heap ordered by key generated by the given closure `g`

## `BinaryHeap::with_capacity_xxx()`

- (original) `::with_capacity(n)` // creates a max heap with capacity `n`
- `::with_capacity_min(n)` // creates a min heap with capacity `n`
- `::with_capacity_by(n, f)` // creates a heap with capacity `n`, ordered by the given closure `f`
- `::with_capacity_by_key(n, g)` // creates a heap with capacity `n`,  ordered by key generated by the given closure `g`

## `BinaryHeap::from_vec()`

Currently, the `From<Vec<T>>` trait is implemented for max heap only.
If you add generic impl for other heaps, the existing code breaks, requires 
slight modification such as type annotation.

To maintain good compatibility with `std` version, `::from_vec()` method was added
for the same purpose.

# Changes

## v0.2.0

* [COMPATIBILITY CHANGE] Use `Compare` trait from `compare` crate instead of our own definition.
Most users should not be affected by this. TIP: External `Compare<T>` impls needs to be updated to use `Fn` instead of `FnMut`.
* Refactor ctor impl.

## v0.1.6

* Add generic constructor `from_vec()` and `from_vec_cmp()`.
* Refactor other ctor to call above methods.

## v0.1.5

* Add `serde1` feature which adds Serialize/Deserialize

## v0.1.4

* Merge #1) Do not require T: Ord when a custom comparator is provided

## v0.1.3

* Add comprehensive CI based on `trust` CI template v0.1.2
* README.md tweaks.

## v0.1.2

* Cargo.toml tweaks

# Thanks

- I received many valuable feedback from Pre-RFC thread [1].
  - The current design is based on @ExpHP's suggestion that compiles on stable compiler.
  - DDOtten, steven099, CAD97, ExpHP, scottmcm, Nemo157 and gnzlbg, thanks for looking into the design!
- @ulysseB sent me a first pull request!
- @inesseq contributed feature `serde1`.

# References

See the following discussions for the background of the crate:
- [1] https://internals.rust-lang.org/t/pre-rfc-binaryheap-flexibility/7482
- https://users.rust-lang.org/t/binaryheap-flexibility-revisited-supporting-other-than-max-heap/17062
- https://users.rust-lang.org/t/binaryheap-flexibility/8766
- https://github.com/rust-lang/rust/issues/38886
