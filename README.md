# binary-heap-plus-rs

[![Build Status](https://travis-ci.org/sekineh/binary-heap-plus-rs.svg?branch=master)](https://travis-ci.org/sekineh/binary-heap-plus-rs)
[![Build status](https://ci.appveyor.com/api/projects/status/oewb6667ul5pl05d?svg=true)](https://ci.appveyor.com/project/sekineh/binary-heap-plus-rs)

Enhancement over Rust's `std::collections::BinaryHeap`.

It supports the following features and still maintains backward compatibility.
- Max heap
- Min heap
- Heap ordered by closure
- Heap ordered by key generated by closure

This crate requires Rust 1.26 or later.

# Quick start

## Max/Min Heap
For max/min heap, `BiaryHeap::from_vec()` is the most versatile way to create a heap.

```rust
    extern crate binary_heap_plus;
    use binary_heap_plus::*;

    // max heap
    let mut h: BinaryHeap<i32> = BinaryHeap::from_vec(vec![]);
    // max heap with initial capacity
    let mut h: BinaryHeap<i32> = BinaryHeap::from_vec(Vec::with_capacity(16));
    // max heap from iterator
    let mut h: BinaryHeap<i32> = BinaryHeap::from_vec((0..42).collect());
    assert_eq!(h.pop(), Some(41));
```
Min heap requires type annotation.
```rust
    extern crate binary_heap_plus;
    use binary_heap_plus::*;

    // min heap
    let mut h: BinaryHeap<i32, MinComparator> = BinaryHeap::from_vec(vec![]);
    // min heap with initial capacity
    let mut h: BinaryHeap<i32, MinComparator> = BinaryHeap::from_vec(Vec::with_capacity(16));
    // min heap from iterator
    let mut h: BinaryHeap<i32, MinComparator> = BinaryHeap::from_vec((0..42).collect());
    assert_eq!(h.pop(), Some(0));
```

## Custom Heap
For custom heap, `BinaryHeap::from_vec_cmp()` works in a similar way to max/min heap. The only difference is that you add the comparator closure with apropriate signature.
```rust
    extern crate binary_heap_plus;
    use binary_heap_plus::*;

    // custom heap: ordered by second value (_.1) of the tuples; min first
    let mut h = BinaryHeap::from_vec_cmp(
        vec![(1, 5), (3, 2), (2, 3)],
        |a: &(i32, i32), b: &(i32, i32)| b.1.cmp(&a.1), // comparator closure here
    );
    assert_eq!(h.pop(), Some((3, 2)));
```

## Note on `BinaryHeap::from_vec()`

Currently, the `From<Vec<T>>` trait is implemented for max heap only.
If you add generic impl for other heaps, the existing code breaks, requires 
slight modification such as type annotation.

To maintain good compatibility with `std` version, `::from_vec()` method was added
for the same purpose.

# Changes

## v0.2.0

* [COMPATIBILITY CHANGE] Use `Compare` trait from `compare` crate instead of our own definition.
Most users should not be affected by this. TIP: External `Compare<T>` impls needs to be updated to use `Fn` instead of `FnMut`.
* [COMPATIBILITY CHANGE] rename feature `serde1` to `serde` in order to comply with the guideline: 
https://rust-lang-nursery.github.io/api-guidelines/interoperability.html#c-serde
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
