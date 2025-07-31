# EEG-Music TRF Analysis Pipeline

This project includes a complete pipeline for preparing musical stimuli, decoding brain responses, and visualizing results from EEG data collected during music listening. It leverages `Eelbrain`, `MNE`, `PrettyMIDI`, and other Python libraries to process both stimulus and neural data.

## Contents

### 1. `midi_to_wav.ipynb` — **Convert MIDI to WAV**

This notebook converts symbolic MIDI files into audio waveforms (wav) for use in EEG analysis.

* **Key Function:**

  * `convert_midi_to_wav()`: Converts MIDI files to WAV, normalizes amplitude, trims silence, and saves output.
* **Dependencies:** `pretty_midi`, `soundfile`, optional `fluidsynth` with soundfont for higher-quality synthesis.
* **Output:** 10 `.wav` files (`1.wav`, `2.wav`, ..., `10.wav`) saved to `diliBach_wav_4dryad`.

---

### 2. `TRF_decoding_pipeline.ipynb` — **EEG-TRF Decoding and Preprocessing**

Processes raw EEG recordings, extracts neural responses, and computes correlations with music features (e.g., envelope, onset strength).

* **Key Steps:**

  * Loads EEG for one subject (`Sub14`) and performs bandpass filtering (1–4 Hz).
  * Aligns EEG with stimulus events.
  * Computes and saves correlation between actual and predicted stimulus features.

* **Main Functions:**

  * `load_subject_raw_eeg`
  * `create_mne_raw_from_loaded`
  * `create_eelbrain_events`

---

### 3. `FINAL_RESULTS.ipynb` — **Visualization and Summary**

Generates summary plots and statistics across all subjects and trials.

* **Purpose:**

  * Compare predicted vs. true envelopes.
  * Report best, average, and worst decoding performance.

## Data Description

* EEG Data Source: Bach music listening dataset (`diliBach_4dryad_CND`)
* Stimulus: MIDI-converted WAVs from `diliBach_wav_4dryad`
* TRF Models: Computed using `eelbrain.boosting` and cross-validated.

---

## How to Run

1. Convert all MIDI to WAV using `midi_to_wav.ipynb`
2. Run TRF decoding per subject per band in `TRF_decoding_pipeline.ipynb`
3. Aggregate and visualize final results using `FINAL_RESULTS.ipynb`
