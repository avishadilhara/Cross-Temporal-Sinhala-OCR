[![arXiv](https://img.shields.io/badge/arXiv-2606.29378-B31B1B?style=flat-square&logo=arxiv)](https://arxiv.org/abs/2606.29378)
[![MERCon 2026](https://img.shields.io/badge/Accepted-MERCon%202026-2EA3F7?style=flat-square)](https://arxiv.org/abs/2606.29378)
[![CER](https://img.shields.io/badge/CER-1.05%25-brightgreen?style=flat-square)](https://arxiv.org/abs/2606.29378)


# Cross-Temporal Sinhala OCR: Page-Level Adaptation and Diachronic Analysis

## About the Research

Optical Character Recognition (OCR) for low-resource and complex-script languages like Sinhala often suffers from lower accuracy compared to high-resource languages. Furthermore, existing Sinhala OCR evaluations have primarily been conducted on synthetic images or limited font sets, with no publicly available datasets of real, printed Sinhala documents.

This research addresses these gaps through three main contributions:
1. **New Dataset (`sinhala-ocr-lk-acts-1010`)**: We introduce the first real-world, page-level dataset for Sinhala OCR, containing 1,010 annotated and hand-corrected page-level image-text pairs sourced from Sri Lankan Legislative Acts spanning three eras (1981–1989, 2000–2009, 2010–2019).
2. **Page-Level VLM Adaptation**: We fine-tuned multi-billion-parameter Vision-Language Models (VLMs)—specifically **DeepSeek-OCR V1**, **DeepSeek-OCR V2**, and **LightOnOCR-2-1B**—using parameter-efficient QLoRA. Our top-performing model, LightOnOCR-2-1B, achieved a Character Error Rate (CER) of 1.05%, surpassing state-of-the-art open-source engines and commercial solutions like Google Document AI.
3. **Diachronic Evaluation**: We conducted the first diachronic evaluation of page-level Sinhala OCR across three distinct temporal periods, quantifying how historical print degradation affects text recognition accuracy.

## Key Findings

* **State-of-the-Art Accuracy**: **LightOnOCR-2-1B** is the overall top performer, achieving **1.05% CER** and **5.63% WER**, outperforming Google Document AI (2.06% CER) and traditional open-source baselines like Surya-OCR and Tesseract v5.
* **Diachronic Degradation**: We confirmed that OCR accuracy systematically deteriorates with historical print quality. Older, highly degraded documents (1981-1989) exhibit higher error rates across all evaluated systems compared to modern documents (2010-2019).
* **Robustness of VLMs**: Legacy line-segmentation-based OCR systems suffer from "diachronic collapse" on degraded historical texts (e.g., Tesseract v5 reaching ~70% WER on 1980s documents), whereas our fine-tuned page-level VLMs maintain significantly greater robustness and resilience.

## Resources

The following resources have been developed as part of this research. They are hosted on Hugging Face but remain **private** until the research paper is officially released.

### Dataset
* **`sinhala-ocr-lk-acts-1010`**: All experiments use the `sinhala-ocr-lk-acts-1010` dataset (1,010 samples) with a **70 / 10 / 20** train / validation / test split (707 train · 101 val · 202 test)
  * [Dataset Repository](https://huggingface.co/datasets/avishadilhara/sinhala-ocr-lk-acts-1010) 

### Models
* **`sinhala-lightonocr-2-1b-Qlora`**: The best-performing fine-tuned VLM from our experiments, capable of robust page-level Sinhala OCR.
  * [Model Repository](https://huggingface.co/avishadilhara/sinhala-lightonocr-2-1b-Qlora)
* **`sinhala-deepseek-ocr-Qlora`**: QLoRA fine-tuned weights for the DeepSeek-OCR model family.
  * [Model Repository](https://huggingface.co/avishadilhara/sinhala-deepseek-ocr-Qlora)


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

| Exp | Base Model |  LoRA Rank (`lora_r`) | LoRA Alpha (`lora_alpha`) | LoRA Dropout | Quantization (`load_in_4bit`) |
|:---:|:---:|:---:|:---:|:---|:---|
| 1 | DeepSeek-OCR V1 | 16 | 16 | 0 | TRUE (4-bit NF4) |
| 2 | DeepSeek-OCR V2 | 32 | 64 | 0.1 |  FALSE (16-bit) |
| 3 | DeepSeek-OCR V2 | 16 | 16 | 0 |  FALSE (16-bit) |
| 4 | DeepSeek-OCR V2 | 16 | 16 | 0 |  TRUE (4-bit NF4) |
| 5 | LightOnOCR-2-1B | 16 | 16 | 0.05 |  TRUE (4-bit NF4) |
| 6 | LightOnOCR-2-1B | 32 | 64 | 0.1 |  TRUE (4-bit NF4) |
| 7 | LightOnOCR-2-1B | 32 | 64 | 0.1 |  TRUE (4-bit NF4) |
| 8 | DeepSeek-OCR V2 | 16 | 16 | 0 |  TRUE (4-bit NF4) |

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


## Authors
* **Avisha Dilhara** - School of Computing, Informatics Institute of Technology, Sri Lanka
* **Nevidu Jayatilleke** - Department of Computer Science & Engineering, University of Moratuwa, Sri Lanka
