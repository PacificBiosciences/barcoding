---
layout: default
parent: FAQ
title: Bio Sample names
---

## Associate bio sample names with barcode pairs
If you want to assign a bio sample name with a barcode pair, you can use the SMRT
Link feature or directly use it in *lima* version 2.2.0 or later.

For this, please create a CSV file with two columns `Barcodes,Bio Sample`. The
first column contains the barcode names of the pair; if asymmetric is used,
only give the pair a single name as the order of the barcodes in a pair is not
relevant. Example:

    Barcodes,Bio Sample
    bc1001--bc1001,Alfred
    bc1002--bc1002,Berthold
    bc1003--bc1003,Constantin
    bc1008--bc1008,Holger

Provide this CSV to lima via `--biosample-csv input.csv`.

This will associate the bio sample name to the read group using the `SM` tag.
