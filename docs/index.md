---
layout: default
title: Lima Home
nav_order: 1
description: "The PacBio Barcode Demultiplexing and Primer Removal Software."
permalink: /
---

<p align="center">
  <img src="img/lima_card.png" alt="lima logo" width="650px"/>
</p>

***

_lima_ is the standard tool to identify barcode and primer sequences in PacBio
single-molecule sequencing data. It powers the Demultiplex Barcodes, Iso-Seq,
and Mark PCR Duplicates GUI-based analysis applications.

## Availability
The latest `lima` can be installed via the bioconda package `lima`.

Please refer to our [official pbbioconda page](https://github.com/PacificBiosciences/pbbioconda)
for information on Installation, Support, License, Copyright, and Disclaimer.

## Latest Version
Version **2.1.0**: [Full changelog here](/changelog)

## What's new!
New documentation is up, a 1:1 port from the original GitHub docs with minor
enhancements. Expect major enhancements in upcoming releases.

## Execution
**Input**: Subreads or HiFi reads from a single movie and barcodes in FASTA format.

**Output**: Demultiplexed reads in a format inferred from the file extension.

Run on a full movie with **symmetric** barcodes:

    lima movie.hifi_reads.bam barcodes.fasta demux.bam --same --ccs --min-score 80

Run on a full movie with **asymmetric** barcodes:

    lima movie.hifi_reads.bam barcodes.fasta demux.bam --different --ccs --min-score 80
