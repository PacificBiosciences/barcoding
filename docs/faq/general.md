---
layout: default
parent: FAQ
title: General
---

## Which minimum barcode score?
The old *bam2bam* tool required a minimum barcode score threshold of `45` to
generate reliable output. This is not true for *lima*. Both, no threshold and
`26` were tested extensively with downstream applications to assure that
results are not convoluted by contaminants.
A much lower threshold can be used, because additional internal filters in
lima remove unreliable calls that go beyond simplistic min-score thresholding.

## How fast is fast?
Example: 200 barcodes, asymmetric mode (try each barcode forward and
reverse-complement), 300,000 CCS reads. On my 2014 iMac with 4 cores + HT:

    503.57s user 11.74s system 725% cpu 1:11.01 total

Those 1:11 minutes translate into 0.233 milliseconds per ZMW,
1.16 microseconds per barcode for both sides aligning forward and reverse-complement,
and 291 nanoseconds per alignment. This includes IO.

## Why doesn't *lima* utilize the maximum number of provided cores?
This might be a simple IO bottleneck. With a barcode.fasta containing only a few
barcodes, most of the time is spent reading and writing BAM files, as the barcode
identification is too fast.

## Is there a way to show the progress?
No. Please run `wc -l prefix.report` to get the number of already processed ZMWs.

## Can I have upper- and lower-case bases in my barcodes?
You can, but lima is case-insensitive and will convert them to upper case before
the alignment step.

## Can I split my data by barcode?
You can either iterate over the `prefix.bam` file N times or use
`--split-bam`. Each barcode has its own BAM file called
`prefix.idxBest--idxCombined.bam`, e.g., `prefix.0--0.bam`.

The optional parameter `--split-bam-named`, names the files by their barcode names instead
of their barcode indices. Non-word characters, anything except [A-Za-z0-9_],
in barcode names are replaced with an underscore in the file name.

This mode might consume more memory. Read the next FAQ entry for more information.

In addition, a `prefix.datastore.json` is generated to wrap the individual dataset
files.

## Why is the memory consumption really high?
Most likely this is due to `--split-bam` or `--split-bam-named` mode.
The latter is activated per default in SMRT Link.
*Lima* is able to stream up to 500 barcode pairs to individual split BAM files.
If more than 500 barcode pairs are detected, additional output is buffered first.
In this case, memory usage (RES column in top) is approximately the size of the
input, uncompressed.

The maximum concurrent output BAM file handles can be adjusted with
`--bam-handles N`. The default is 500.

