---
layout: default
parent: Output files
title: Fasta/q
---

## FASTA/Q
Alternatively, if output file is fasta or fastq, the header of each sequence
contains all tags, separated by a single whitespace, that would be present in
the BAM format. Example FASTQ header:

    @m54006_171006_044150/5/ccs bc=3,3 bl=CGCGCGTG bq=100 bt=CGCGCGTG bx=8,8 cx=12 qe=2235 ql=p\tttrop qs=8 qt=G^\IGR]K

**Additional tags**

|  Tag  |                                         Description                                          |
| :---: | -------------------------------------------------------------------------------------------- |
| `bc`  | Barcode pair indices, integer codes represent 0-based position in the FASTA file of barcodes |
| `bl`  | Barcode sequence clipped from leading end                                                    |
| `bq`  | Barcode score / quality, normalized between 0 and 100                                        |
| `bt`  | Barcode sequence clipped from trailing end                                                   |
| `bx`  | Pair of clipped barcode sequence lengths                                                     |
| `ql`  | Qualities of barcode bases clipped from leading end, stored as a FASTQ string                |
| `qt`  | Qualities of barcode bases clipped from trailing end, stored as a FASTQ string               |
