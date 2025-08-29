# EEG-Music TRF Analysis Pipeline

This repository implements a pipeline for analyzing EEG responses to musical stimuli using Temporal Response Functions (TRFs). The pipeline includes: preparing musical stimuli from MIDI files, preprocessing and decoding neural responses, and generating statistical visualizations from collected data. The analysis focuses on modeling brain-stimulus relationships to compare brain activity patterns between musicians and non-musicians across multiple EEG frequency bands, providing insights into how musical expertise shapes brain responses to acoustic features.

## Structure

The analysis pipeline consists of three main components:
1. **Stimulus Preparation**: Converting MIDI files to WAV format (midi_to_wav.ipynb)
2. **Neural Decoding**: Computing TRFs to model brain-stimulus relationships (TRF_decoding_pipeline.ipynb)
3. **Statistical Analysis**: Comparing decoding performance across groups (musicians and non-musicians) and frequency bands (FINAL_RESULTS.ipynb)

## Data Requirements

- **EEG Data**: Bach music listening dataset (`diliBach_4dryad_CND/`)
  - 20 subjects (10 non-musicians: Sub1-Sub10, 10 musicians: Sub11-Sub20)
  - 30 trials per subject (10 unique Bach pieces, each presented 3 times)
  - 64-channel EEG recordings

- **Audio Stimuli**: MIDI files (`diliBach_midi_4dryad/`)
  - 10 Bach pieces in MIDI format (`audio1.mid` through `audio10.mid`)
  - Converted to WAV format for acoustic feature extraction

## Dependencies

**Python 3.8+**

```bash
# Core scientific computing
pip install numpy scipy matplotlib

# EEG analysis  
pip install mne eelbrain

# Audio processing
pip install pretty_midi soundfile

# Data handling (built-in)
# os, re, pathlib, pickle
```

## Pipeline Components

### 1. MIDI to WAV Conversion (`midi_to_wav.ipynb`)

Converts symbolic MIDI files into audio waveforms suitable for acoustic feature extraction.

**Key Features:**
- High-quality audio synthesis using PrettyMIDI
- Optional FluidSynth integration for enhanced audio quality
- Automatic amplitude normalization and silence trimming
- Batch processing of all stimulus files

**Output:** 10 WAV files (`1.wav` through `10.wav`) with consistent audio properties

### 2. TRF Decoding Pipeline (`TRF_decoding_pipeline.ipynb`)

The core analysis pipeline that processes EEG data and computes brain-stimulus relationships (for one subject and one band).

**Analysis Steps:**

1. **EEG Preprocessing**
   - Load subject data from MATLAB files
   - Bandpass filtering (4 frequency bands: delta 1-4Hz, theta 4-8Hz, alpha 8-13Hz, beta 13-30Hz)
   - Channel localization and montage setup

2. **Stimulus Processing**
   - Extract acoustic envelope from WAV files
   - Compute onset strength (derivative of envelope)
   - Resample features to 100Hz for TRF analysis

3. **TRF Computation**
   - Forward models: predict EEG from acoustic features (for further research)
   - Backward models (decoders): predict acoustic features from EEG (for this research)

4. **Decoding Analysis**
   - Trial-by-trial correlation between predicted and actual acoustic features
   - Separate analysis for envelope and onset features
   - Performance metrics stored for statistical testing

**Output:** Pickle files (`sub{X}_{band}`) containing TRF models and trial-wise correlations

### 3. Results Analysis (`FINAL_RESULTS.ipynb`)

Comprehensive statistical analysis and visualization of decoding results across all subjects and conditions.

**Analysis Components:**

1. **Group Comparisons**
   - Statistical testing (t-tests) comparing musicians vs. non-musicians across frequency bands
   - Separate analysis for envelope and onset decoding

2. **Backward TRF Visualization**
   - Average TRF topographies for each group
   - Temporal response patterns highlighting peak latencies
   - Butterfly plots showing sensor-level responses

3. **Performance Examples**
   - Good, average, and bad decoding examples
   - Trial-specific visualizations comparing predicted vs. actual features

## How to Run

1. Convert all MIDI files to WAV format using `midi_to_wav.ipynb`.
2. Run TRF decoding per subject per band using `TRF_decoding_pipeline.ipynb`.
3. Aggregate and visualize final results using `FINAL_RESULTS.ipynb`.

## Key Findings

The study reveals several key findings about musical expertise and neural processing:

1. **Musical Training Effects**: Correlation coefficients were significantly higher for musicians than non-musicians, demonstrating that musical expertise enables more accurate decoding of musical features from EEG.

2. **Theta Band Superiority**: The theta band (4-8 Hz) yielded the best decoding performance overall for both groups, making it the optimal frequency range for musical feature extraction.

3. **Envelope vs Onset Processing**: Envelope decoding generally outperformed onset decoding.

4. **Right-Lateralized Neural Patterns**: Musicians showed stronger filter amplitudes and stronger voltage fields, particularly over right-lateralized channels, aligning with prior findings on hemispheric lateralization for musicians.

## References

The full research "Decoding Melodic Acoustic Features from Neural Data" can be found at:

Paper: https://ccrma.stanford.edu/~iran/papers/Bozilovic_and_Roman_AESAIMLA_2025.pdf

Poster: https://ccrma.stanford.edu/~iran/student_projects/zorka_poster.pdf

All citations for the pipeline methodology and dataset used can be found in the paper's references section.
