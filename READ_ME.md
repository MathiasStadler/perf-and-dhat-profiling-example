# [in this repo will i follow this article](https://www.justanotherdot.com/posts/profiling-with-perf-and-dhat-on-rust-code-in-linux.html)

## preparation

### install nightly build

```bash
rustup toolchain install  nightly-x86_64-unknown-linux-gnu
```

### set default nightly

```bash
rustup default nightly
```

### switch from stable to nightly

```bash
# show which version is default
rustup default

# switch to nightly
rustup default nightly

# switch back to stable
rustup default stable

```

### Show which toolchain will be used in the current directory

[[from here](https://rust-lang.github.io/rustup/examples.html)]

```bash
rustup show
```

### Keeping Rust up to date stable and nightly

[[from here](https://rust-lang.github.io/rustup/basics.html)]

```bash
rustup update
```

### install ubuntu perf iotg (cli)

```bash
sudo apt install linux-intel-iotg-tools-common
```

## if the iotg version wrong on your ubuntu

> fix it, delete it :-)

```bash
sudo apt purge --remove linux-intel-iotg-tools-common linux-tools-common linux-tools-5.15.0-76 linux-tools-5.15.0-76-generic  linux-tools-generic

```

### install standard version perf for ubuntu

```bash
sudo apt install linux-tools-common
```

### install additional package ATTENTION this package must match the installed kernel

check so your running kernel

> cat /proc/version
>
> > 'Linux version 5.19.0-46-generic (buildd@lcy02-amd64-025) (x86_64-linux-gnu-gcc (Ubuntu 11.3.0-1ubuntu1~22.04.1) 11.3.0, GNU ld (GNU Binutils for Ubuntu) 2.38) #47~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Wed Jun 21 15:35:31 UTC 2'

check in your distro have you the right package

> echo linux-tools-$(uname -r)
> apt-cache show linux-tools-$(uname -r)
> echo linux-cloud-tools-$(uname -r)
> apt-cache show linux-cloud-tools-$(uname -r)

for example :

```bash
sudo apt install linux-tools-5.19.0-46-generic linux-cloud-tools-5.19.0-46-generic
```

## start with perf

### set global setting for perf: Allow use of (almost) all events by all users Ignore mlock limit after perf_event_mlock_kb without CAP_IPC_LOCK

```bash
echo -1 | sudo tee /proc/sys/kernel/perf_event_paranoid
```

```bash
export RUSTFLAGS="-C target-cpu=native"
echo $RUSTFLAGS
```

### hyperfine

- We use the tool hyperfine to **compare** multiple targets ( debug/target) of measurement at the same time

> Before must we compile both targets
>
> > `cargo build`
> >
> > `cargo build --release`

- debug

  -path/file: `target/debug/perf-and-dhat-profiling-example`

- release

  -path/file: `target/release/perf-and-dhat-profiling-example`

:+1:

- run hyperfine testcase

```bash
export RUSTFLAGS="-C target-cpu=native" && echo $RUSTFLAGS
hyperfine --show-output "target/debug/perf-and-dhat-profiling-example test.csv" "target/release/perf-and-dhat-profiling-example test.csv"
```

Summary: We can see the difference

````bash
Summary
  target/release/perf-and-dhat-profiling-example test.csv ran
    4.59 ± 1.42 times faster than target/debug/perf-and-dhat-profiling-example test.csv
    ```



### test perf stat s.[[webpage](https://www.justanotherdot.com/posts/profiling-with-perf-and-dhat-on-rust-code-in-linux.html)] and look for "Describing key metrics with perf stat"

```bash
perf stat -ad -r 100 target/release/perf-and-dhat-profiling-example test.csv 
````

### output

![alt text for screen readers](md_parts/perf-output.png "Text to show on mouseover")

**Error**: Invalid event (LLC-load-misses)

```bash
perf record \
-e L1-dcache-loads,LLC-load-misses \
--call-graph dwarf \
-- target/release/perf-and-dhat-profiling-example \
test.csv
```

> [see this desc last comment](https://stackoverflow.com/questions/62821668/why-wont-perf-report-dcache-store-misses)
> LLC (last level cache)

**Error**: Invalid event (LLC-load-misses) in per-thread mode, enable system wide with '-a'

**Note**: perf list always list all events defined in kernel.

```bash
perf record \
-e L1-dcache-loads,LLC-loads \
--call-graph dwarf \
-- target/release/perf-and-dhat-profiling-example \
test.csv
```

### Digging deeper with perf record and perf report

#### Run `perf record`` as root

`sudo perf record -e L1-dcache-loads,LLC-load --call-graph dwarf -- target/release/perf-and-dhat-profiling-example test.csv`

#### Run `sudo perf report`

