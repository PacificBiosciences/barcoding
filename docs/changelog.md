---
layout: default
title: Changelog
nav_order: 99
---

# Version changelog

 * **2.1.0**:
   * SMRT Link v10.1 release
   * Do not allow double hyphen in barcode name

 * 2.0.1:
   * SMRT Link v10.0 release
   * Minor fixes

 * 2.0.0:
   * Add support for FASTA and FASTQ
   * Fix `-k` with by-strand HiFi reads

 * 1.11.0:
   * Add barcode to read groups, use one barcode pair per RG
   * Fix double demux, used to clip wrongly for the second round of demuxing

 * 1.10.0:
   * Output N barcodes per subdirectory with `--files-per-directory N` and output splitting
   * BioSample awareness for XML input and split output and allow ignoring them with `--ignore-biosamples`
   * Increase `--window-size-mult` to `3` to allow longer spacers
   * Do not report no adapter hits as too short inserts
   * Increase `--guess` barcode score to `75` if `--peek-guess --ccs` are combined
   * Enable double demux of CCS data
   * Print run time, CPU time, and peak memory consumption with `--log-level INFO`
   * New CLI UX

 * 1.9.0:
   * Add `--bad-adapter-ratio` to remove ZMWs with molecularly missing adapters
   * Fix rare case, where a read only matches one barcode and not a single alternative
   * Fix `--no-bam` to automatically omit pbi
   * Allow combination of `--isoseq` with `--peek-guess`

 * 1.8.0:
   * Add clip lengths as `bx` tag
   * Enable single-barcode samples
   * Implicitly call `--dump-clips` in `--isoseq` mode

 * 1.7.1: Fix rare-care PBI generation bug, included in SMRT Link 6.0.0

 * 1.7.0: Fix corner-case bug

 * 1.6.1: Fix `--min-end-score` in combination with `--isoseq`

 * 1.6.0:
   * New filter `--min-end-score`
   * Add latest filters to summary file
   * New IsoSeq default parameters
   * Fix streaming of asymmetric BAM files

 * 1.5.0: Support spacer sequence between adapter and barcode

 * 1.4.0:
   * New filter `--min-ref-span` and `--min-scoring-regions`
   * Single-side library improvements

 * 1.3.0: `--peek-guess` uses only full-length ZMWs

 * 1.2.0:
   * Streaming of split BAM files
   * New fat binary build approach

 * 1.1.0: IsoSeq support

 * 1.0.0: Initial release, included in SMRT Link 5.1.0
