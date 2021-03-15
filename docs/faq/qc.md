---
layout: default
parent: FAQ
title: QC
---

# `report_detail.R`

The first is for the `lima.report` file: `scripts/r/report_detail.R`. Example:

    $ Rscript --vanilla scripts/r/report_detail.R prefix.lima.report

The second, optional argument is the output file type `png` or `pdf`, with
`png` as default:

    $ Rscript --vanilla scripts/r/report_detail.R prefix.lima.report pdf

You can also restrict output to only barcodes of interest, using the barcode
name not the index.
For example, all barcode pairs that contain the barcode "bc1002":

    $ Rscript --vanilla scripts/r/report_detail.R prefix.lima.report png bc1002

A specific barcode pair "bc1020--bc1045"; note that, the script will look for both
combinations "bc1020--bc1045" and "bc1045--bc1020":

    $ Rscript --vanilla scripts/r/report_detail.R prefix.lima.report png bc1020--bc1045

Or any combination of those two:

    $ Rscript --vanilla scripts/r/report_detail.R prefix.lima.report pdf bc1002 bc1020--bc1045 bc1321

## Yield
Per-barcode base yield:
<img src="../img/plots/detail_yield_base.png" width="886px">

Per-barcode read yield:

<img src="../img/plots/detail_yield_read.png" width="886px">

Per-barcode ZMW yield:
<img src="../img/plots/detail_yield_zmw.png" width="886px">

## Scores
Score per number of adapters (lines) and all adapters (histogram).
[What are half adapters?](#what-are-half-adapters)

<img src="../img/plots/detail_scores_per_adapter.png" width="886px">

Read length vs. mean score (99.9% percentile)
<img src="../img/plots/detail_read_length_vs_score.png" width="886px">

HQ length vs. mean score (99.9% percentile)
<img src="../img/plots/detail_hq_length_vs_score.png" width="886px">

## Read length (99.9% percentile, 1000 binwidth)
Grouped by barcode, same y-axis :

<img src="../img/plots/detail_read_length_hist_group_same_y.png" width="886px">

Grouped by barcode, free y-axis:

<img src="../img/plots/detail_read_length_hist_group_free_y.png" width="886px">

Not grouped into facets, line histogram:

<img src="../img/plots/detail_read_length_linehist_nogroup.png" width="886px">

Barcoded vs. non-barcoded:

<img src="../img/plots/detail_read_length_hist_barcoded_or_not.png" width="886px">

## HQ length (99.9% percentile, 2000 binwidth)
Grouped by barcode, same y-axis:

<img src="../img/plots/detail_hq_length_hist_group_same_y.png" width="886px">

Grouped by barcode, free y-axis:

<img src="../img/plots/detail_hq_length_hist_group_free_y.png" width="886px">

Not grouped into facets, line histogram:

<img src="../img/plots/detail_hq_length_linehist_nogroup.png" width="886px">

Barcoded vs. non-barcoded:

<img src="../img/plots/detail_hq_length_hist_barcoded_or_not.png" width="886px">

## Adapters  (99.9% percentile, 1 binwidth)
Number of adapters:

<img src="../img/plots/detail_num_adapters.png" width="886px">

# `report_summary.R`
The second script is for high-plex data in one `lima.report` file:
`scripts/r/report_summary.R`.

Example:

    $ Rscript --vanilla scripts/r/report_summary.R prefix.lima.report

Yield per barcode:

<img src="../img/plots/summary_yield_zmw.png" width="750px">

Score distribution across all barcodes:

<img src="../img/plots/summary_score_hist.png" width="750px">

Score distribution per barcode:

<img src="../img/plots/summary_score_hist_2d.png" width="886px">

Read length distribution per barcode:

<img src="../img/plots/summary_read_length_hist_2d.png" width="886px">

HQ length distribution per barcode:

<img src="../img/plots/summary_hq_length_hist_2d.png" width="886px">

Bad adapter ratio histogram:

<img src="../img/plots/summary_bad_adapter_ratio.png" width="886px">