Examples, how memory usage is affected by `--bam-handles`. Option
`--bam-handles-verbose` is only used to visualize the BAM output file handles.
Memory usage reported using [memusg](https://gist.github.com/netj/526585):

    $ lima input.bam barcodes.fasta out.bam --same --split-bam --bam-handles 9 --bam-handles-verbose
    Open stream 7--7
    Open stream 3--3
    Open stream 5--5
    Open stream 1--1
    Open stream 4--4
    Open stream 6--6
    Open stream 0--0
    Open stream 2--2
    Open stream 210--210
    memusg: peak=86,728

    $ lima input.bam barcodes.fasta out.bam --same --split-bam --bam-handles 4 --bam-handles-verbose
    Open stream 7--7
    Open stream 3--3
    Open stream 5--5
    Open stream 1--1
    Buffered stream 0--0
    Buffered stream 2--2
    Buffered stream 4--4
    Buffered stream 6--6
    Buffered stream 210--210
    memusg: peak=113,476

    $ lima input.bam barcodes.fasta out.bam --same --split-bam --bam-handles 0 --bam-handles-verbose
    Buffered stream 0--0
    Buffered stream 1--1
    Buffered stream 2--2
    Buffered stream 3--3
    Buffered stream 4--4
    Buffered stream 5--5
    Buffered stream 6--6
    Buffered stream 7--7
    Buffered stream 210--210
    memusg: peak=132,276

## What are half adapters?
If there is an adapter call with only one barcode region,
as the high-quality region finder cut right through the adapter,
or the preceding or succeeding subread was too short and got removed,
or the sequencing reaction started/stopped there, we call such an adapter half.
Thus, there are also 1.5, 2.5, N+0.5 adapter calls.

ZMWs with half or only one adapter can be used to identify *same* barcode pairs;
positive-predictive value might be reduced compared to high adapter calls.
For asymmetric designs with *different* barcodes in a pair, at least a single
full-pass read is required; this can be two adapters, two half-adapters, or a
combination.

## What are bad adapters?
In the subreads.bam file, each subread has a context flag `cx`.
It annotates, among other things, if a subread has flanking adapters,
before and/or after. Adapter finding has been improved and can also find
molecularly missing adapters or those obscured by a local decrease in accuracy.
This may lead to missing or obscured bases in the flanking barcode.
Such adapters are called "bad", since they don't align with the adapter reference
sequence(s).
Regions flanking those bad adapters are problematic, because they can fully or
partially miss the barcode bases, leading to wrong classification of the
molecule.
*Lima* can handle those adapters, by ignoring regions flanking
bad adapters. For this, *lima* computes the ratio of
number of bad adapters divided by number of all adapters.

By default, `--bad-adapter-ratio` is set to `0` and does not perform any filtering.
In this mode, bad adapters are handled just like good adapters.
But the `*.lima.summary` file contains one row with the number of
ZMWs that have at least 25% bad adapters, but otherwise pass all other filters.
This metric can be used as a diagnostic to assess the library prep.

If `--bad-adapter-ratio` is set non-zero positive `(0,1]`,
bad adapter flanking barcode regions are treated as missing.
If a ZMW has a higher ratio of bad adapters than provided, the ZMW
is being filtered and consequently removed from the output.
The `*.lima.summary` file contains two additional rows.

    With bad adapter              : 10349 (28%)
    Bad adapter yield loss        : 10112 (5%)

The first row counts the number of ZMWs that have too high bad adapter ratios
and the percentage is with respected to the number of all ZMW not passing.
The second row counts the number of ZMWs that only get removed because of
too high bad adapter ratios and the percentage is with respect the number of all
input ZMWs and consequently is the effective yield loss caused by bad adapters.

If a ZMW has ~50% bad adapters, one side of the molecule is molecularly missing
an adapter. For 100% bad adapter, both sides are missing adapters.
A lower than ~40% percentage indicates decreased local accuracy during
sequencing leading to adapter sequences not being found. If a high percentage
of ZMWs is molecularly missing adapters, you should improve library prep.


## Why are *different* barcode pair hits reported in --same mode?
*Lima* tries all barcode combinations and `--same` only filters BAM output.
Sequences flanked by *different* barcodes are still reported, but are not
written to BAM. By not enforcing only *same* barcode pairs, *lima* gains
higher PPV, as your sample might be contaminated and contains unwanted
barcode pairs; instead of enforcing one *same* pair, *lima* rather
filters such sequences. Every *symmetric* / *tailed* library contains few *asymmetric*
templates. If many *different* templates are called, your library preparation
might be bad.

## Why are *same* barcode pair hits reported in the default *different* mode?
Even if your sample is labeled *asymmetric*, *same* hits are simply
sequences flanked by the *same* barcode ID.

But my design does not include *same* barcode pairs! We are aware of this,
but it happens that some ZMWs do not have sufficient signal to call a pair
with different barcodes.

## How do barcode indices correspond to the input sequences?
Input barcode sequences are tagged with an incrementing counter. The first
sequence is barcode `0` and the last barcode `numBarcodes - 1`.

## I used the tailed library prep, what options to choose?
Use `--same`.

## How can I demultiplex data with one adapter only being barcoded?
Use `--single-side`.

## What are undesired hybrids?
When running with `--peek-guess` or similar manual option combination and
different barcode pairs are found during peek, the full chip may contain
low-abundant different barcode pairs that were identified during peek
individually, but not as a pair. Those unwanted barcode pairs are called
hybrids in *lima*.

## How can I demultiplex IsoSeq data?
Even if you only want to remove IsoSeq primers, *lima* is the tool of choice.

1) Remove all duplicate sequences.
2) Annotate sequence names with a `5p` or `3p` suffix. Example:

```
    >primer_5p
    AAGCAGTGGTATCAACGCAGAGTACATGGGG
    >sample_brain_3p
    AAGCAGTGGTATCAACGCAGAGTACCACATATCAGAGTGCG
    >sample_liver_3p
    AAGCAGTGGTATCAACGCAGAGTACACACACAGACTGTGAG
```

3) Use the `--isoseq` mode. Run in combination with `--peek-guess` to remove spurious false positive.
4) Output will be only different pairs with a `5p` and `3p` combination:

```
    demux.primer_5p--sample_brain_3p.bam
    demux.primer_5p--sample_liver_3p.bam
```

Those options are very conservative to remove any spurious and ambiguous
calls, in order to guarantee that only proper asymmetric (barcoded) primer
are used in downstream analyses. Good libraries reach >75% CCS reads passing
*lima* filters.

## What is a universal spacer sequence and how does it affect demultiplexing?
For library designs that include an identical sequence between adapter
and barcode, e.g. probe-based linear barcoded adapters samples,
*lima* offers a special mode that is activated if it finds a shared prefix
sequence among all provided barcode sequences. Example:

