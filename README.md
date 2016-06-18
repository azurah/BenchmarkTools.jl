Note: Some type definitions were changed between BenchmarkTools v0.0.2 and v0.0.3. To deserialize old JLD-formatted data using a newer version of BenchmarkTools, use `BenchmarkTools.loadold(args...)` instead of `JLD.load(args...)`.

# BenchmarkTools.jl

[![Build Status](https://travis-ci.org/JuliaCI/BenchmarkTools.jl.svg?branch=master)](https://travis-ci.org/JuliaCI/BenchmarkTools.jl)
[![Coverage Status](https://coveralls.io/repos/github/JuliaCI/BenchmarkTools.jl/badge.svg?branch=master)](https://coveralls.io/github/JuliaCI/BenchmarkTools.jl?branch=master)

BenchmarkTools makes **performance tracking of Julia code easy** by supplying a framework for **writing and running groups of benchmarks** as well as **comparing benchmark results**.

This package is used to write and run the benchmarks found in [BaseBenchmarks.jl](https://github.com/JuliaCI/BaseBenchmarks.jl).

The CI infrastructure for automated performance testing of the Julia language is not in this package, but can be found in [Nanosoldier.jl](https://github.com/JuliaCI/Nanosoldier.jl).

## Installation

To install BenchmarkTools, you can run the following:

```julia
Pkg.add("BenchmarkTools")
```

## Documentation

If you're just getting started, check out the [manual](doc/manual.md) for a thorough explanation of BenchmarkTools.

If you want to explore the BenchmarkTools API, see the [reference document](doc/reference.md).

If you want a short example of a toy benchmark suite, see the sample file in this repo ([benchmark/benchmarks.jl](benchmark/benchmarks.jl)).

If you want an extensive example of a benchmark suite being used in the real world, you can look at the source code of [BaseBenchmarks.jl](https://github.com/JuliaCI/BaseBenchmarks.jl/tree/nanosoldier).

If you're benchmarking on Linux, I wrote up a series of [tips and tricks](https://github.com/JuliaCI/BenchmarkTools.jl/blob/master/doc/linuxtips.md) to help eliminate noise during performance tests.

## Why does this package exist?

Our story begins with two packages, "Benchmarks" and "BenchmarkTrackers". The Benchmarks package implemented an execution strategy for collecting and summarizing individual benchmark results, while BenchmarkTrackers implemented a framework for organizing, running, and determining regressions of groups of benchmarks. Under the hood, BenchmarkTrackers relied on Benchmarks for actual benchmark execution.

For a while, the Benchmarks + BenchmarkTrackers system was used for automated performance testing of Julia's Base library. It soon became apparent that the system suffered from a variety of issues:

1. Individual sample noise could significantly change the execution strategy used to collect further samples.
2. The estimates used to characterize benchmark results and to detect regressions were statistically vulnerable to noise (i.e. not robust).
3. Different benchmarks have different noise tolerances, but there was no way to tune this parameter on a per-benchmark basis.
4. Running benchmarks took a long time - an order of magnitude longer than theoretically necessary for many functions.
5. Using the system in the REPL (for example, to reproduce regressions locally) was often cumbersome.

The BenchmarkTools package is a response to these issues, designed by examining user reports and the benchmark data generated by the old system. BenchmarkTools offers the following solutions to the corresponding issues above:

1. Benchmark execution parameters are configured separately from the execution of the benchmark itself. This means that subsequent experiments are performed more consistently, avoiding branching "substrategies" based on small numbers of samples.
2. A variety of simple estimators are supported, and the user can pick which one to use for regression detection.
3. Noise tolerance has been made a per-benchmark configuration parameter.
4. Benchmark configuration parameters can be easily cached and reloaded, significantly reducing benchmark execution time.
5. The API is simpler, more transparent, and overall easier to use.

## Acknowledgements

This package was authored primarily by Jarrett Revels (@jrevels). Additionally, I'd like to thank the following people:

- John Myles White, for authoring the original Benchmarks package, which greatly inspired BenchmarkTools
- Andreas Noack, for statistics help and investigating weird benchmark time distributions
- Oscar Blumberg, for discussions on noise robustness
- Jiahao Chen, for discussions on error analysis
