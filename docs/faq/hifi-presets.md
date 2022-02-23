---
layout: default
parent: FAQ
title: HiFi Presets
---

## HiFi presets
With v2.5.0 we introduced the concept of recommended parameter presets called
`--hifi-preset`. All preset use

    --ccs --min-score 80 --min-end-score 50 --min-ref-span 0.75

in addition they differ as following

|        Preset        |              Definition               |
| -------------------- | ------------------------------------- |
| `SYMMMETRIC`         | `--same`                              |
| `SYMMETRIC-ADAPTERS` | `--same --ignore-missing-adapters`    |
| `ASYMMETRIC`         | `--different --min-scoring-regions 2` |

For barcoded adapter libraries, `SYMMETRIC-ADAPTERS` will increase demultiplexed
yield. More info under [barcoded adapter FAQ](/faq/barcoded-adapter)
