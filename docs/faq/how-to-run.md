---
layout: default
parent: FAQ
title: How to run
---

# How to run

**Notes:**
 1. Any existing output files will be overwritten after execution.
 2. Always use `--peek-guess` to remove spurious barcode hits.

Run on CLR subread data:

    $ lima <movie>.subreads.bam <barcodes>.fasta <demux>.bam
    $ lima <movie>.subreadset.xml <barcodes>.barcodeset.xml <demux>.subreadset.xml

Run on CCS / HiFi data:

    $ lima <movie>.ccs.bam <barcodes>.fasta <demux>.bam
    $ lima <movie>.consensusreadset.xml <barcodes>.barcodeset.xml <demux>.consensusreadset.xml

### *Symmetric* or *Tailed* options

    CLR: --same
    CCS: --preset-hifi SYMMETRIC

### *Asymmetric* options

    CLR: --different
    CCS: --preset-hifi ASYMMETRIC

### Example execution

    $ lima m54317_180718_075644.subreadset.xml Sequel_RSII_384_barcodes_v1.barcodeset.xml \
           m54317_180718_075644.demux.subreadset.xml --different --peek-guess

## Workflow
*Lima* processes input reads grouped by ZMW, except if `--per-read` is chosen.
All barcode regions along the read are processed individually.
The final per-ZMW result is a [summary](#implementation-details) over all barcode regions,
a pair of selected barcodes from the provided set of candidate barcodes;
subreads from the same ZMW will have the same barcode and barcode quality.
For a particular target barcode region, every barcode sequence gets aligned
as given and as reverse-complement, and higher scoring orientation is chosen;
the result is a list of scores over all candidate barcodes.

If only *same* barcode pairs are of interest, *symmetric/tailed*, please use
`--same` to filter out *different* barcode pairs.

If only *different* barcode pairs are of interest, *asymmetric*, please use
`--different` to require at least two barcodes to be read and remove pairs with
the same barcode.
