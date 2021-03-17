---
layout: default
title: Get started
nav_order: 4
---

# Get started
## Input

### Reads

Reads can be in following formats:

| Data type  |      File type       |            Example             |
| :--------: | :------------------: | :----------------------------: |
|    CLR     | unaligned PacBio BAM |     `<movie>.subreads.bam`     |
|    CCS     | unaligned PacBio BAM |      `<movie>.reads.bam`       |
|    HiFi    | unaligned PacBio BAM |    `<movie>.hifi_reads.bam`    |
|    CLR     |         XML          |    `<movie>.subreadset.xml`    |
| CCS / HiFi |         XML          | `<movie>.consensusreadset.xml` |
| CCS / HiFi |   FASTA [opt gzip]   |      `<movie>.fasta[.gz]`      |
| CCS / HiFi |   FASTQ [opt gzip]   |      `<movie>.fastq[.gz]`      |

### Barcodes
Barcodes have to be in FASTA format `<barcodes>.fasta`, one entry per barcode
sequence, **no duplicate** sequences, only upper-case bases, orientation
agnostic (forward or reverse-complement, but **NOT** reversed). Example:

    >bc1000
    CTCTACTTACTTACTG
    >bc1001
    GTCGTATCATCATGTA
    >bc1002
    AATATACCTATCATTA

Please name your barcodes with an alphabetic character prefix to avoid
later confusion of barcode name and index. Duplicate names or sequences
are not permitted.

## Output

It is advised to use the same output as input file type. [This compatibility
matrix]() explains what in- and output combinations are possible.

Following additional auxilliary files are generated:
 * `<prefix>.lima.summary`, a human-readable summary of barcoded yield and failures
 * `<prefix>.lima.report`, in-depth diagnostics for each ZMW
 * `<prefix>.lima.counts`, ZMW counts per barcode pair and mean barcode score
 * `<prefix>.lima.clips`, clipped barcode regions with `--dump-clips`
 * `<prefix>.lima.guess`, barcode pairs and if they were selected with `--guess` / `--peek-guess`
 * `<prefix>.removed.bam`, unbarcoded ZMWs with `--dump-removed`

Each output `.bam` file is accompanied by a `.bam.pbi` index file.

## Example executions

HiFi run from *BAM* with **symmetric** barcodes:

    lima <movie>.hifi_reads.bam barcodes.fasta <movie>.demux.bam --same --ccs --min-score 80

HiFi run from *FASTQ* with **asymmetric** barcodes:

    lima <movie>.hifi_reads.fq.gz barcodes.fasta <movie>.demux.fastq --different --ccs --min-score 80

CLR run from *XML* with **symmetric** barcodes:

    lima <movie>.subreadset.xml barcodes.fasta <movie>.demux.subreadset.xml --same

CLR run from *BAM* with **asymmetric** barcodes:

    lima <movie>.subreads.bam barcodes.fasta <movie>.demux.bam --different

HiFi run from *FASTA* with single-sided barcodes:

    lima <movie>.fasta barcodes.fasta <movie>.demux.fasta --ccs --min-score 80 --single-side
