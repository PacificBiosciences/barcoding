---
layout: default
parent: FAQ
title: Barcode score
---

## Identify best matching barcode pair
Let's explore how *lima* is processing and determining barcode hits.

Given a ZMW with the insert of interest `R` and its reverse-complement `r`,
each have a leading `_l` and trailing `_t` candidate barcode region:

    ┌→ r_t + adapter + R_l + R + R_t + adapter + r_l + r + --┐
    └--------------------------------------------------------┘

For each barcode, *lima* computes the score of each barcode region.
What is a barcode region? Each flanking side of an adapter is a barcode region.
Actually, *lima* performs analysis per read, by looking at the leading and
trailing region of the read. If begin and/or end of a read is not
determined by an adapter, *lima* does not try to call barcodes and puts a `-1`
as score. Why would a read not determined by adapters?
For example, if the high-quality region finder cut a read in the middle.
This happens to almost all ZMWs, for which the first and last read is cut,
so that the score list has a leading and trailing `-1`.
So for each individual barcode, we get a list of scores and if we take all
barcodes, we have a matrix of barcode regions as columns and rows for each barcode.

Example for a really good asymmetric pair of barcodes out of 4 candidate barcodes:

|Barcode|R_l|R_t|r_l|r_t|R_l|R_t|r_l|r_t|R_l|R_t|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|bc0|-1|80|69|12|26|97|85|23|40|-1|
|bc1|-1|12|7|23|45|13|7|33|25|-1|
|bc2|-1|52|23|89|87|23|34|79|85|-1|
|bc3|-1|32|50|19|8|23|17|18|13|-1|
|bc4|-1|47|35|86|79|17|29|75|80|-1|

First step: Compute mean barcode score, omitting `-1`:

|Barcode|Mean Score|
|:---:|:---:|
|bc0|54|
|bc1|21|
|bc2|**59**|
|bc3|23|
|bc4|56|

Pick the best barcode, here `bc2`, and set it as `First` barcode.

Compute the average element-wise absolute difference between `First` and each
remaining barcode; set the alternative barcode with the lowest distance to `First`
as `FirstAlt` barcode.

Compute all pairwise max with `bc2` (**bold** exchanged scores):

|Barcode<br>Combination|R_l|R_t|r_l|r_t|R_l|R_t|r_l|r_t|R_l|R_t|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|bc2+bc0|-1|**80**|**69**|89|87|**97**|**85**|79|85|-1|
|bc2+bc1|-1|52|23|89|87|23|34|79|85|-1|
|bc2+bc3|-1|52|**50**|89|87|23|34|79|85|-1|
|bc2+bc4|-1|52|**35**|89|87|23|34|79|85|-1|

Compute mean of each combined barcode list:

|Barcode<br>Combination|Mean Score|
|:---:|:---:|
|bc2+bc0|**84**|
|bc2+bc1|59|
|bc2+bc3|62|
|bc2+bc4|61|

Pick the best mean combined: **bc2+bc0** with mean score `84`.

Set `bc0` as `SecondCandidate`.

Compute the average element-wise absolute difference between `SecondCandidate` and each
remaining barcode; set the alternative barcode with the lowest distance to `SecondCandidate`
as `SecondAlt` barcode.

Determine if the barcode pair with *different* barcodes `bc2+bc0` is better than a
barcode pair with the *same* barcode `bc2`.
Here shines our new threshold, which is the minimum difference of the mean
scores between the single and combined result:

    signal_increase = 84-59 = 25

If `signal_increase` is greater than the `--min-signal-increase` threshold, accept
the combined solution. Otherwise, stick with the best single barcode and copy
all information from `First` to `Combined`. Set the sequence of barcode indices as
`IdxsCombined` to allow a traceback which barcode contributed to which score.

Why is such threshold necessary?
Imagine the data is truly *symmetric/tailed*, has the *same* barcode on both sides.
The majority of the ZMWs should pick one barcode...but that's not what happens
in reality. Taking the example from above and only looking at `bc1-4`,
we'd pick `bc2` as the best barcode and for the combined solution,
we would pick `bc2+bc3`, as the mean score increases by `3` points.
Only because a single barcode region scored higher (`50` vs `23`),
we'd choose an asymmetric/different pair. This does not make sense at all,
thus we need a minimal combined score difference to the single best score.

Why did we pick a `FirstAlt` barcode? The average absolute pairwise distance
between scores of `First` and `FistAlt` measures the mean score lead of the best
to the next best alternative barcode.
If that score lead is larger than the provided `--min-score-lead` threshold, default `10`,
accept the chosen barcodes for this ZMW. Otherwise, the difference is too small
and the ZMW can't be labelled reliably.

Why the need for `SecondAlt`? In a pair with different barcodes, passing
`--min-signal-increase`, use `IdxsCombined` to create a combined version of
`ScoresFirstAlt` and `ScoresSecondAlt` and compute the score lead to
`ScoresCombined`.

## Smith-Waterman 101
Barcode score and clipping position are computed by a Smith-Waterman algorithm.
The dynamic-programming matrix has the barcode on the vertical and the target
sequence on the horizontal axis. The initialization of the first row and column
follows a glocal alignment; global in the reference, local in the query.
The best score is determined by chosing the maximum in the last row, which is
also the clipping position. This allows us to skip overhang from the adapter
or alien DNA like primer IDs or known as molecular identifiers.

|   |    | R | E | F | E | R | E | N | C | E |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|   |  0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| B | -1 |   |   | x |   |   |   |   |   |   |
| A | -2 |   |   |   | x |   |   |   |   |   |
| R | -3 |   |   |   |   | x |   |   |   |   |
| C | -4 |   |   |   |   | x |   |   |   |   |
| O | -5 |   |   |   |   |   | x |   |   |   |
| D | -6 |   |   |   |   |   |   | x |   |   |
| E | -7 |   |   |   |   |   |   | **x** |   |   |

For the trailing barcode region, the sequence of the reference window gets
reverse-complemented and the clipping position gets transformed back into the
correct coordinate system.


## Barcode score
The barcode score is an indicator how well the chosen barcode pair matches.
After identifying the highest barcode score, it gets normalized:

    (100 * sw_score) / (sw_match_score * barcode_length)

The range is between 0 and 100, whereas 0 is no hit and 100 perfect match.
The provided mean score is the mean of both normalized barcode scores.

## Which minimum barcode score?
For CLR data, both, no threshold and `26` were tested extensively with
downstream applications to assure that results are not convoluted by
contaminants. A much lower threshold can be used, because additional internal
filters in lima remove unreliable calls that go beyond simplistic `--min-score`
thresholding.

For HiFi data, it depends on the amount of contamination you are willing to accept.
The recommended `--min-score 80` achieves a precision >99.99%. Without `--min-score` filtering
precision is `>99.95%`. For more information, read the [FAQ about precision](/faq/precision).
