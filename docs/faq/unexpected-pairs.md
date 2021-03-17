---
layout: default
parent: FAQ
title: Unexpected pairs
---

## Why are *different* barcode pair hits reported in --same mode?
*Lima* tries all barcode combinations and `--same` only filters BAM output.
Sequences flanked by *different* barcodes are still reported, but are not
written to BAM. By not enforcing only *same* barcode pairs, *lima* gains
higher PPV, as your sample might be contaminated and contains unwanted
barcode pairs; instead of enforcing one *same* pair, *lima* rather
filters such sequences. Every *symmetric* / *tailed* library contains few *asymmetric*
templates. If many *different* templates are called, your library preparation
might be bad.

## Why are *same* barcode pair hits reported in the default *different* mode?
Even if your sample is labeled *asymmetric*, *same* hits are simply
sequences flanked by the *same* barcode ID.

But my design does not include *same* barcode pairs! We are aware of this,
but it happens that some ZMWs do not have sufficient signal to call a pair
with different barcodes.

## What are undesired hybrids?
When running with `--peek-guess` or similar manual option combination and
different barcode pairs are found during peek, the full chip may contain
low-abundant different barcode pairs that were identified during peek
individually, but not as a pair. Those unwanted barcode pairs are called
hybrids in *lima*.
