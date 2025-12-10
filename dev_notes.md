## Development & Experimentation Log

### Project Title: CT Hematoma Classifier – Multi-class

**Author:** Emmanuel Niyi-Oriolowo  
**Repository:** CT Hematoma Classifier – Multi-class  
**Date Started:** 2025-10-30  
**Purpose:** To systematically document experiments, reasoning, and technical changes throughout the model development process.


### Entry — 06-11-2025

#### Changes Implemented

-   Implemented model performance evaluation on both the internal validation split and an external dataset. Computed confusion matrix, precision, recall, and F1-score for both datasets.
    
-   For the external dataset, used scikit-learn directly to better understand the underlying evaluation process used by fastai.
    

#### Results / Observations
- Internal: 64% accuracy, macro F1 = 0.47.
- External: 41% accuracy, macro F1 = 0.40.
- Noted drop in generalization, with high precision but low recall on minority hematoma classes.
- 

#### Reflections / Hypotheses

-   Documentation needs improvement once evaluation results are logged more systematically.
    
-   Current accuracy and precision remain relatively low — likely due to limited and/or noisy training data.
    

#### Next Steps
-   Explore potential improvements to data quality and validation strategy:
    
    -   **Option 1:** Switch the image search engine to Google for higher-quality images (though this may increase cost).
        
    -   **Option 2:** Use my current validation data (small) and apply augmentation, adjusting the training–validation split to around 60/40. The drawback is that I’ll lose an independent validation set, which makes it harder to detect overfitting.
        
    -   **Option 3:** Download and clean search-engine images offline to create a more controlled training dataset, ensuring no overlap with current validation data.

 

### Entry — 07-11-2025

####  Changes Implemented

-   Added data augmentation 
    

#### Results / Observations
- Changes to accuracy and F1 were minimal in both internal and external data sets 

####  Reflections / Hypotheses

-   I think augmentation was not so significant because the initial data was of low quality.
-   Another issue i noticed was the inconsistency in the results because the search_images function retruns different images on each run. So each session is run on a different data set
-   This makes tracking changes and progress more difficult
-   The best step would be to adopt option 3 where I manually clean a downloaded dataset offline
    

#### Next Steps
        
-  Download and clean search-engine images offline to create a more controlled training dataset, ensuring no overlap with current validation data.
-  Look in to Grad-CAM




### Entry — 08-11-2025

**Changes:**

-   Uploaded a cleaned dataset for the data loaders.
    
-   Initially applied `batch_transformers` augmentation. Later, removed `batch_transformers` augmentation to evaluate its impact.
    

**Results:**
**Internal Validation:**  
- Accuracy: 0.553 → 0.63 (+0.077)  
- Precision: 0.545 → 0.64 (+0.095)  
- Recall: 0.539 → 0.61 (+0.071)  
- F1-score: 0.533 → 0.60 (+0.067)  

**External Validation:**  
- Accuracy: 0.39 → 0.48 (+0.09)  
- Precision: 0.50 → 0.59 (+0.09)  
- Recall: 0.40 → 0.49 (+0.09)  
- F1-score: 0.40 → 0.51 (+0.11)


**Observations:**

-   Removing augmentation led to notable improvements in all metrics:
    
    -   **Internal validation:** Accuracy +13%, F1-score +6.7%
        
    -   **External validation:** Accuracy +9%, F1-score +11%
        
-   The most frequently confused classes are **normal**, **epidural hematoma**, and **subdural hematoma**, suggesting these cases may require additional attention or more distinct features.
    
-   Augmentation likely harmed performance on low-resolution images by obscuring subtle details critical for detecting medical changes.
    

**Next Steps:**

-   Experiment with a different model architecture to see if performance can be further improved.
    
-   Implement Grad-CAM for model interpretability once the architecture is stable.
    
-   Continue monitoring internal vs external performance to evaluate generalization and robustness.



### Entry — 09-11-2025

**Changes:**

-   Tried different model architectures with varying performance, but decided to keep ResNet-18.
-   Increased training to 5 epochs. 
    

