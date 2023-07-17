# [in this repo will i follow this article](https://www.justanotherdot.com/posts/profiling-with-perf-and-dhat-on-rust-code-in-linux.html)

## switch to nightly build

```bash
rustup toolchain install  nightly-x86_64-unknown-linux-gnu
```

## set another default

```bash
rustup default nightly
```

## switch back stable and nightly

```bash
# show which  version is default
rustup default

# switch to nightly
rustup default nightly

# switch back to stable
rustup default stable

```

## Show which toolchain will be used in the current directory

[[from here](https://rust-lang.github.io/rustup/examples.html)]

```bash
rustup show
```
