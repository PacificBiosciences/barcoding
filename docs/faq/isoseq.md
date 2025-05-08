---
layout: default
parent: FAQ
title: Iso-Seq
---

## How can I demultiplex IsoSeq data?
Even if you only want to remove IsoSeq primers, *lima* is the tool of choice.

1. Remove all duplicate sequences.
2. Annotate sequence names with a `5p` or `3p` suffix. Example:
```
    >primer_5p
    AAGCAGTGGTATCAACGCAGAGTACATGGGG
    >sample_brain_3p
    AAGCAGTGGTATCAACGCAGAGTACCACATATCAGAGTGCG
    >sample_liver_3p
    AAGCAGTGGTATCAACGCAGAGTACACACACAGACTGTGAG
```
3. Use the `--isoseq` mode. Run in combination with `--peek-guess` to remove spurious false positive.
4. Output will be only different pairs with a `5p` and `3p` combination:
```
    demux.primer_5p--sample_brain_3p.bam
    demux.primer_5p--sample_liver_3p.bam
```
The order of the output bam file name suffix is based on the order of the primers in the input fasta.
To guarantee file names be consistently labeled `demux.{prefix}.primer_5p--primer_3p.bam`, ensure the input fasta is in the order 5' to 3'.

Those options are very conservative to remove any spurious and ambiguous
calls, in order to guarantee that only proper asymmetric (barcoded) primer
are used in downstream analyses. Good libraries reach >75% CCS reads passing
*lima* filters.

## Demultiplexing cDNA barcoded adapters after SMRTbell adapter-level demultiplexing
[Iso-Seq](https://isoseq.how/clustering/cli-workflow.html#step-2---primer-removal-and-demultiplexing)
supports pooled cDNA barcoded analysis. If using barcoded cDNA primers after adapter-level demultiplexing,
add `--overwrite-biosample-names` to replace the [bio sample names](https://lima.how/faq/biosample.html) assigned during the first round of
demultiplexing.