**Results:**
**Internal Validation:**  
- Accuracy: 0.63 → 0.67 (absolute +0.04, +6.35% relative)
- Precision: 0.64 → 0.65 (absolute +0.01, +1.56% relative)
- Recall: 0.61 → 0.64 (absolute +0.03, +4.92% relative)
- F1-score: 0.60 → 0.65 (absolute +0.05, +8.33% relative) 

**External Validation:**  
- Accuracy: 0.48 → 0.56 (absolute +0.08, +16.67% relative)
- Precision: 0.59 → 0.62 (absolute +0.03, +5.08% relative)
- Recall: 0.49 → 0.57 (absolute +0.08, +16.33% relative)
- F1-score: 0.51 → 0.57 (absolute +0.06, +11.76% relative)


**Observations:**

-   Overall improvement across internal and external metrics after increasing epochs and sticking with ResNet-18.
-   Precision for the epidural class — previously low — has increased, which is encouraging.

I observed variability in training outcomes even when using the same data loaders and seed. This suggests possible sources of nondeterminism. I should investigate and enforce determinism where needed.
    

**Next Steps:**

-   Implement Grad-CAM for model interpretability once the architecture is confirmed stable.
-   Investigate training variability



### Entry — 11-12-2025
**Summary of Model Performance (Internal & External Validation)**

#### Internal Validation (held-out split of training set)

-   **Overall accuracy:** 61%
    
-   **Best-performing classes:**
    
    -   _Others_ (F1 = 0.95)
        
    -   _Intracerebral hematoma_ (F1 = 0.75)
        
    -   _Normal brain CT_ (F1 = 0.62)
        
-   **Most challenging classes:**
    
    -   _Epidural hematoma_ (F1 = 0.45)
        
    -   _Subdural hematoma_ (F1 = 0.45)
        

**Interpretation:**  
The model separates well-defined categories such as _others_ and _intracerebral hematoma_, but struggles with visually similar entities (e.g., _epidural_ vs _subdural_). This behaviour is expected in radiology datasets where subtle boundary and density differences make classification more difficult.

#### External Validation (fully unseen dataset)

-   **Overall accuracy:** 52%
    
-   **Best-performing classes:**
    
    -   _Intracerebral hematoma_ (F1 = 0.62)
        
    -   _Subarachnoid hematoma_ (F1 = 0.55)
        
    -   _Others_ (F1 = 0.67)
        
-   **Most challenging classes:**
    
    -   _Normal brain CT_ (F1 = 0.40)
        
    -   _Subdural hematoma_ (F1 = 0.38)
        

**Interpretation:**  
As expected, accuracy drops on external images due to **domain shift**—differences in scanners, acquisition protocols, slice thickness, and patient populations.  
Nevertheless, several classes maintain reasonable performance, suggesting the model is learning generalizable features rather than overfitting entirely.

#### Generalization Gap (Internal → External)

| Metric   | Internal | External |
|----------|----------|----------|
| Accuracy | 0.61     | 0.52     |
| Macro F1 | 0.63     | 0.52     |


-   The **~9% reduction in accuracy** and **~0.11 drop in macro F1** indicate **moderate overfitting**.
    
-   This behaviour is typical for small, heterogeneous medical imaging datasets and reinforces the importance of evaluating models on external data.
    

#### Key Insights

-   The model excels on hematoma types with clearer radiologic patterns.
    
-   _Normal_ and _subdural_ scans remain challenging across datasets.
    
-   External dataset performance suggests the model is **reasonably robust**, though clearly affected by domain shift.
    
-   Further improvements could include:
    
    -   Enhanced data augmentation
        
    -   Domain adaptation techniques
        
    -   Training with larger or more diverse datasets
        
    -   Stronger backbones such as **ResNet-50, ConvNeXt, or EfficientNet**
        

The GradCAM heatmap shows that the model focused on the left-sided extra-axial region, where the abnormal hyperdensity is located. The highlighted area also overlaps with the region responsible for the visible midline shift, suggesting that the model is paying attention to both the hematoma and its associated mass effect. This indicates that the model’s prediction is guided by clinically relevant anatomical features rather than unrelated image areas.**