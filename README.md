# Stutter Detection Using Prosody

This project implements a robust speech feature extraction pipeline for automated stutter detection, focusing on prosodic and acoustic features. It utilizes the **SEP-28k** dataset and extracts fine-grained frame-level and clip-level features to distinguish between fluent and stuttered speech.

## Quick Start: Data Acquisition

To set up the project environment, run the following commands to download the dataset and pre-extracted features.

### 1. Download SEP-28k Dataset (Kaggle)
Requires [Kaggle CLI](https://github.com/Kaggle/kaggle-api) configured with your API key.

```bash
mkdir -p dataset
kaggle datasets download -d vudominhgiang/sep-28k-maintained -p dataset/
unzip dataset/sep-28k-maintained.zip -d dataset/
rm dataset/sep-28k-maintained.zip
```

### 2. Download Extracted Features (Hugging Face)
Requires [huggingface-cli](https://huggingface.co/docs/huggingface_hub/guides/cli).

```bash
mkdir -p output
huggingface-cli download bropal/stutter_detection_prosody --local-dir output/ --repo-type space
```

> Manual downloads are available at:
> - [Kaggle: SEP-28k Maintained](https://www.kaggle.com/datasets/vudominhgiang/sep-28k-maintained)
> - [Hugging Face: Stutter Detection Prosody (Output)](https://huggingface.co/bropal/stutter_detection_prosody/tree/main/output)

---

## Features & Methodology

The pipeline extracts several layers of speech features based on prosodic dynamics and spectral characteristics:

### 1. Vowel Onset Point (VOP) Detection
Faithful implementation of **Mary & Yegnanarayana (2008)**, using:
- LP Residual & Hilbert Envelope.
- Gabor filter convolution for evidence enhancement.
- Peak picking with dynamic thresholds and F0-based spurious reduction.

### 2. Syllable Prosody (7 Parameters)
Extracts prosodic dynamics between VOPs:
- **Duration**: Syllable duration and voiced duration.
- **Intonation**: Peak F0, Distance of peak from VOP ($D_p$), and F0 range ($\Delta F_0$).
- **Tilt**: Amplitude tilt and Duration tilt parameters.
- **Stress**: Delta Log Energy.

### 3. Acoustic & Spectral Features
- **MFCCs**: 13 coefficients + $\Delta$ + $\Delta\Delta$ (39 dims).
- **Voice Quality**: Jitter, Shimmer, CPP (Cepstral Peak Prominence).
- **Prosody Contours**: F0 (RAPT-inspired autocorrelation), RMS Energy, Zero-Crossing Rate.
- **Pause Features**: Silence duration, pause count, and max pause length (targeting 'Block' stutters).

---



This project is licensed under the GPL-3.0 License.
