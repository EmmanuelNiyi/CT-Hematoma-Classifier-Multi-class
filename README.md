# CT Hematoma Classifier – Multi-Class

## Overview

This project explores the use of deep learning for **multi-class classification of intracranial hematomas** on non-contrast brain CT images. The work is framed as a **research and experimentation prototype**, with emphasis on dataset construction, model evaluation, generalization, and interpretability rather than clinical deployment.

The long-term objective is to build a robust classifier capable of distinguishing between multiple hematoma subtypes. This version focuses on establishing a reproducible workflow, understanding model behaviour under data constraints, and evaluating performance on both internal and external datasets.

---

## Clinical Motivation

Intracranial hemorrhages are medical emergencies where timely and accurate diagnosis is critical. While deep learning has shown promise in radiology, real-world performance is often limited by:

- small or noisy datasets,
- domain shift between scanners and institutions,
- and lack of interpretability.

This project aims to explicitly address these challenges by:

- evaluating generalization beyond the training data,
- analyzing class-specific failure modes,
- and applying explainability techniques (Grad-CAM) to assess whether model predictions are driven by clinically meaningful regions.

---

## Dataset

- **Source:** Brain CT images collected from internet search engines.
- **Classes:** Normal CT, Epidural, Subdural, Intracerebral, Subarachnoid, and Others.
- **Characteristics:**
    - Initial datasets were noisy and weakly labeled.
    - Images varied in resolution, quality, and acquisition style.
- **Mitigation:**
    - Images were downloaded offline and manually cleaned.
    - Broken and unreadable files were removed.
    - Separate datasets were maintained for **internal validation** and **external evaluation** to assess generalization.

This approach prioritizes **controlled experimentation** over raw dataset size.

---
## Dataset Access

The final datasets used for **training** and **external validation** are hosted on Google Drive to ensure reproducibility and transparency.

-   **Training Dataset (Cleaned):**
    
    [Google Drive link – Training Set](https://drive.google.com/drive/folders/1H0-QipShexOVeKHGGb5btqxn4-JXgvMA?usp=drive_link)
    
-   **External Validation Dataset:**
    
    [Google Drive link – Validation Set](https://drive.google.com/drive/folders/1FFj34tkjsjA5aZ0xOJ-HK0e_Q0BP-U5H?usp=drive_link)
    

### Notes on Dataset Usage

-   Datasets were **downloaded, cleaned, and curated offline** to reduce noise and eliminate broken or duplicate images.
-   The validation dataset was kept **fully separate** from the training data to enable an unbiased assessment of generalization.
-   Images originate from heterogeneous sources and reflect real-world variability in acquisition quality and protocols.
-   The datasets are intended **strictly for research and educational purposes**.

> ⚠️ Important: These datasets are not clinically validated and must not be used for diagnostic or clinical decision-making. Access to the datasets is provided to support reproducibility of the reported experiments.

---
## Methodology

### Model Architecture

- Backbone: **ResNet-34**
- Pretrained on ImageNet and fine-tuned for multi-class classification.
- Alternative architectures were tested, but ResNet-34 offered the best balance between performance and stability on the available data.

### Training Strategy

- Train–validation split for internal evaluation.
- Separate **external dataset** used exclusively for generalization testing.
- Training conducted for up to **5 epochs**, with performance monitored across runs.
- Data augmentation was tested but ultimately removed after degrading performance on low-resolution medical images.

### Reproducibility

- Variability was observed across runs despite fixed seeds, highlighting sources of nondeterminism in deep learning pipelines.
- This behaviour is documented and treated as a known limitation.

---

## Evaluation & Results

### Internal Validation

- **Overall accuracy:** ~61%
- Strong performance on visually distinct classes such as:
    - Intracerebral hematoma
    - “Others”
- Reduced performance on visually similar entities:
    - Epidural vs Subdural hematomas

### External Validation

- **Overall accuracy:** ~52%
- Expected performance drop due to **domain shift** (scanner differences, acquisition protocols).
- Several classes maintained reasonable F1-scores, suggesting partial generalization rather than complete overfitting.

### Generalization Gap

| Metric | Internal | External |
| --- | --- | --- |
| Accuracy | 0.61 | 0.52 |
| Macro F1 | 0.63 | 0.52 |

The moderate gap reflects typical behaviour for small, heterogeneous medical imaging datasets and reinforces the importance of external evaluation.

---

## Model Interpretability (Grad-CAM)

Grad-CAM was applied to visualize spatial regions influencing model predictions.

Observations:

- Activation maps frequently highlight **extra-axial regions** corresponding to hematoma location.
- In some cases, attention overlaps with areas of **midline shift**, suggesting sensitivity to mass effect rather than irrelevant image features.

These findings indicate that the model is learning **clinically meaningful cues**, though interpretability remains qualitative and exploratory.

---

## Limitations

- Small and weakly labeled dataset.
- Residual domain shift between datasets.
- Reduced performance on visually similar hematoma subtypes.
- Training nondeterminism despite fixed seeds.
- Model not validated for clinical decision-making.

---

## Future Work

- Expand and standardize datasets across institutions.
- Investigate domain adaptation techniques.
- Experiment with stronger backbones (ResNet-50, EfficientNet, ConvNeXt).
- Refine augmentation strategies specific to medical imaging.
- Perform slice-level or volume-level modeling instead of single-image classification.