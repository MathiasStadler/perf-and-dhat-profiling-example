# NICE TO KNOW

## [from here](https://stackoverflow.com/questions/50544499/how-to-make-a-styled-markdown-admonition-box-in-a-github-gist/72327818#72327818)

> **⚠️ Warning**
>
> You shouldn't. This is irreversible!
> **❌ Error**
>
> Don't do that. This is irreversible!
> **ℹ️ Information**
>
> You can do that without problem.
> **✅ Success**
>
> Don't hesitate to do that.
> **🦄 New line support**
>
> It supports new lines:
>
> .. simply use an empty `>` line

## [github markdown](https://docs.github.com/de/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

---

```bash
 perf stat -ad -r 100 target/release/perf-and-dhat-profiling-example test.csv
 sudo perf record -e L1-dcache-loads,LLC-load-misses --call-graph dwarf -- target/release/perf-and-dhat-profiling-example test.csv
cargo install --force inferno
perf script | inferno-collapse-perf > stacks.folded
perf record -e L1-dcache-loads,LLC-load --call-graph dwarf -- target/release/perf-and-dhat-profiling-example test.csv
sudo perf script | inferno-collapse-perf > stacks.folded
```
