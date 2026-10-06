# Human ECoG speaking consonant-vowel syllables

High-density (256-channel) electrocorticography (ECoG) recorded at the University of California, San Francisco from
patients undergoing clinical treatment for epilepsy while they read aloud consonant-vowel (CV) syllables from a list.
Source study: Bouchard KE, Mesgarani N, Johnson K, Chang EF (2013). Functional organization of human sensorimotor
cortex for speech articulation. Nature 495:327-332. doi:10.1038/nature11911.

## Source and provenance
- Source: Bouchard, Kristofer E.; Chang, Edward F (2019). Human ECoG speaking consonant-vowel syllables. figshare. Collection. https://doi.org/10.6084/m9.figshare.c.4617263.v4 (31 Figshare articles, each version 1, each one NWB 2.0 file, each declared MIT).
- The same 31 files (identical size and SHA-256) are also published on DANDI as Dandiset 000019, version 0.220126.2148,
  doi:10.48324/dandi.000019/0.220126.2148 (DANDI lists the licence as CC-BY-4.0). This BIDS release follows the Figshare
  record's MIT licence and credits both releases.
- Every `*_ieeg.nwb` file in this dataset is a byte-identical copy of the corresponding source file (SHA-256 verified).
  The original files are also kept under their upstream names in `sourcedata/figshare-c4617263-v4/` with
  `sourcedata/sourcedata_provenance.json` (size, MD5, SHA-256, URL, article DOI per file).
- Each source block (file `<subject>_B<n>.nwb`) maps to one BIDS session `ses-B<n>` of subject `sub-<subject>`
  (subjects: EC2, EC9, GP31, GP33; 31 sessions).

## Ethics
Human Research Protection Program, University of California, San Francisco: "The experimental protocol was approved by the Human Research Protection Program at the University of California, San Francisco." "Subjects gave their written informed consent before the day of surgery." (Bouchard et al. 2013, Nature 495:327-332, doi:10.1038/nature11911, Methods; PMC3606666).
The Nature paper describes three subjects; this collection contains four subject codes. The source records do not
state the approval for each subject separately; the data were released by the study authors as part of this collection.

## Recordings
- Sampling rate(s): 3051.7578 and 3051.7578125 Hz (as stored per file); channels per file: 256; channel type ECOG.
- Conversion is organisation only: no filtering, resampling, re-referencing, channel removal or rescaling.
- Scaling caveat: the NWB ElectricalSeries declares unit 'volt' with conversion attribute(s) 0.001. Under the
  NWB specification physical value = stored value x conversion. The stored float32 values have a median absolute
  value of about 0.00014-0.00019 (10 s mid-recording window, all channels), which is the typical
  magnitude of ECoG in volts; multiplying by 0.001 would give sub-microvolt values. The source does not resolve this,
  so values and attributes are left exactly as published. Users should check the scaling against their analysis.
- Reference, hardware filters and amplifier are not stated in the files: iEEGReference and SoftwareFilters are n/a.
  PowerLineFrequency is 60 Hz (United States mains).
- Electrode positions: the source electrode tables of sub-EC2, sub-GP31 contain x/y/z values,
  but the source does not document their coordinate frame or units. They are kept unchanged in electrodes.tsv columns
  `nwb_x`, `nwb_y`, `nwb_z`; BIDS x/y/z are n/a and coordsystem.json declares 'Other' with units n/a (no frame is
  asserted). For sub-EC9, sub-GP33 the source coordinates are NaN.
  All other source electrode columns, including the anatomical `location` labels (the source collection describes
  hand-marked anatomical labels), `group_name`, `filtering`, `imp` and `bad`, are kept as `nwb_*` columns.
  Channel `status` reflects the NWB `bad` flag ("electrode identified as too noisy").
- Derived content inside source files: sub-EC2_ses-B1_task-syllables_ieeg.nwb: processing/ecephys (ProcessingModule), processing/ecephys/spectrum (Spectrum).
  This is part of the published NWB file and is kept byte-identical; it is not raw signal.

## Events
`*_events.tsv` lists every NWB interval table: `trials` (syllable start/stop, `nwb_trials_condition` = syllable,
`nwb_trials_speak`), an additional zero-duration `cv_transition` event per trial (from `cv_transition_time`),
`epochs` (e.g. baseline `rest_period`) and `invalid_times` (artifact annotations; the signal is not removed).
Onsets are seconds from the first ElectricalSeries sample; original NWB times are kept in `nwb_*` columns.
Empty strings in the source (e.g. a few trials with an empty `condition`) are written as `n/a`.

## Privacy
The source collection states that microphone audio was removed and that dates were replaced by 1900-01-01. A
byte-level review of all HDF5 string datasets and attributes in every file found no names, contact details, record
numbers or real recording dates (session_start_time is 1900-01-01; `file_create_date` is the 2019 NWB file creation
time). Files embedding NWB schema/extension specifications (sub-EC2_ses-B1_task-syllables_ieeg.nwb) carry the
public NWB schema text, which names the schema/extension authors with their work contact addresses; this is not
participant information.
No demographics are included. scans.tsv acq_time is n/a because the recording dates are placeholders.

## Funding (source record)
R00-NS065120; DP2-OD00862; R01-DC012379
