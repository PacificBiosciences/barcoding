---
layout: default
parent: Output files
title: Report
---

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
