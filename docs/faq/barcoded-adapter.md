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
demultiplexed yield from one-sided barcoded molecules with ease.

As the first step, generate HiFi data with *ccs* v6.3.0 or later. This version
will store [additional tags per
records](https://ccs.how/faq/missing-adapters.html), indicating if the molecule
has missing adapters on either side. The second step is to use the new
`--hifi-preset SYMMETRIC-ADAPTERS` introduced with *lima* v2.5.0, [described
here](/faq/hifi-presets). That's it.
