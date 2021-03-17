---
layout: default
parent: Output files
title: Counts
---

## Counts
The fourth file is `prefix.lima.counts`, a tsv file, that shows the counts of each
observed barcode pair; only passing ZMWs are counted.
Example:

    $ column -t prefix.lima.counts
    IdxFirst  IdxCombined      IdxFirstNamed  IdxCombinedNamed  Counts
    1         1                bc1002         bc1002            113
    14        14               bc1015         bc1015            129
    18        18               bc1019         bc1019            106
