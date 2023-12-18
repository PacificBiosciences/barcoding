---
layout: default
parent: FAQ
title: Adapter types
---

## What are half adapters?
If there is an adapter call with only one barcode region,
as the high-quality region finder cut right through the adapter,
or the preceding or succeeding subread was too short and got removed,
or the sequencing reaction started/stopped there, we call such an adapter half.
Thus, there are also 1.5, 2.5, N+0.5 adapter calls.

ZMWs with half or only one adapter can be used to identify *same* barcode pairs;
positive-predictive value might be reduced compared to high adapter calls.
For asymmetric designs with *different* barcodes in a pair, at least a single
full-pass read is required; this can be two adapters, two half-adapters, or a
combination.

## What are bad adapters?
In the subreads.bam file, each subread has a context flag `cx`.
It annotates, among other things, if a subread has flanking adapters,
before and/or after. Adapter finding has been improved and can also find
molecularly missing adapters or those obscured by a local decrease in accuracy.
This may lead to missing or obscured bases in the flanking barcode.
Such adapters are called "bad", since they don't align with the adapter reference
sequence(s).
Regions flanking those bad adapters are problematic, because they can fully or
partially miss the barcode bases, leading to wrong classification of the
molecule.
*Lima* can handle those adapters, by ignoring regions flanking
bad adapters. For this, *lima* computes the ratio of
number of bad adapters divided by number of all adapters.

By default, `--bad-adapter-ratio` is set to `0` and does not perform any filtering.
In this mode, bad adapters are handled just like good adapters.
But the `*.lima.summary` file contains one row with the number of
ZMWs that have at least 25% bad adapters, but otherwise pass all other filters.
This metric can be used as a diagnostic to assess the library prep.

If `--bad-adapter-ratio` is set non-zero positive `(0,1]`,
bad adapter flanking barcode regions are treated as missing.
If a ZMW has a higher ratio of bad adapters than provided, the ZMW
is being filtered and consequently removed from the output.
The `*.lima.summary` file contains two additional rows.

    With bad adapter              : 10349 (28%)
    Bad adapter yield loss        : 10112 (5%)

The first row counts the number of ZMWs that have too high bad adapter ratios
and the percentage is with respected to the number of all ZMW not passing.
The second row counts the number of ZMWs that only get removed because of
too high bad adapter ratios and the percentage is with respect the number of all
input ZMWs and consequently is the effective yield loss caused by bad adapters.

If a ZMW has ~50% bad adapters, one side of the molecule is molecularly missing
an adapter. For 100% bad adapter, both sides are missing adapters.
A lower than ~40% percentage indicates decreased local accuracy during
sequencing leading to adapter sequences not being found. If a high percentage
of ZMWs is molecularly missing adapters, you should improve library prep.

## How can I demultiplex data with one adapter only being barcoded in CLR data?
Use `--single-side`.
