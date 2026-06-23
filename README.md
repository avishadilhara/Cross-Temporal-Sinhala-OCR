# Cross-Temporal Sinhala OCR: Page-Level Adaptation and Diachronic Analysis

> **Note:** The dataset and model weights linked below are currently private and will be made public upon the release of the associated research paper.

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
* **`sinhala-ocr-lk-acts-1010`**: A dataset of 1,010 page-level image-text pairs split into 707 training, 101 validation, and 202 testing examples.
  * [Dataset Repository](https://huggingface.co/datasets/avishadilhara/sinhala-ocr-lk-acts-1010) 

### Models
* **`sinhala-lightonocr-2-1b-Qlora`**: The best-performing fine-tuned VLM from our experiments, capable of robust page-level Sinhala OCR.
  * [Model Repository](https://huggingface.co/avishadilhara/sinhala-lightonocr-2-1b-Qlora)
* **`sinhala-deepseek-ocr-Qlora`**: QLoRA fine-tuned weights for the DeepSeek-OCR model family.
  * [Model Repository](https://huggingface.co/avishadilhara/sinhala-deepseek-ocr-Qlora)

## Authors
* **Avisha Dilhara** - School of Computing, Informatics Institute of Technology, Sri Lanka
* **Nevidu Jayatilleke** - Department of Computer Science & Engineering, University of Moratuwa, Sri Lanka
