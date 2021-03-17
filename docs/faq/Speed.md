---
layout: default
parent: FAQ
title: Speed
---

## How fast is fast?
Example: 200 barcodes, asymmetric mode (try each barcode forward and
reverse-complement), 300,000 CCS reads. On my 2014 iMac with 4 cores + HT:

    503.57s user 11.74s system 725% cpu 1:11.01 total

Those 1:11 minutes translate into 0.233 milliseconds per ZMW,
1.16 microseconds per barcode for both sides aligning forward and reverse-complement,
and 291 nanoseconds per alignment. This includes IO.

## Why doesn't *lima* utilize the maximum number of provided cores?
This might be a simple IO bottleneck. With a barcode.fasta containing only a few
barcodes, most of the time is spent reading and writing BAM files, as the barcode
identification is too fast. Starting version 2.2.0, you can enable multi-threaded
BAM reading by setting the number of threads via an environment variable

    export PB_BAMREADER_THREADS=2

## Is there a way to show the progress?
No. Please run `wc -l prefix.report` to get the number of already processed ZMWs.
