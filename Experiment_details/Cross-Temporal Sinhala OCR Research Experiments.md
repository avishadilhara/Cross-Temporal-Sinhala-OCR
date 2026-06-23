# Cross-Temporal Sinhala OCR: Page-Level Adaptation and Diachronic Analysis

This repository contains the experimental results for the fine-tuning of Vision-Language Models (VLMs) on the `sinhala-ocr-lk-acts-1010` dataset. The dataset consists of 1,010 real-world page-level images from Sri Lankan Legislative Acts (spanning 1981–2019) and their transcriptions.

---

## Experiment Parameter Details

The table below shows the full hyperparameter configurations for all 8 fine-tuning experiments (Experiments 1–8) conducted on the `sinhala-ocr-lk-acts-1010` dataset (1,010 samples, split 70/10/20).


---

## GPU and Model size

| Exp | Base Model | GPU |  Model Size |
|:---:|:---|:---:|:---:|
| 1 | DeepSeek-OCR V1 | P100 | 281 MB |
| 2 | DeepSeek-OCR V2 | A100 | 1.6 GB |
| 3 | DeepSeek-OCR V2 | A100 | 289 MB |
| 4 | DeepSeek-OCR V2 | RTX 5090 | 298 MB |
| 5 | LightOnOCR-2-1B | P100 | 77 MB |
| 6 | LightOnOCR-2-1B | P100 | 140 MB |
| 7 | LightOnOCR-2-1B | RTX 4090 | 140 MB |
| 8 | DeepSeek-OCR V2 | RTX 3090 | 296 MB |

---

### LoRA Adapter Parameters

| Exp | LoRA Rank (`lora_r`) | LoRA Alpha (`lora_alpha`) | LoRA Dropout | Quantization (`load_in_4bit`) |
|:---:|:---:|:---:|:---:|:---|
| 1 | 16 | 16 | 0 | TRUE (4-bit NF4) |
| 2 | 32 | 64 | 0.1 |  FALSE (16-bit) |
| 3 | 16 | 16 | 0 |  FALSE (16-bit) |
| 4 | 16 | 16 | 0 |  TRUE (4-bit NF4) |
| 5 | 16 | 16 | 0.05 |  TRUE (4-bit NF4) |
| 6 | 32 | 64 | 0.1 |  TRUE (4-bit NF4) |
| 7 | 32 | 64 | 0.1 |  TRUE (4-bit NF4) |
| 8 | 16 | 16 | 0 |  TRUE (4-bit NF4) |

---

### Training Parameters

| Exp | Base Model | Epochs | Learning Rate | Train BS | Eval BS | Grad. Accum. | Grad. Checkpointing | LR Scheduler | Warmup Steps | Warmup ratio | Optimizer | Weight Decay | Eval Strategy | Eval Steps | Dataloader Workers |
|:---:|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---|:---:|:---|:---:|:---:|
| 1 | DeepSeek-OCR V1 | 3 | 2e-4 | 2 | 2 | 4 | TRUE | linear | 10 | - | adamw_8bit | 0.001 | steps | 25 | 2 |
| 2 | DeepSeek-OCR V2 | 50 | 2e-4 | 6 | 8 | 3 | TRUE | cosine_with_restarts | - | 0.03 | adamw_torch_fused | 0.1 | epoch | — | 8 |
| 3 | DeepSeek-OCR V2 | 50 | 2e-4 | 8 | 8 | 2 | TRUE | linear | 10 | - | adamw_8bit | 0.001 | epoch | — | 2 |
| 4 | DeepSeek-OCR V2 | 50 | 2e-4 | 2 | 2 | 4 | TRUE | linear | 10 | - | adamw_8bit | 0.001 | epoch | — | 2 |
| 5 | LightOnOCR-2-1B | 20 | 2.5e-4 | 2 | 2 | 8 | TRUE | cosine | 20 | - | adamw_torch_fused | 0.01 | epoch | — | 2 |
| 6 | LightOnOCR-2-1B | 30 | 5e-5 | 1 | 1 | 16 | TRUE | cosine | 50 | - | adamw_8bit | 0.05 | epoch | — | 2 |
| 7 | LightOnOCR-2-1B | 20 | 2e-4 | 4 | 4 | 1 | TRUE | linear | 10 | - | adamw_torch_fused | 0.001 | epoch | — | 2 |
| 8 | DeepSeek-OCR V2 | 20 | 2e-4 | 2 | 2 | 4 | TRUE | linear | 10 | - | adamw_8bit | 0.001 | steps | 25 | 2 |

---

### Image Processing Parameters

| Exp | Model Family | `LONGEST_EDGE` (LightOnOCR) | `image_size` (DeepSeek) | `base_size` (DeepSeek) | `crop_mode` (DeepSeek) |
|:---:|:---|:---:|:---:|:---:|:---:|
| 1 | DeepSeek | — | 640 | 1024 | ✅ |
| 2 | DeepSeek | — | 768 | 1024 | ✅ |
| 3 | DeepSeek | — | 768 | 1024 | ✅ |
| 4 | DeepSeek | — | 768 | 1024 | ✅ |
| 5 | LightOnOCR | 700 | — | — | — |
| 6 | LightOnOCR | 1024 | — | — | — |
| 7 | LightOnOCR | **1540** | — | — | — |
| 8 | DeepSeek | — | 768 | 1024 | ✅ |

> 💡 **Experiment 7** used the highest input resolution (longest edge = **1540 px**), which is the key differentiator enabling it to resolve fine Sinhala stroke diacritics.

---

### Fine-Tuning Technique Summary

| Exp | Fine-Tuning Technique |
|:---:|:---|
| 1 | LoRA + QLoRA (4-bit), gradient checkpointing, mixed precision |
| 2 | LoRA, gradient checkpointing, mixed precision |
| 3 | LoRA, gradient checkpointing, mixed precision |
| 4 | LoRA + QLoRA (4-bit), gradient checkpointing, mixed precision |
| 5 | LoRA + QLoRA (4-bit), gradient checkpointing, mixed precision |
| 6 | QLoRA (4-bit), gradient checkpointing, mixed precision |
| 7 | QLoRA (4-bit), gradient checkpointing, mixed precision |
| 8 | QLoRA (4-bit), gradient checkpointing, mixed precision |

---

> **Dataset:** All experiments use the `sinhala-ocr-lk-acts-1010` dataset (1,010 samples) with a **70 / 10 / 20** train / validation / test split (707 train · 101 val · 202 test).
