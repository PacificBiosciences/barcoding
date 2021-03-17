---
layout: default
parent: FAQ
title: Infer
---

# Advanced parameters to infer the used barcodes
## `--peek`
The option `--peek N` allows to look at the first `N` ZMWs of the input and
return the mean barcode score. This allows to test multiple test `barcode.fasta`
files and see which set of barcodes has been used.

## `--guess`
The option `--guess N` performs demultiplexing twice. In the first iteration,
all barcodes are tested per ZMW. Afterwards, the barcode pair occurrences are counted
and their mean barcode score is tested against the provided threshold `N`;
only those barcode pairs that pass this threshold are used in the second round.
In this second round of demultiplexing, only barcodes from the selected
barcode pairs are being tested for each ZMW. Finally, only ZMWs from barcode
pairs that were selected in the first round, are included in the BAM output.
Both `--same` and `--different` are being respected and can be used as
additional filters.

A `prefix.lima.guess` file shows the decision process. Example:

    $ column -t *guess
    IdxFirst  IdxCombined  IdxFirstNamed  IdxCombinedNamed  NumZMWs  MeanScore  Picked
    0         0            bc1002         bc1002            174      76         1
    0         4            bc1002         bc1048            1        43         0
    9         9            bc1080         bc1080            3        16         0
    10        10           bc1093         bc1093            742      75         1
    10        14           bc1093         bc1115            2        55         1
    12        12           bc1101         bc1101            4        18         0

## `--guess-min-count`
The minimum ZMW abundance to whitelist a barcode. This filter is `AND`ed with
the minimum barcode score provided by `--guess`. The default is 0.
If there are in total less barcoded ZMWs than the provided threshold,
the guess feature is automatically deactivated.

## `--peek` && `--guess`
The optimal way is to use both advanced options in combination, e.g.,
`--peek 1000 --guess 45`. *Lima* will run twice on the input data.
For the first 1000 ZMWs, *lima* will guess the barcodes and store the mask of
identified barcodes.
In the second run, the barcode mask is used to demultiplex all ZMWs.

## `--peek-guess`
Equivalent to the `Infer Barcodes Used parameter` option in SMRT Link.
Sets the following options:
`--peek 50000 --guess 45 --guess-min-count 10`.

If used in combination with `--isoseq`:
`--peek 50000 --guess 75 --guess-min-count 100`.

If used in combination with `--ccs`:
`--peek 50000 --guess 75 --guess-min-count 10`.

## `--peek-guess` does not work with XML input!
If your input XML file contains `<BioSamples>`, lima will deactivate barcode
inference via `--peek-guess` and only output barcodes specified in this section.
The assumption is that you know exactly which barcodes have been used and need no
inference. If this assumption is wrong, like the barcodes in the XML are wrong,
you can either just use BAM as input or use `--ignore-biosamples`.
