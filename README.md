# Joint Beamforming and Speaker-Attributed ASR for Real Distant-Microphone Meeting Transcription

This repository collects code and scripts used in the paper  
[**"Joint Beamforming and Speaker-Attributed ASR for Real Distant-Microphone Meeting Transcription."**](https://eusipco2025.org/wp-content/uploads/pdfs/0000336.pdf)

It includes the full data generation, dereverberation model training, and speaker-attributed ASR pipelines based on real AMI meeting data.

---

## 📂 Repository Overview

This repository integrates both signal processing (denoising + dereverberation) and ASR system training with speaker attribution.

---

## 🛠️ Data Synthesis for Neural Dereverberation (AMI)

To train a supervised denoising/dereverberation model on AMI data, we prepare paired clean and mixed signals as follows:

### ✂️ Clip Extraction (Ground Truth Preparation)

- [`cut_clips.py`](https://github.com/can-cui/asteroid-related/blob/main/egs/AMIx/data_synthesis/cut_clips/cut_clips.py)  
  Extracts clean single-speaker speech segments from AMI recordings. These segments serve as ground truth during model training.

### 🔀 Mixture Simulation

- [`clips_synthesis.py`](https://github.com/can-cui/asteroid-related/blob/main/egs/AMIx/data_synthesis/synthesis/clips_synthesis.py)  
  Combines clean headset recordings (from multiple speakers) to synthesize realistic far-field microphone mixtures for training.

### 🧩 Chunk Generation for ASR Input

- [`cut_clips_8array1.py`](https://github.com/can-cui/asteroid-related/blob/main/egs/AMIx/data_synthesis/cut_clips/cut_clips_8array1.py)  
  Splits AMI recordings into 4-second chunks to prepare model-ready ASR input for both training and testing.

---

## 🔁 Dereverberation / Denoising Model (FaSNet-TAC)

- [`run_reverb.sh`](https://github.com/can-cui/asteroid-related/blob/main/asteroid_pl150/egs/AMIx/TAC/run_reverb.sh)  
  Script to train FaSNet-TAC model for multichannel dereverberation and denoising on AMI.

- [`train_dm.py`](https://github.com/can-cui/asteroid-related/blob/main/asteroid_pl150/egs/AMIx/TAC/train_dm.py)  
  Core training logic for FaSNet-TAC. Also includes integration of WPE (Weighted Prediction Error) pre-processing.

---

## 🧠 Speaker-Attributed ASR Models

This project uses a Conformer-based ASR model (via [SpeechBrain](https://github.com/speechbrain/speechbrain)) with speaker attribution and joint beamforming.

### 🎯 Baseline Models

- **Single-Channel SA-ASR**:  
  [`conformer_large_spk.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/AMI/ASR/transformer/hparams/conformer_large_spk.yaml)

- **Multichannel SA-ASR**:  
  [`conformer_large_MC_spk.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/AMI/ASR/transformer/hparams/conformer_large_MC_spk.yaml)

---

## 📶 Beamformer Comparison Experiments

The following configurations correspond to the three beamformers compared in the paper:

| Method                  | Config                                                                 |
|-------------------------|------------------------------------------------------------------------|
| DAS + SA-ASR            | [`conformer_large_spk_DAS.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/AMI/ASR/transformer/hparams/conformer_large_spk_DAS.yaml)  |
| MVDR + SA-ASR           | [`conformer_large_spk_MVDR.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/AMI/ASR/transformer/hparams/conformer_large_spk_MVDR.yaml) |
| FaSNet + SA-ASR         | [`conformer_large_MC_dereverb_saasr.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/AMI/ASR/transformer/hparams/conformer_large_MC_dereverb_saasr.yaml) |
| FaSNet + SA-ASR (Joint) | [`conformer_large_MC_dereverb_saasr_retrain.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/AMI/ASR/transformer/hparams/conformer_large_MC_dereverb_saasr_retrain.yaml) |

---

## 🎧 Spectrogram Visualization

To visually compare audio quality (e.g., denoised vs. raw signals), use:

- [`Visualize_beamformed.ipynb`](https://github.com/can-cui/asteroid-related/blob/main/egs/DAS/Visualize_beamformed.ipynb)  
  This notebook plots spectrograms for qualitative comparison acr

## 📌 Citation

If you find this work helpful, please consider citing the original paper.

```bibtex
@inproceedings{cui2025joint,
  title={Joint Beamforming and Speaker-Attributed ASR for Real Distant-Microphone Meeting Transcription,
  author={Cui, Can and Sheikh, Imran and Sadeghi, Mostafa and Vincent, Emmanuel},
  booktitle={IEEE the European Signal Processing Conference (EUSIPCO)},
  year={2025},
  pages={336--340}
}
```
---
