---
layout: default
parent: Output files
title: Summary
---

## Summary
The third file is `prefix.lima.summary`, shows how many ZMWs have been filtered,
how ZMWs many are *same/different*, and how many reads have been filtered.

    ZMWs input                (A) : 213120
    ZMWs above all thresholds (B) : 176356 (83%)
    ZMWs below any threshold  (C) : 36764 (17%)

    ZMW marginals for (C):
    Below min length              : 26 (0%)
    Below min score               : 0 (0%)
    Below min end score           : 5138 (13%)
    Below min passes              : 0 (0%)
    Below min score lead          : 11656 (32%)
    Below min ref span            : 3124 (8%)
    Without SMRTbell adapter      : 25094 (68%)
    With bad adapter              : 10349 (28%) <- Only with --bad-adapter-ratio
    Undesired hybrids             : xxx (xx%) <- Only with --peek-guess
    Undesired same pairs          : xxx (xx%) <- Only with --different
    Undesired diff pairs          : xxx (xx%) <- Only with --same
    Undesired 5p--5p pairs        : xxx (xx%) <- Only with --isoseq
    Undesired 3p--3p pairs        : xxx (xx%) <- Only with --isoseq
    Undesired single side         : xxx (xx%) <- Only with --isoseq
    Undesired no hit              : xxx (xx%) <- Only with --isoseq

    ZMWs for (B):
    With same pair                : 162244 (92%)
    With different pair           : 14112 (8%)
    Coefficient of correlation    : 32.79%

    ZMWs for (A):
    Allow diff pair               : 157264 (74%)
    Allow same pair               : 188026 (88%)
    Bad adapter yield loss        : 10112 (5%) <- Only with --bad-adapter-ratio
    Bad adapter impurity          : 10348 (5%) <- Only without --bad-adapter-ratio

    Reads for (B):
    Above length                  : 1278461 (100%)
    Below length                  : 2787 (0%)

Explanation of each block:

  1. Number of ZMWs that went into lima, how many ZMWs have been passed into the
     output file, and how many did not qualify.
  2. For those ZMWs that did not qualify, the marginal counts of each filter;
     each filter is explained in great detail elsewhere in this document.
  3. For those ZMWs that passed, how many have been flagged as having a *same*
     or *different* barcode pair. And what is the coefficient of variation for
     the barcode ZMW yield distribution in percent.
  4. For all input ZMWs, how many allow calling a *same* or *different* barcode
     pair. This a simplified version of, how many ZMWs have at least one full
     pass to allow a *different* barcode pair call and how many ZMWs have at
     least half an adapter, allowing a *same* barcode pair call.
  5. For those ZMWs that qualified, list the number of reads that are above and
     below the provided `--min-length` threshold  (details see
     [here](#filter)).
