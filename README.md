# AI-Based Early-Screening of Prosopagnosia Using Multimodal Electroencephalography and Diagnosis-Specific Questionnaire

A multimodal early-screening system for **prosopagnosia (face blindness)** in children, combining EEG-based deep learning analysis with a diagnosis-specific behavioral questionnaire. Built as undergraduate research at the **Department of Artificial Intelligence and Data Science, SRM Valliammai Engineering College**.

📄 **Research work is currently under publication** — paper draft included in this repo.

**Authors:** R. Lakshmi¹ (Assistant Professor), Anapalli Arjun², Aadithya Arunachalam², Varun Radhakrishnan²
¹Department of AI & DS, SRM Valliammai Engineering College · ²Student authors

GitHub collaboration: [@arjun-anapalli](https://github.com/arjun-anapalli) & [@Aadi1508](https://github.com/Aadi1508)

## Abstract

Prosopagnosia frequently goes undiagnosed in children due to subtle symptoms and a lack of effective screening tools. Existing methods rely on lengthy behavioral assessments or subjective self-reporting. This project presents an integrated early-screening framework combining EEG signal analysis with diagnosis-specific questionnaires. EEG is recorded while participants view familiar and unfamiliar faces, capturing neural responses tied to face perception. After preprocessing (bandpass filtering, ICA, epoch segmentation) to extract event-related potentials — particularly the **N170** component — a hybrid **CRNN (Convolutional Recurrent Neural Network)** analyzes spatial and temporal features in the EEG. Behavioral questionnaire data is collected in parallel, and both modalities are combined via a decision fusion module to produce a screening prediction with a confidence score, surfaced through a web-based dashboard with exportable clinical reports.

**The fused system achieves 96% classification accuracy.**

## How it works

1. **Data acquisition** — EEG recorded while participants view a randomized sequence of familiar and unfamiliar faces; questionnaire completed afterward.
2. **Signal preprocessing** — bandpass filtering (1–40 Hz), ICA-based artifact removal (eye blinks, muscle activity), epoch segmentation (-200 to 800 ms around stimulus), isolating ERPs — especially **N170** at occipito-temporal electrodes (PO7, PO8, P7, P8), plus P100 and P300.
3. **CRNN classification** — 3 conv layers (32/64/128 filters, 3×3 kernels, ReLU, max pooling, dropout 0.25) → 2 LSTM layers (128/64 units, dropout 0.3) → dense layer (64 units) → softmax output. Trained with Adam (lr 0.001), binary cross-entropy, 10 epochs, batch size 16.
4. **Questionnaire scoring** — 15-item, diagnosis-specific, 5-point Likert scale, covering recognition errors with familiar people, reliance on non-facial cues (voice, gait, clothing), social anxiety, and compensatory strategies. Normalized to [0,1].
5. **Decision fusion** — `P_final = 0.7 × P_CRNN + 0.3 × S_questionnaire`, thresholded at 0.5, with a confidence score based on distance from threshold.
6. **Web dashboard** — visualizes EEG waveforms, frequency bands, questionnaire scores, and screening outcome; exports clinical reports.

## Results

| Model | Accuracy | Precision (macro) | Recall (macro) | F1 (macro) |
|---|---|---|---|---|
| **CRNN + Questionnaire (Fused)** | **0.96** | 0.975 | 0.93 | 0.952 |
| CRNN (EEG only) | 0.94 | 0.96 | 0.90 | 0.93 |
| CNN (EEG) | 0.91 | 0.92 | 0.88 | 0.90 |
| RNN (EEG) | 0.89 | 0.90 | 0.86 | 0.88 |
| Random Forest (feature-based) | 0.85 | 0.86 | 0.84 | 0.85 |
| SVM (feature-based) | 0.82 | 0.84 | 0.80 | 0.82 |

Multimodal fusion outperforms every unimodal baseline, confirming that combining objective neural signal with subjective behavioral data improves screening reliability over either alone.

## Tech stack

- **Frontend:** React + TypeScript, Vite, Tailwind CSS — dashboard for visualization & report export
- **ML / Signal processing:** Python, PyTorch (CRNN), EEG preprocessing (bandpass filtering, ICA, epoch segmentation)
- **Tooling:** ESLint, PostCSS

## Repo contents

```
.
├── src/                              # Frontend dashboard (React/TS)
├── public/images/                    # Static assets
├── train_crnn_eeg.py                 # CRNN training script
├── crnn_eeg.pth                      # Trained model weights
├── ICIDA_2025_Presentation.pptx      # Conference presentation
├── Prosopagnosia_Paper_Draft.pdf     # Full paper draft (under publication)
└── .bolt/                            # Bolt project config
```

## Status

Research prototype, under active development. The associated paper is currently under publication — please don't redistribute the draft without checking with the authors first.

## Future work

- Larger, more diverse pediatric datasets for better generalization
- Longitudinal validation across age groups and developmental stages
- Additional ERP components / connectivity measures
- Real-time screening capability
- Clinical validation studies with neurological and educational specialists

## Citation

If you reference this work, please cite the paper (full citation to follow once published — for now, see `Prosopagnosia_Paper_Draft.pdf` in this repo).

---
*Undergraduate research at SRM Valliammai Engineering College, Dept. of AI & DS — feedback and discussion welcome.*
