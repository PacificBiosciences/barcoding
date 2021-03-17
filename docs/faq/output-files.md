---
layout: default
parent: FAQ
title: Output files
---

*Lima* generates multiple output files per default, all starting with the same
prefix as the output file, omitting suffixes `.bam`, `.subreadset.xml`, and
`.consensusreadset.xml`. The report infix is `lima`.
Example:

    $ lima m54007_170702_064558.subreads.bam barcode.fasta /my/path/m54007_170702_064558_demux.subreadset.xml

For all output files, the prefix will be `/my/path/m54007_170702_064558_demux.`

## BAM
The first file `prefix.bam` contains clipped records, annotated with
barcode tags, that passed filters.

## FASTA/Q
Alternatively, if output file is fasta or fastq, the header of each sequence
contains all tags, separated by a single whitespace, that would be present in
the BAM format. Example FASTQ header:

    @m54006_171006_044150/4588126/ccs bc=3,3 bl=CGCGCGTGTGTGCGTG bq=100 bt=CGCGCGTGTGTGCGTG bx=16,16 cx=12 qe=2235 ql=p\tttropqorrtnnH qs=16 qt=G^\IGR]K8S>>^\^p


## Report
The second file is `prefix.lima.report`, a tab-separated file about each ZMW, unfiltered.
This report contains any information necessary to investigate the demultiplexing
process and the underlying data.
A single row contains all reads of a single ZMW. For `--per-read`, each row
contains one subread and ZMWs might span multiple rows.

*Lima* reports different information for four basic entities
`First`, `FirstAlt`, `Combined`, `SecondCandidate`, `SecondAlt`.
 - `First` is the barcode with the highest mean score.
 - `FirstAlt` is the barcode with the next highest mean score to `First`.
 - `SecondCandidate` is the barcode that in combination with `First` has the
                     highest mean score.
 - `SecondAlt` is the barcode with the next highest mean score to `SecondCandidate`.
 - `Combined` is the combination of `First` and `SecondCandidate`,
              if the call has been deemed to be *different*,
              otherwise this entity is a copy of `First`.

Information about each entity:
 - `Idx`: Barcode index, 0-based.
   `IdxCombined` equals `IdxSecondCandidate` if *different* or `IdxFirst` if *same*.
 - `Idx*Named`: Barcode name.
 - `Score`: Mean score, omitting missing barcode regions.
 - `Scores`: List of scores for each barcode region.
 - `Clips`: List of clipping positions, leading and trailing alternating.

Additional columns:
 - `ZMW`: Movie name and the ZMW hole number.
 - `ScoreCombinedFirst`: The mean barcode score for the first barcode of `Combined`.
 - `ScoreCombinedSecond`: The mean barcode score for the second barcode of `Combined`.
 - `ReadLengths`: List of read lengths.
 - `ScoreLead`: The mean absolute element-wise score difference of the best to
                alternative pair.
 - `SignalIncrease`: Difference of `ScoreFirst` to a combination of
                     `ScoreFirst` and `ScoreSecondCandidate`.
 - `NumPasses`: Number of full passes.
 - `NumAdapters`: Number of adapters.
 - `IdxsCombined`: The sequence of barcode indices `IdxFirst` and `IdxCombined`
                   along the ZMW.
 - `PassedFilters`: `1`, if reads of this ZMW passed user-provided filters
                    and records were written to BAM, otherwise `0`.
 - `IdxLowest` & `IdxHighest`: The sorted `IdxFirst` and `IdxCombined`.
                               Sorting deactivated with `--keep-idx-order`.
 - `IdxLowestNamed` & `IdxHighestNamed`: The sorted `IdxFirstNamed` and `IdxCombinedNamed`.
                                         Sorting deactivated with `-keep-idx-order`.

Side note:
 - `ScoreCombined` is also referred to as `Mean Score`, `Barcode Score`, or
                   `Barcode Quality`, depending who you are talking to.

Missing data:
An individual score with `-1` indicates that a leading or trailing adapter is
missing. This is irrelevant for CCS reads, as they are full length and flanked
by adapters. If there is no barcode region to call, fields are filled with a
`0`, `-1`, or `-` placeholders; please see example.

