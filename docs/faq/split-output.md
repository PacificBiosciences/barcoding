---
layout: default
parent: FAQ
title: Split output
---

## Can I split my data by barcode?
You can either iterate over the `prefix.bam` file N times or use
`--split-bam`. Each barcode has its own BAM file called
`prefix.idxBest--idxCombined.bam`, e.g., `prefix.0--0.bam`.

The optional parameter `--split-bam-named`, names the files by their barcode names instead
of their barcode indices. Non-word characters, anything except [A-Za-z0-9_],
in barcode names are replaced with an underscore in the file name.

This mode might consume more memory. Read the next FAQ entry for more information.

In addition, a `prefix.datastore.json` is generated to wrap the individual dataset
files.

## How do barcode indices correspond to the input sequences?
Input barcode sequences are tagged with an incrementing counter. The first
sequence is barcode `0` and the last barcode `numBarcodes - 1`.
