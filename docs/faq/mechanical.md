---
layout: default
parent: FAQ
title: Mechanical options
---

## `--num-threads`
Spawn a threadpool of `N` woker threads.
The default is `0`, meaning all available cores in the system.

## `--chunk-size`
By default, each thread consumes `N` ZMWs per chunk for processing.
The default is `10`.

## `--no-bam`
Do not produce BAM output, nor PBI. Useful if only the reports are of interest,
as time to results is lower.

## `--no-reports`
Do not produce any reports. Useful if only the demultiplexed BAM is of interest.

## `--keep-idx-order`
Per default, the two identified barcode idx are sorted ascending, as in CLR data,
the correct order cannot be determined. This affects the `bc` tag, `prefix.counts`
file, and `--split-bam` file names; `prefix.report` columns `IdxLowest`,
`IdxHighest`, `IdxLowestNamed`, `IdxHighestNamed` will have the same order as
`IdxFirst` and `IdxCombined`. This option only makes sense for single read data,
such as CCS.

If you are using an asymmetric barcode design with `NxN` pairs
and your input is CCS, you can use `--keep-idx-order` to preserve
the order. If your input is CLR subreads and you use `NxN` asymmetric pairs,
there is no way to distinguish between pairs `bc1001--bc1002` and `bc1002--bc1001`.
