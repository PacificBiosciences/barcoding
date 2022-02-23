---
layout: default
parent: FAQ
title: Undo
---

## Undo demultiplexing
With the introduction of *lima* v2.5.0, it is possible to undo all
demultiplexing steps for **HiFi data**. For this, the bioconda package contains a
new `lima-undo` binary.

Example:

    lima movie.hifi_reads.bam demux.consensusreadset.xml --hifi-preset SYMMETRIC --store-unbarcoded
    lima-undo demux.consensusreadset.xml undo.bam

Let's unroll what's happening. In the first line, we explicitly request to store
the unbarcoded reads. Without this, we would not be able to recover unbarcoded
reads. The `XML` contains all the file paths to the `BAM` files. The second call is
to the new *lima-undo* binary that takes a `XML` or `BAM` file as input and
ouput.

Optionally, you can also provide multiple input `BAM` files with one output `BAM`:

    lima-undo demux.bam demux.unbarcoded.bam undo.bam

This works also with split BAM files:

    lima-undo demux.bc1001-bc1001.bam demux.bc1002-bc1002.bam demux.unbarcoded.bam undo.bam

## How does it work?
*lima* v2.5.0 and later stores everything that got clipped in an internal binary
structure in the `ls` tag. Multiple demultiplexing rounds are supported. Once
*lima-undo* gets called, for each read the individual demultiplexing steps get
reverted until the read is identical to the original HiFi read.

## How can I check if undo results are correct?
How to check that the result is identical:

    samtools sort --no-PG -t "zm" undo.bam -o sorted.undo.bam
    samtools view --no-PG sorted.undo.bam > undo.sam
    samtools view --no-PG movie.hifi_reads.bam > original.sam
    diff original.sam undo.sam