```
    >custombc1
    ACATGACTGTGACTATCTCACACATATCAGAGTGCG
    >custombc2
    ACATGACTGTGACTATCTCAACACACAGACTGTGAG
```

In this case, *lima* detects the shared prefix `ACATGACTGTGACTATCTCA` and
removes it internally from all barcodes. Subsequently, it increases the
window size by the length `L` of the prefix sequence.
If `--window-size-bp N` is used, the actual window size is `L + N`.
If `--window-size-mult M` is used, the actual window size is `(L + |bc|) * M`.

Because the alignment is semi-global, a leading reference gap can be added
without any penalty to the barcode score.

## Why do most of my ZMWs get filtered by the score lead threshold?
The score lead measures how close the best barcode call is to the second best.
Possible solutions without seeing your data:
 * Is that sample actually barcoded?
 * Are your barcode sequences genetically too close for SMRT sequencing?
   Try CCS calling first and demultiplex with `--ccs`.
 * Are the synthesized products clean and not degenerate?
 * Did the sequencing run perform optimally, is the accuracy in the expected range?
 * Did you run lima twice, first on the original and then on the already
   demultiplexed data? This is not supported, as the barcodes have been clipped
   and removed.

Try to decrease `--score-lead`, with the potential risk of introducing
false positives.

## What is different in *lima* to *bam2bam*?
 * CCS read support
 * Barcodes of every adapter gets scored for CLR subreads
 * Does not enforce symmetric barcode pairing, which increases PPV
 * For asymmetric barcodes, `lima` can report the identified order, instead of
   ascending sorting
 * Calls barcodes per barcode region and does not enforce adapter coupling
 * Nice reports for QC

## Can I remove PCR primers after demultiplexing?
Yes! After demultiplexing, just lima on the output again with your PCR primer(s).

## Can I limit the output files per directory?
If you use output BAM splitting, it can happen that you get a lot of output files.
Using `--files-per-directory N` creates subdirectories and outputs at most `N`
barcodes per directory.

## `--peek-guess` does not work with XML input!
If your input XML file contains `<BioSamples>`, lima will deactivate barcode
inference via `--peek-guess` and only output barcodes specified in this section.
The assumption is that you know exactly which barcodes have been used and need no
inference. If this assumption is wrong, like the barcodes in the XML are wrong,
you can either just use BAM as input or use `--ignore-biosamples`.

## Help, I get `ERROR: Could not find matching barcodes!`
If you happen to get following error message

    ERROR: Could not find matching barcodes! Check that the set of barcodes contains the used sequences and the correct mode has been selected: same or different.

then your XML input contains BioSamples with different barcode names than the
provided `barcode.fasta` file. Please check that you've used the correct
barcodes. You can ignore barcodes specified in the XML with `--ignore-biosamples`.

## CCS or demux first?
Many people have been wondering, what is the recommended order for a multiplexed
HiFi pool:
  1) first ccs and then demux
  2) first demux and then ccs

### Experiment
Use 2k ecoli amplicons with barcoded overhang adapters, symmetric. Workflow steps:
  1) Generate CCS
  2) Demux subreads and whitelist on CCS hole numbers
  3) Demux CCS
  4) Compare both sets of hole numbers

### Results
Verbatim results for one chip:

    Generated CCS reads : 274185
    Demuxed CCS reads   : 269919 (98.44%)
    Demuxed subreads    : 271068 (98.86%)

Venn diagrams for two chips:

<img src="img/venn_diagramm_1.png" width="400px">
<img src="img/venn_diagramm_2.png" width="400px">

Just based on those numbers, one would say, pick subread demuxing.
Here comes the but. Demuxing subreads is very IO heavy and takes ~100x longer
than demuxing CCS.
For the sake of time to result and disk space,
**perform CCS first and demux afterwards**.

### Discussion
Q: Is there any systematic reason for reads that get correctly called by subread demux but not ccs or vice versa?

Let's plot subread barcode scores, grouped by if they were only called in subreads (blue) or not (red)
<img src="img/subread_only_scores.png" width="600px">

Majority of what is subread output only is on the verge of being called at all.
The problem with the current CCS draft stage is that it sometimes trims a few
bases, which is generally not a big issue for demuxing, but if the barcode is
molecularly damaged, too short or of low quality, a few missing bases lead to
being uncallable.

Vice versa, only called by CCS and not in subreads:
<img src="img/ccs_only_scores.png" width="600px">

Again something that is on the verge being called. The reason for the ~300 reads
at 100 score, no idea so far. In general, this is 0.1% of the data.
Let's investigate those ~300 calls and plot their subread demux barcode scores.
<img src="img/ccs_only_subread_scores.png" width="600px">

It's curious why they didn't get called, but for 0.1% not worth changing
any parameters now, but worth future investigation.
