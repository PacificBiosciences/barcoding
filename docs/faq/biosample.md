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

## UUID passthrough
Since *lima* v2.5.0, the functionality has been enhanced to allow specifying
UUIDs for the resulting XML files; for this, use `--reuse-uuids` in addition to
the extended csv for `--biosample-csv`. Example:

    Barcodes,UUID,Bio Sample
    bc1001--bc1001,11111111-1111-1aaa-0111-111111111111,Alfred
    bc1002--bc1002,22222222-2222-2bbb-8222-222222222222,Berthold
    bc1003--bc1003,33333333-3333-3ccc-9222-333333333333,Constantin
    bc1008--bc1008,e04f12c9-7b2e-45fd-ab49-1bc2f75d653a,Holger

Ensure that the UUID matches the regex

    [0-9a-f]{8}-[0-9a-f]{4}-[0-5][0-9a-f]{3}-[089ab][0-9a-f]{3}-[0-9a-f]{12}
