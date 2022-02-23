---
layout: default
parent: FAQ
title: Speed
---

## How fast is fast?
Example: 64 barcodes / asymmetric mode / 1.9M HiFi reads on a dual 64c EPYC system:

    Processed : 1912155
    Throughput: 2393135/min
    Run Time  : 48s 306ms
    CPU Time  : 2h 14m

That's 2.4M HiFi reads processed per minute on 128 physical CPU cores, including
IO.

## Is there a way to show the progress?
Yes, please use `--log-level INFO`. If there is a `.pbi` file present, the
estimated time will be shown. Otherwise, it will show progress as number of
reads every 5 seconds.


