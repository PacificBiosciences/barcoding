---
layout: default
parent: FAQ
title: Mechanical options
---

# Mechanical options

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
