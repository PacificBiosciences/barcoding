---
layout: default
parent: FAQ
title: HiFi Presets
---

## HiFi presets
With v2.5.0 we introduced the concept of recommended parameter presets called
`--hifi-preset`. 

|        Preset        |              Definition               |
| -------------------- | ------------------------------------- |
| `SYMMETRIC`          | `--ccs --min-score 0 --min-end-score 80 --min-ref-span 0.75 --same --single-end` |
| `SYMMETRIC-ADAPTERS` | `--ccs --min-score 0 --min-end-score 80 --min-ref-span 0.75 --same --ignore-missing-adapters --single-end` |
| `ASYMMETRIC`         | `--ccs --min-score 80 --min-end-score 50 --min-ref-span 0.75 --different --min-scoring-regions 2` |

For barcoded adapter libraries, `SYMMETRIC-ADAPTERS` will increase demultiplexed
yield. More info under [barcoded adapter FAQ](/faq/barcoded-adapter)
