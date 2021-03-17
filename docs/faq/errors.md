---
layout: default
parent: FAQ
title: Errors
---

## Help, I get `ERROR: Could not find matching barcodes!`
If you happen to get following error message

    ERROR: Could not find matching barcodes! Check that the set of barcodes contains the used sequences
           and the correct mode has been selected: same or different.

then your XML input contains BioSamples with different barcode names than the
provided `barcode.fasta` file. Please check that you've used the correct
barcodes. You can ignore barcodes specified in the XML with `--ignore-biosamples`.
