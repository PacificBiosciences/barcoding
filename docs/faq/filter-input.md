---
layout: default
parent: FAQ
title: Filter - Input
---

## `--min-length`
Reads with length below `N` bp after demultiplexing are omitted. The default is
`50`. ZMWs with no reads passing are omitted.

## `--max-input-length`
Reads with length above `N` bp are omitted for scoring in the demultiplexing
step. The default is `0`, meaning deactivated.

## `--min-score`
Threshold for the average barcode score of the leading and trailing ends.
ZMWs with barcode score below or equal to `N` are omitted. The default is `0`.
It is advised to set it to `26` for CLR and `80` for HiFi.

## `--min-passes`
ZMWs with less than `N` full passes, a read with a leading and
trailing adapter, are omitted. This is only useful for CLR data. CCS / HiFi data
ignore this option. The default is `0`, no full-pass needed. Example

    0 pass  : insert - adapter - insert
    1 pass  : insert - adapter - INSERT - adapter - insert
    2 passes: insert - adapter - INSERT - adapter - INSERT - adapter - insert
