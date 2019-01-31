---
layout: default
title: Analyzing fuzzer performance
permalink: /using-clusterfuzz/workflows/analyzing-fuzzing-performance/
nav_order: 3
parent: Workflows
grand_parent: Using ClusterFuzz
---

# Analyzing fuzzer performance

ClusterFuzz automates fuzzing as much as possible, but it's responsibility of
the users to write and maintain fuzzers in order to find security
vulnerabilities and other software bugs. This page gives some recommendations on
how to analyze performance of the fuzzers running on ClusterFuzz infrastructure.

- TOC
{:toc}

## When to analyze fuzzer performance

It's important to regularly monitor the performance of fuzzers, especially after
a new fuzzer is created. If a fuzzer keeps finding new issues, it might be more
important to prioritize fixing those issues first, but if a fuzzer has not
reported anything for a while, that is a strong signal that you need to check
its performance.

## Performance factors

* Speed is crucial for fuzzing. There is no minimum threshold. The faster a
fuzzer generates testcases, the better.
* Code coverage should grow over time. A fuzzer should be continuously
generating new "interesting" testcases that exercise various parts of the target
program.
* Blocking issues should be resolved. If a fuzzer frequently reports a Timeout,
Out-of-Memory, or other crashes, it will be blocking fuzzer's progress.

## Fuzzer stats

The fuzzer stats page provides various metrics on fuzzer performance. Using the
filters on the page, you can see how those metrics (e.g. execution speed or
number of crashes) change over time (if you choose "Group by Day") or compare
different fuzzers to each other ("Group by Fuzzer"). There is also "Group by
Time" filter that shows fuzzer stats as charts rather than raw numbers.

This feature requires [production setup]({{ site.baseurl }}/production-setup/),
as fuzzer stats are stored in BigQuery. The stats are usually delayed by up to
24 hours, as data is uploaded to BigQuery once a day.

## Performance report

ClusterFuzz provides automatically generated performance reports that identify
performance issues and give recommendations on how those issues can be resolved.
The reports also prioritize the issues detected and provide fuzzer log examples.

This feature currently only works with fuzz targets using libFuzzer fuzzing
engine.

## Coverage report

Code coverage is a very important metric for evaluating performance of a fuzzer.
Looking at the code coverage report, you can see which exact parts of the target
program are tested by the fuzzer and which parts are never executed. If you set
up a [code coverage builder](/using-clusterfuzz/advanced/code-coverage/) for
ClusterFuzz, you can find links to the coverage reports on the fuzzer stats
page. Otherwise, you can generate code coverage reports locally. For C and C++
targets, we recommend using [Clang Source-based Code Coverage].

## Fuzzer logs

If none of the above gives you enough information about performance of a fuzzer,
looking into the fuzzer logs will be the next step. On the fuzzer stats page,
you can find a link to the GCS bucket storing the logs. You can also manually
navigate to that bucket using either
[gsutil](https://cloud.google.com/storage/docs/gsutil) tool or Google Cloud
Storage web interface.

[Clang Source-based Code Coverage]: https://clang.llvm.org/docs/SourceBasedCodeCoverage.html