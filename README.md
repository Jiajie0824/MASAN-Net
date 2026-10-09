# MASAN-Net

Attention-assisted multi-scale spectral representation learning for continuous music emotion quantification.

## Overview

MASAN-Net is the framework described in the manuscript:

**Attention-Assisted Multi-Scale Music Spectral Representation Learning for Intelligent Emotion Quantification**

The framework estimates continuous Valence and Arousal values from music audio. It combines multi-scale convolutional feature extraction with frequency and temporal attention to learn emotion-related representations from Mel-spectrograms.

- Valence represents the negative-to-positive dimension of perceived musical emotion.
- Arousal represents the low-to-high activation dimension of perceived musical emotion.

This README documents the manuscript's architecture and experimental configuration. Executable reproduction requires the corresponding implementation, dataset split manifests, preprocessing settings, and model checkpoints.

## Model Architecture

The processing pipeline consists of:

1. Audio preprocessing  
   Normalize audio waveforms and construct Mel-spectrogram representations.

2. Multi-scale spectral encoding 
   Apply parallel convolutional branches with different receptive fields and concatenate their feature representations.

3. Frequency attention  
   Adaptively weight spectral regions within the extracted representations.

4. Temporal attention  
   Adaptively weight temporal regions in the frequency-refined representations.

5. Emotion regression 
   Aggregate the refined features through global pooling and use a multilayer perceptron to predict Valence and Arousal.

6. Attention visualization  
   Inspect frequency attention, temporal attention, and integrated attention maps.

Attention maps describe the model's weighting behavior. They should not be interpreted as causal evidence about human emotional perception.

## Datasets

### DEAM

The Database for Emotional Analysis of Music (DEAM) serves as the primary dataset for supervised continuous Valence–Arousal regression.

Reproduction requires documenting:

- The selected recordings and annotation files.
- The use of static or time-varying emotion annotations.
- Audio segmentation and label alignment.
- Target normalization.
- Training, validation, and test recording identifiers.

Segments from the same recording should remain in the same partition to prevent information leakage.

### MTG-Jamendo

The manuscript uses MTG-Jamendo for external evaluation.

Because the manuscript acknowledges differences between its annotations and those of DEAM, continuous regression evaluation requires an explicit description of compatible Valence–Arousal targets and their provenance.

Any conversion from categorical annotations to numerical targets must be documented and evaluated separately from direct continuous-label validation.

### Data Access

Obtain datasets from their original providers and follow the applicable access conditions and licenses. Dataset recordings are not included in this README.

## Experimental Configuration

The manuscript reports the following configuration:

| Setting | Value |
| --- | --- |
| Deep learning framework | PyTorch 2.1.0 |
| CUDA environment | 12.1 |
| GPU | NVIDIA RTX 3090, 24 GB |
| Optimizer | AdamW |
| Initial learning rate | 0.0001 |
| Learning rate scheduler | Cosine annealing |
| Batch size | 32 |
| Maximum epochs | 100 |
| Training objective | Mean squared error |
| Model selection | Early stopping using validation performance |
| Independent training runs | 5 |

Complete reproduction also requires the audio sampling rate, STFT parameters, Mel filter settings, convolutional branch specifications, attention dimensions, random seeds, and early stopping configuration.

## Evaluation

Performance is evaluated separately for Valence and Arousal using:

| Metric | Preferred direction |
| --- | --- |
| Mean squared error (MSE) | Lower |
| Root mean squared error (RMSE) | Lower |
| Mean absolute error (MAE) | Lower |
| Pearson correlation coefficient (PCC) | Higher |

The manuscript compares MASAN-Net with:

- Support vector machine regression.
- Random Forest regression.
- CNN.
- CRNN.
- CNN-LSTM.
- Transformer.

Ablation experiments examine the removal of:

- Multi-scale spectral encoding.
- Frequency attention.
- Temporal attention.

Repeated training runs assess variation across random initializations.

## Manuscript-Reported Results

The following DEAM results are transcribed from Table 1 of the manuscript. They are manuscript-reported values, not independently reproduced repository results.

| Model | Valence RMSE | Valence PCC | Arousal RMSE | Arousal PCC |
| --- | ---: | ---: | ---: | ---: |
| SVM | 0.290 | 0.362 | 0.270 | 0.401 |
| Random Forest | 0.279 | 0.395 | 0.261 | 0.432 |
| CNN | 0.253 | 0.486 | 0.235 | 0.526 |
| CRNN | 0.241 | 0.536 | 0.221 | 0.574 |
| CNN-LSTM | 0.235 | 0.558 | 0.214 | 0.601 |
| Transformer | 0.228 | 0.581 | 0.207 | 0.624 |
| MASAN-Net | 0.212 | 0.636 | 0.190 | 0.681 |

Metric comparisons require consistent target scales, dataset partitions, and aggregation procedures.

## Reproducibility Materials

A reproducible release should provide:

- Model and baseline implementations.
- A versioned dependency specification.
- Preprocessing and training configurations.
- Dataset split manifests and exclusion records.
- Random seeds and checkpoint selection rules.
- Evaluation scripts and per-recording predictions.
- Results for each independent training run.
- Attention visualization procedures.
- External evaluation target construction records.

Installation and execution commands should be added alongside the corresponding scripts.


## License

A software license has not been specified in this README. Refer to the repository's LICENSE file when available. Third-party datasets retain their respective licenses.
