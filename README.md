<!-- Copyright (c) 2017-2018 Twitter, Inc. -->
<!-- Licensed under the Apache License, Version 2.0 (see LICENSE.md). -->
<!-- Autogenerated by https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bin/bench_upload from README.tmpl. -->

# Reasonable Scala Compiler Performance

Reasonable Scala compiler (also known as Rsc) is an experimental Scala compiler
focused on compilation speed. This project is developed by Eugene Burmako
and his team at Twitter.

Our research goal is to achieve dramatic compilation speedups (5-10x)
for typical Scala codebases, and we are currently well on track to reaching
this goal.

In this document, we aim to publish the results of running our benchmark suite
on the most recent commit in our repository. Since running benchmarks takes time,
there may be short periods of time when this document is out of date. If you're
curious about the exact version of Rsc that is benchmarked in this document,
[click here](https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895).

## Hardware

All benchmarks run on a computer with
Intel(R) Core(TM) i7-4770 CPU @ 3.40GHz (4x32KB+32KB L1 cache,
4x256KB L2 cache, 8192KB L3 cache,
configured to run 4 physical cores and
4 logical cores with Turbo Boost disabled),
31GB RAM and Crucial_CT240M50 disk drive.

## Software

In our benchmarks, we use Debian GNU/Linux 9.3 (stretch) and Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
to run [Rsc 99f55744](https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895), Scalac 2.11.12, Scalac 2.12.4
and javac 1.8.0_151.
To benchmark native applications, we use [our own microbenchmark harness](https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/rsc/shared/src/main/scala/rsc/bench/CliBench.scala).
To benchmark JVM applications, we use sbt-jmh 0.2.25 that runs in sbt 0.13.16.

Our benchmarks run different fragments of compilation pipelines of
different compilers on two comparable codebases:
  * [re2j](https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/examples/re2j/src/main/java/java/util/regex), an implementation
    of linear time regular expression matching in Java (11724 loc).
  * [re2s](https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/examples/re2s/src/main/scala/java/util/regex), a port of re2j
    to Scala [performed in Scala Native](https://github.com/scala-native/scala-native/pull/894).
    For Rsc, re2s is accompanied by [Stdlib.scala](https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/stdlib/src/main/scala/Stdlib.scala),
    a source file that declares stubs for referenced definitions from
    scala-library and the JDK (11028 loc + 182 loc =
    11210 loc).

Every benchmark runs in two different modes: cold (performance
of a single run without warm-up) and hot (performance of a steady state achieved
by doing a large number of runs).

## Disclaimer

At this point, Rsc only implements a subset of functionality provided by the
Scala compiler. This means that the benchmark results provided below must
be interpreted with utmost care. Concretely:
  * Performance numbers may significantly deteriorate as we will be
    implementing more and more functionality of the Scala compiler.
    For example, adding support for classpath loading or implicit search
    is very likely to slow down our compiler by a significant factor.
  * Direct comparisons of Rsc and Scalac performance numbers should take
    into account similarities and differences in provided functionality.
    Consult [the summary in the "Compiler" document](https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/docs/compiler.md#summary)
    for more information.

## Results

To reproduce, run `bin/bench` (this will take a while).

<table>
  <th>
    <td>Cold</td>
    <td>Hot</td>
  </th>
  <tr>
    <td width="208px"><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/rsc/jvm/src/main/scala/rsc/bench/RscNativeSchedule.scala">RscNativeSchedule</a></td>
    <td width="208px">48.770 ms</td>
    <td width="208px">34.451 ms</td>
  </tr>
  <tr>
    <td><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/rsc/jvm/src/main/scala/rsc/bench/RscSchedule.scala">RscSchedule</a></td>
    <td>318.599 ms</td>
    <td>9.535 ms</td>
  </tr>
  <tr>
    <td><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/scalac211/src/main/scala/rsc/bench/ScalacNamer211.scala">ScalacNamer211</a></td>
    <td>1179.715 ms</td>
    <td>62.111 ms</td>
  </tr>
  <tr>
    <td><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/scalac212/src/main/scala/rsc/bench/ScalacNamer212.scala">ScalacNamer212</a></td>
    <td>1642.299 ms</td>
    <td>27.683 ms</td>
  </tr>
</table>

<table>
  <th>
    <td>Cold</td>
    <td>Hot</td>
  </th>
  <tr>
    <td width="208px"><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/rsc/jvm/src/main/scala/rsc/bench/RscNativeTypecheck.scala">RscNativeTypecheck</a></td>
    <td width="208px">91.561 ms</td>
    <td width="208px">63.438 ms</td>
  </tr>
  <tr>
    <td><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/rsc/jvm/src/main/scala/rsc/bench/RscTypecheck.scala">RscTypecheck</a></td>
    <td>427.062 ms</td>
    <td>24.922 ms</td>
  </tr>
  <tr>
    <td><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/scalac211/src/main/scala/rsc/bench/ScalacTyper211.scala">ScalacTyper211</a></td>
    <td>4295.242 ms</td>
    <td>707.156 ms</td>
  </tr>
  <tr>
    <td><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/scalac212/src/main/scala/rsc/bench/ScalacTyper212.scala">ScalacTyper212</a></td>
    <td>5167.287 ms</td>
    <td>610.896 ms</td>
  </tr>
</table>

<table>
  <th>
    <td>Cold</td>
    <td>Hot</td>
  </th>
  <tr>
    <td width="208px"><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/scalac211/src/main/scala/rsc/bench/ScalacCompile211.scala">ScalacCompile211</a></td>
    <td width="208px">8047.402 ms</td>
    <td width="208px">1702.511 ms</td>
  </tr>
  <tr>
    <td><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/scalac212/src/main/scala/rsc/bench/ScalacCompile212.scala">ScalacCompile212</a></td>
    <td>9456.717 ms</td>
    <td>1630.761 ms</td>
  </tr>
  <tr>
    <td><a href="https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/bench/javac18/src/main/scala/rsc/bench/JavacCompile.scala">JavacCompile</a></td>
    <td>801.029 ms</td>
    <td>73.772 ms</td>
  </tr>
</table>

## Summary

  * First and foremost, the current prototype of Rsc is significantly faster
    than full Scalac in both cold and hot benchmarks. Most impressively,
    hot typechecking is ~26x faster
    [with the disclaimer provided above](README.md#disclaimer).
  * Scala Native has clearly succeeded in its goal of speeding
    up startup time of Scala applications. In cold benchmarks that are
    representative of running programs from command line, Rsc Native has
    a ~4.7x edge over vanilla Rsc.
  * Finally, it was interesting to see that the current prototype of Rsc
    typechecks re2s ~3x faster than Javac compiles re2j,
    [given the disclaimer provided above](README.md#disclaimer).
    As we will be adding more features to Rsc, we will be keeping an eye on how
    this will affect compilation performance relative to Javac.
  * In the benchmarks above, all compilers are run in single-threaded mode.
    However, unlike Scalac and Javac that are inherently single-threaded,
    [Rsc was designed to enable massive parallelism](https://github.com/twitter/reasonable-scala/tree/99f55744fe31181544ff563b7344ac22287da895/docs/compiler.md).
    In the near future, we plan to leverage this unique feature of Rsc and
    parallelize its pipeline.
