# PyO3 Sub Interpreter Broken Reproduce

Hi, this repo served as a reproduce for the issue I'm facing with PyO3 and Sub Interpreter.

Most of code are ported from [arrow-udf](https://github.com/arrow-udf/arrow-udf).

## Context

Arrow UDF is a User-Defined Functions Framework that allows users to easily create and run user-defined functions (UDF) on Apache Arrow.

We use pyo3 to create a sub-interpreter and run users' UDFs within it to avoid the global GIL. It used to work fine, but we discovered it broke after the `0.22` release, specifically after [this commit](https://github.com/PyO3/pyo3/commit/1c64a03ea084852572872c0d6b5fd029f116c807).

Upstream issue: https://github.com/PyO3/pyo3/issues/4570

## Reproduce

In the `Cargo.toml`, there are several pyo3 versions with comments. You can uncomment them and run `cargo test`.

For example:

with `0.21`, we will have:

```shell
running 1 test
test tests::test_forbid ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.02s
```

with `0.22`, we will have:

```shell
running 1 test
error: test failed, to rerun pass `--lib`

Caused by:
  process didn't exit successfully: `/home/xuanwo/Code/xuanwo/pyo3-sub-interpreter-broken/target/debug/deps/pyo3_sub_interpreter_broken-a4a62a805e1d13b9` (signal: 11, SIGSEGV: invalid memory reference)
cargo test  5.03s user 0.34s system 80% cpu 6.717 total
```

## More Information

This repo is not a full reproduce, for more information, please refer to [this PR](https://github.com/arrow-udf/arrow-udf/pull/70) which is also easy to reproduce by running `cargo test`.