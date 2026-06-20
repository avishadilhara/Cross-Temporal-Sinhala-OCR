# Cross-Temporal Sinhala OCR: Page-Level Adaptation and Diachronic Analysis

This repository contains the experimental results for the fine-tuning of Vision-Language Models (VLMs) on the `sinhala-ocr-lk-acts-1010` dataset. The dataset consists of 1,010 real-world page-level images from Sri Lankan Legislative Acts (spanning 1981–2019) and their transcriptions.

## Hyperparameter Tuning & Research Results

The table below summarizes the hyperparameter configurations and the corresponding performance metrics for all 8 fine-tuning experiments conducted with QLoRA adaptation. 

The experiments vary by Model, GPU hardware, Quantization level, LoRA rank ($r$), LoRA alpha ($\alpha$), and Dropout. All models were evaluated on the same 202-sample held-out test set.

| Exp | Model | GPU | Quantization | LoRA $r$ | LoRA $\alpha$ | Dropout | CER (↓) | WER (↓) | METEOR (↑) | BLEU (↑) | ANLS (↑) |
|:---:|:---|:---|:---|:---:|:---:|:---:|---:|---:|---:|---:|---:|
| **1** | DeepSeek-OCR V1 | P100 (16 GB) | 4-bit NF4 | 16 | 16 | 0 | 3.02% | 12.27% | 0.8441 | 0.9531 | 0.9698 |
| **2** | DeepSeek-OCR V2 | A100 (80 GB) | 16-bit | 32 | 64 | 0.1 | 34.71% | 47.45% | 0.5191 | 0.7288 | 0.6529 |
| **3** | DeepSeek-OCR V2 | A100 (80 GB) | 16-bit | 16 | 16 | 0 | 6.94% | 17.23% | 0.7739 | 0.9153 | 0.9306 |
| **4** | DeepSeek-OCR V2 | RTX 5090 | 4-bit NF4 | 16 | 16 | 0 | 6.94% | 18.28% | 0.7794 | 0.9181 | 0.9306 |
| **5** | LightOnOCR-2-1B | P100 (16 GB) | 4-bit NF4 | 16 | 16 | 0.05 | 25.17% | 38.31% | 0.5342 | 0.7329 | 0.7483 |
| **6** | LightOnOCR-2-1B | P100 (16 GB) | 4-bit NF4 | 32 | 64 | 0.1 | 14.13% | 21.18% | 0.6541 | 0.8332 | 0.8587 |
| **7** | **LightOnOCR-2-1B** (1540px)* | RTX 4090 | 4-bit NF4 | 32 | 64 | 0.1 | **1.05%** | **5.63%** | **0.9492** | **0.9808** | **0.9895** |
| **8** | DeepSeek-OCR V2 | RTX 3090 | 4-bit NF4 | 16 | 16 | 0 | 8.31% | 19.75% | 0.7602 | 0.9046 | 0.9169 |

*\*Experiment 7 is the top performer, achieving state-of-the-art results for real-world historical Sinhala OCR by utilizing a higher input resolution of 1540px.*

### Key Findings
- **Resolution matters:** Scaling the input longest edge to 1540px (Exp 7) significantly improved the model's ability to identify finer Sinhala stroke diacritics, pushing CER down to 1.05%.
- **Regularization on small datasets:** Aggressive regularization (higher rank $r=32$ and dropout $0.1$) was counterproductive for DeepSeek-OCR V2 on this relatively small 1,010-sample dataset, as seen in the poorer performance of Exp 2 compared to Exp 3 & 4.