Example data:

    $ head prefix.lima.report | column -t
    ZMW                           IdxFirst  IdxCombined  IdxSecondCandidate  IdxFirstAlt  ScoreFirst  ScoreCombined  ScoreSecondCandidate  ScoreFirstAlt  ScoresFirst                      ScoresCombined                   ScoresSecondCandidate               ScoresFirstAlt                      ReadLengths                   IdxsCombined                                   ClipsFirst                                  ClipsCombined                               ClipsSecondCandidate                      ClipsFirstAlt                             ScoreLead  SignalIncrease  NumPasses  NumAdapters  PassedFilters  IdxFirstNamed  IdxCombinedNamed  IdxSecondCandidateNamed  IdxFirstAltNamed  IdxLowest  IdxHighest  IdxLowestNamed  IdxHighestNamed
    m54007_170603_012109/4522478  366       366          136                 136          24          24             23                    23             -1,37,37,25,14,12,20,-1          -1,37,37,25,14,12,20,-1          -1,21,25,25,31,9,31,-1              -1,21,25,25,31,9,31,-1              1582,2392,2656,2243           -1,366,366,366,366,366,366,-1                  0,1561,19,2380,12,2654,18,2243              0,1561,19,2380,12,2654,18,2243              0,1574,14,2372,19,2651,13,2243            0,1574,14,2372,19,2651,13,2243            9          4               2          3            0              bc1367         bc1367            bc1137                   bc1137            366        366         bc1367          bc1367
    m54007_170603_012109/4588251  265       265          1                   14           67          67             17                    56             -1,65,70,-1                      -1,65,70,-1                      -1,15,20,-1                         -1,54,59,-1                         1471,1698                     -1,265,265,-1                                  0,1456,17,1698                              0,1456,17,1698                              0,1453,16,1698                            0,1453,20,1698                            11         0               0          1            1              bc1266         bc1266            bc1002                   bc1015            265        265         bc1266          bc1266
    m54007_170603_012109/4653426  213       213          325                 278          18          18             17                    18             -1,32,12,6,6,32,21,6,6,12,48,-1  -1,32,12,6,6,32,21,6,6,12,48,-1  -1,20,20,14,25,14,12,6,14,31,20,-1  -1,25,25,12,14,18,15,9,12,25,25,-1  715,2326,2267,2315,2332,1587  -1,213,213,213,213,213,213,213,213,213,213,-1  0,697,2,2325,1,2250,16,2314,1,2330,19,1587  0,697,2,2325,1,2250,16,2314,1,2330,19,1587  0,709,6,2321,4,2262,2,2314,5,2327,6,1587  0,711,4,2324,5,2264,6,2310,2,2328,4,1587  9          6               4          5            0              bc1214         bc1214            bc1326                   bc1279            213        213         bc1214          bc1214
    m54007_170603_012109/4719449  -1        -1           -1                  -1           -1          -1             -1                    -1             -1                               -1                               -1                                  -1                                  292                           -                                              0                                           0                                           0                                         0                                         -1         -1              0          0            0              -1             -1                -1                       -1                -1         -1          -1              -1


## Summary
The third file is `prefix.lima.summary`, shows how many ZMWs have been filtered,
how ZMWs many are *same/different*, and how many reads have been filtered.

    ZMWs input                (A) : 213120
    ZMWs above all thresholds (B) : 176356 (83%)
    ZMWs below any threshold  (C) : 36764 (17%)

    ZMW marginals for (C):
    Below min length              : 26 (0%)
    Below min score               : 0 (0%)
    Below min end score           : 5138 (13%)
    Below min passes              : 0 (0%)
    Below min score lead          : 11656 (32%)
    Below min ref span            : 3124 (8%)
    Without SMRTbell adapter      : 25094 (68%)
    With bad adapter              : 10349 (28%) <- Only with --bad-adapter-ratio
    Undesired hybrids             : xxx (xx%) <- Only with --peek-guess
    Undesired same pairs          : xxx (xx%) <- Only with --different
    Undesired diff pairs          : xxx (xx%) <- Only with --same
    Undesired 5p--5p pairs        : xxx (xx%) <- Only with --isoseq
    Undesired 3p--3p pairs        : xxx (xx%) <- Only with --isoseq
    Undesired single side         : xxx (xx%) <- Only with --isoseq
    Undesired no hit              : xxx (xx%) <- Only with --isoseq

    ZMWs for (B):
    With same pair                : 162244 (92%)
    With different pair           : 14112 (8%)
    Coefficient of correlation    : 32.79%

    ZMWs for (A):
    Allow diff pair               : 157264 (74%)
    Allow same pair               : 188026 (88%)
    Bad adapter yield loss        : 10112 (5%) <- Only with --bad-adapter-ratio
    Bad adapter impurity          : 10348 (5%) <- Only without --bad-adapter-ratio

    Reads for (B):
    Above length                  : 1278461 (100%)
    Below length                  : 2787 (0%)

Explanation of each block:

1) Number of ZMWs that went into lima,
   how many ZMWs have been passed into the output file,
   and how many did not qualify.

2) For those ZMWs that did not qualify,
   the marginal counts of each filter; each filter is explained in great detail
   elsewhere in this document.

3) For those ZMWs that passed, how many have been flagged as having a
   *same* or *different* barcode pair. And what is the coefficient of variation
   for the barcode ZMW yield distribution in percent.

4) For all input ZMWs, how many allow calling a *same* or *different*
   barcode pair. This a simplified version of, how many ZMWs have at least one
   full pass to allow a *different* barcode pair call and how many ZMWs have
   at least half an adapter, allowing a *same* barcode pair call.

5) For those ZMWs that qualified, list the number of reads that are above and
   below the provided `--min-length` threshold  (details see [here](#filter)).

## Counts
The fourth file is `prefix.lima.counts`, a tsv file, that shows the counts of each
observed barcode pair; only passing ZMWs are counted.
Example:

    $ column -t prefix.lima.counts
    IdxFirst  IdxCombined      IdxFirstNamed  IdxCombinedNamed  Counts
    1         1                bc1002         bc1002            113
    14        14               bc1015         bc1015            129
    18        18               bc1019         bc1019            106

## Clipped regions
Using the option `--dump-clips`, clipped barcode regions are stored in the file
`prefix.lima.clips`.
Example:

    $ head -n 6 prefix.lima.clips
    >m54007_170702_064558/4850602/6488_6512 bq:34 bc:11
    CATGTCCCCTCAGTTAAGTTACAA
    >m54007_170702_064558/4850602/6582_6605 bq:37 bc:11
    TTTTGACTAACTGATACCAATAG
    >m54007_170702_064558/4916040/4801_4816 bq:93 bc:10

## Removed records
Using the option `--dump-removed`, records that did not pass provided thresholds
or are without barcodes, are stored in the file `prefix.removed.bam`.

## DataSet
One DataSet, SubreadSet or ConsensusReadset, is generated per output BAM file.

## PBI
One PBI file is generated per output BAM file.

## Can I limit the output files per directory?
If you use output BAM splitting, it can happen that you get a lot of output files.
Using `--files-per-directory N` creates subdirectories and outputs at most `N`
barcodes per directory.
