---
layout: default
parent: FAQ
title: Split output
---

## Can I split my data by barcode?
You can either iterate over the `prefix.bam` file N times or use
`--split-bam`. Each barcode has its own BAM file called
`prefix.idxBest--idxCombined.bam`, e.g., `prefix.0--0.bam`.

The optional parameter `--split-named`, names the files by their barcode names instead
of their barcode indices. Non-word characters, anything except [A-Za-z0-9_],
in barcode names are replaced with an underscore in the file name.

This mode might consume more memory. Read the next FAQ entry for more information.

In addition, a `prefix.datastore.json` is generated to wrap the individual dataset
files.

## How do barcode indices correspond to the input sequences?
Input barcode sequences are tagged with an incrementing counter. The first
sequence is barcode `0` and the last barcode `numBarcodes - 1`.

## Can I limit the output files per directory?
If you use output BAM splitting, it can happen that you get a lot of output files.
Using `--files-per-directory N` creates subdirectories and outputs at most `N`
barcodes per directory.

## Split barcodes into own sub-directories
Since v2.5.0 each barcode can be stored in its own sub-directory: `--split-subdirs`.
A parent XML will point to all of the barcoded files.

## Output missing barcodes
If you have provided bio samples with barcode pairs, option `--output-missing-pairs`
allows to create empty barcode files in all split modes.
