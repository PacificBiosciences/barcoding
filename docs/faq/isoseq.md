---
layout: default
parent: FAQ
title: Universal sequence
---

## What is a universal spacer sequence and how does it affect demultiplexing?
For library designs that include an identical sequence between adapter
and barcode, e.g. probe-based linear barcoded adapters samples,
_lima_ offers a special mode that is activated if it finds a shared prefix
sequence among all provided barcode sequences. Example:

    >custombc1
    ACATGACTGTGACTATCTCACACATATCAGAGTGCG
    >custombc2
    ACATGACTGTGACTATCTCAACACACAGACTGTGAG

In this case, *lima* detects the shared prefix `ACATGACTGTGACTATCTCA` and
removes it internally from all barcodes. Subsequently, it increases the
window size by the length `L` of the prefix sequence.
If `--window-size-bp N` is used, the actual window size is `L + N`.
If `--window-size-mult M` is used, the actual window size is `(L + |bc|) * M`.

Because the alignment is semi-global, a leading reference gap can be added
without any penalty to the barcode score.
