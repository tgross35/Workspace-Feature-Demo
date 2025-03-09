Demonstration of different feature enablement between crate and virtual
workspaces.

`demo` is the root crate. The `sets-features` crates enables feature `feat` in
`demo`, and asserts it is set. `demo/examples/ex` does the opposite and asserts
that `feat` is  unset. Both of these succeed:

```text
$ cd crate-ws/demo/
$ cargo t
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running unittests src/lib.rs (target/debug/deps/demo-0e2d8be017cbcd93)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

$ cargo run --example ex
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/examples/ex`
```

However, changing to a virtual workspace seems to mean that `feat` also gets
enabled when running the example, causing it to fail:

  
```text
$ cd ../../virtual-ws/
$ cargo t
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running unittests src/lib.rs (target/debug/deps/demo-b1ea6d616e2c7c2c)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

     Running unittests src/lib.rs (target/debug/deps/sets_features-cf495e095412863e)

running 1 test
test t ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

$ cargo run --example ex
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/examples/ex`

thread 'main' panicked at demo/examples/ex.rs:2:5:
assertion failed: !demo::FEAT_IS_SET
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
````
