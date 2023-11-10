---
layout: default
title: Changelog
nav_order: 99
---

# Version changelog

 * **2.9.0**:
   * SMRT Link v13.0 release
   * Add `--overwrite-biosample-name` option for double demux
   * Add `sa`, `sm`, and `sx` tags
   * Fixes for `--single-end` 

 * 2.6.0:
   * Support SEGMENT read type
   * Use final MM/ML basemod tags
   * Bug fixes

 * 2.5.0:
   * Upcoming SMRT Link release
   * Add [`lima-undo` functionality](/faq/undo)
   * Support methylation tag clipping
   * Add progress and ETA for `--log-level INFO`
   * Rename `--preset` to [`--hifi-preset`](/faq/hifi-presets)
   * Add barcoded adapter `--hifi-preset SYMMETRIC-ADAPTERS`
   * Fixes to support stranded HiFi BAM input
   * Do not abort on empty input, but warn only

 * 2.4.0:
   * Fix fasta/q input and `--guess`
   * Output empty files for missing barcode pairs `--output-missing-pairs`
   * Output each barcode into its own sub-directory `--split-subdirs`
   * Passthrough of input ExternalResources in XML
   * Add Provenance element to XML output
   * Do not override Sample tag `SM` in read group if `--isoseq` is used
   * Enable `--reuse-uuids` with `--biosample-csv`
   * Add `--reuse-source-uuid` from input XML
   * Store BioSamples under WellSample
   * Allow `--dataset-name` to override dataset name attribute

 * 2.3.0:
   * SMRT Link v10.2 release
   * Add `--preset` for `HIFI-SYMMETRIC` and `HIFI-ASYMMETRIC`

 * 2.2.0:
   * Add `--no-clip` to assign a barcode pair, but not clip barcode sequences from reads
   * Output `.consensusreadset.xml` for CCS/HiFi input
   * Add `--biosample-csv` to assign [bio sample names](/faq/biosample) to barcode pairs
   * Better help description for `--peek-guess` in combination with `--ccs` and `--isoseq`
   * Optional [performance improvements](/faq/Speed)

 * 2.1.0:
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
