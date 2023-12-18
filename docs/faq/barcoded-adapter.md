---
layout: default
parent: FAQ
title: Barcoded Adapter
---

## Barcoded Adapter
The most convenient way to barcode a sample is the use of barcoded adapters, as
depicted in the [barcode design overview](barcode-design). One minor
disadvantage is that the ligation might not be as efficient as with standard
SMRTbell adapters, leaving some molecules only with one adapter. As barcoded
adapter designs are inherently symmetric, we implemented ways to recover the
demultiplexed yield from one-sided barcoded molecules with ease, using
`--single-end`.

As the first step, generate HiFi data with *ccs* v6.3.0 or later. This version
will store [additional tags per
records](https://ccs.how/faq/missing-adapters.html), indicating if the molecule
has missing adapters on either side. The second step is to use the new
`--hifi-preset SYMMETRIC-ADAPTERS` introduced with *lima* v2.5.0, [described
here](/faq/hifi-presets). That's it. All of this done automatically starting
with Revio.

## What is `--single-end`?
The option `--single-end` was introduced with Revio in SMRT Link v12.0 and
*lima* v2.7.1. It allows to only score one side of a HiFi read, if the other
flank is molecularly missing a barcoded adapter. It is controlled by
`--min-end-score` and is set by default for both SYMMETRIC HiFi presets. If you
see a barcode not matching one side of the HiFi read at all, check the [`ma` and
`ac` tags](https://ccs.how/faq/missing-adapters.html) to see if the read is
missing the read is missing the barcoded adapter on one side and the
`--single-end` heuristic got activated. In *lima* v2.9.0 we improved this option
to further reduce barcode miscalls.
