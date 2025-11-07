# CT Hematoma Classifier – Multi-class

Welcome to my CT Hematoma Classifier. This project is designed as a multi-class image classification model for brain CT scans.

---

## Overview

Although the ultimate goal is a multi-class classifier for different types of hematomas, this initial version focuses on creating a small dataset, training a model, automatically testing it, and calculating accuracy.  

The model is trained using images collected from the internet. The dataset is somewhat “dirty” and contains errors, but this approach allows rapid experimentation and testing.  

In future versions, I plan to:
- Scrape, clean, and validate a larger, high-quality dataset to improve model reliability.
- Implement data augmentation strategies to enhance model generalization and accuracy.

Explore explainability techniques (e.g., Grad-CAM or feature attribution) to visualize model reasoning and build clinician trust.

---

## Key Steps in This Notebook

1. **Setup and Library Installation**  
   - Import essential Python libraries (`numpy`, `pandas`, `fastai`, `ddgs`, `requests`).  
   - Ensure correct versions are installed for compatibility.

2. **Image Downloading**  
   - Search and download sample images for each category using `DDGS` and `requests`.  
   - Display thumbnails to verify successful downloads.

3. **Dataset Preparation**  
   - Organize images into category folders.  
   - Verify and remove broken or unreadable images.  
   - Resize images to a consistent size for model input.

4. **Data Loading and Transformation**  
   - Create a `DataBlock` with an 80/20 train-validation split.  
   - Apply image resizing transformations for model training.  
   - Display a batch of images to visually inspect the dataset.

5. **Model Training**  
   - Create a `ResNet18` vision learner.  
   - Fine-tune the pretrained model for 3 epochs.  
   - Track performance using `error_rate`.

6. **Validation and Accuracy Calculation**  
   - Load a separate validation dataset.  
   - Predict classes for all images and save results to CSV.  
   - Compute per-category and overall accuracy.
7. **Testing with New Images**  
   - Download sample test images.  
   - Predict categories using the trained model.  
   - Display predicted class and probability of being an epidural hematoma.

---

## Notes

- This version is primarily experimental and focuses on workflow: dataset creation, model training, testing, and evaluation.  
- Dataset quality is limited due to reliance on uncurated internet images.  
- Future versions will focus on cleaning datasets and improving accuracy.

---

## License / Disclaimer

This notebook is for learning and experimentation purposes. The model is **not intended for clinical use**, and the dataset may contain mislabeled or incorrect images. Always consult a qualified professional for medical diagnoses.
