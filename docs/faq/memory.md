---
layout: default
parent: FAQ
title: Memory
---

## Why is the memory consumption really high?
Most likely this is due to `--split-bam` or `--split-bam-named` mode.
The latter is activated per default in SMRT Link.
*Lima* is able to stream up to 500 barcode pairs to individual split BAM files.
If more than 500 barcode pairs are detected, additional output is buffered first.
In this case, memory usage (RES column in top) is approximately the size of the
input, uncompressed.

The maximum concurrent output BAM file handles can be adjusted with
`--bam-handles N`. The default is 500.

Examples, how memory usage is affected by `--bam-handles`. Option
`--bam-handles-verbose` is only used to visualize the BAM output file handles.
Memory usage reported using [memusg](https://gist.github.com/netj/526585):

    $ lima input.bam barcodes.fasta out.bam --same --split-bam --bam-handles 9 --bam-handles-verbose
    Open stream 7--7
    Open stream 3--3
    Open stream 5--5
    Open stream 1--1
    Open stream 4--4
    Open stream 6--6
    Open stream 0--0
    Open stream 2--2
    Open stream 210--210
    memusg: peak=86,728

    $ lima input.bam barcodes.fasta out.bam --same --split-bam --bam-handles 4 --bam-handles-verbose
    Open stream 7--7
    Open stream 3--3
    Open stream 5--5
    Open stream 1--1
    Buffered stream 0--0
    Buffered stream 2--2
    Buffered stream 4--4
    Buffered stream 6--6
    Buffered stream 210--210
    memusg: peak=113,476

    $ lima input.bam barcodes.fasta out.bam --same --split-bam --bam-handles 0 --bam-handles-verbose
    Buffered stream 0--0
    Buffered stream 1--1
    Buffered stream 2--2
    Buffered stream 3--3
    Buffered stream 4--4
    Buffered stream 5--5
    Buffered stream 6--6
    Buffered stream 7--7
    Buffered stream 210--210
    memusg: peak=132,276

## What is the expected peak RSS for a single output file?
Expect a few hundred MBytes peak RSS.

## What is the expected peak RSS for output file splitting?
For a 2.5M HiFi reads run, yielding ~1800 barcode pairs, running on 256 threads:

|     --bam-handles     | Peak RSS |
| :-------------------: | :------: |
| 0 (buffer everything) |  6.7 GB  |
|     500 (default)     |  1.9 GB  |
|         1000          |  2.2 GB  |
|         2000          |  3.3 GB  |
