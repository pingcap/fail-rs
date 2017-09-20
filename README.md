# fail-rs

[![Build Status](https://travis-ci.org/pingcap/fail-rs.svg?branch=master)](https://travis-ci.org/pingcap/fail-rs)
[![Build status](https://ci.appveyor.com/api/projects/status/vksd5ifajog5gbiu/branch/master?svg=true)](https://ci.appveyor.com/project/busyjay/fail-rs/branch/master)
[![Crates.io](https://img.shields.io/crates/v/fail.svg?maxAge=2592000)](https://crates.io/crates/fail)

[Documentation](https://docs.rs/fail)

A fail point implementation for Rust.

Fail point is a code point that are used to inject errors by users at runtime.
This crate is inspired by FreeBSD's [failpoints](https://freebsd.org/cgi/man.cgi?query=fail).

## Usage

First, add this to your `Cargo.toml`:

```toml
[dependencies]
fail = "0.1"
```

Next, add the following code to your crate:

```rust
#[macro_use]
extern crate fail;
```

Define the fail points:

```
fn function_return_tuple() {
    fail_point!("name1");
}

fn function_return_others() -> u64 {
    fail_point!("name2", |r| r.map_or(2, |e| e.parse().unwrap()));
    0
}

fn function_conditional(enable: bool) {
    fail_point!("name3", enable, |_| {});
}
```

Trigger a fail point via the environment variable:

```
$ FAILPOINTS=bar::foo=panic cargo run
```

In unit tests:

```
#[test]
fn test_foo() {
    fail::cfg("bar::foo", "panic");
    foo();
}
```

## To DO

Triggering a fail point via the HTTP API is planned but not implemented yet.
