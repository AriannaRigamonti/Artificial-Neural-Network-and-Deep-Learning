# AN2DL Homework 1: Multi-Class Blood Cell Classification  

## Team Members  
- **Luca Lepore**  
- **Arianna Rigamonti**  
- **Michele Sala**  
- **Jacopo Libero Tettamanti**  

## Project Overview  
This project focuses on **multi-class image classification** of blood cells using **deep learning techniques**. The goal is to develop a **robust** and **generalizable** model capable of accurately classifying blood cell images into their respective categories.  

Our approach follows a structured pipeline, starting from a **baseline CNN** and progressively implementing **regularization techniques**, **transfer learning**, and **hyperparameter tuning** to enhance performance. The final models were evaluated on both a standard **test set** and the **Codabench platform**.  

## Dataset  
- **Total Images:** 13,759 RGB images (96×96 pixels)  
- **Classes (8):**  
  - Basophils  
  - Eosinophils  
  - Erythroblasts  
  - Immature granulocytes  
  - Lymphocytes  
  - Monocytes  
  - Neutrophils  
  - Platelets  
- **Challenges:**  
  - **Class imbalance**  
  - **Duplicate images**  
  - **Variability in staining, shape, and orientation**  

## Methodology  

### 1. Data Preprocessing  
- **Duplicate removal:** Used **Perceptual Hashing (pHash)** to detect and remove **1,806 duplicate images**.  
- **Data Splitting:** 60% Training, 20% Validation, 20% Test.  

### 2. Data Augmentation  
To improve generalization and simulate real-world variations, we implemented:  
- **RandAugment** (random transformations with intensity tuning)  
- **AugMix** (multiple augmentation chains with blending)  
- **Expanded dataset size:**  
  - Training & Validation: **3× original size**  
  - Test: **4× original size**  

### 3. Model Architectures  

#### **Baseline & Regularized CNNs**  
- **Baseline:** A simple **2-block CNN** with ReLU activations and MaxPooling.  
- **Regularized CNN:** Added **Batch Normalization** and **Dropout** (0.3 & 0.4).  

#### **Transfer Learning (Pretrained Models)**  
We tested several **Keras Applications models**, selecting:  
- **MobileNetV3**  
- **EfficientNetV2M**  
- **ConvNeXtBase**  
- **VGG16**  

Each model underwent a **two-phase training pipeline**:  
1. **Feature Extraction:** Frozen convolutional layers, only training the classifier head.  
2. **Fine-Tuning:** Unfrozen top layers, training with a lower learning rate.  

#### **Hyperparameter Tuning**  
- Used **Hyperband Algorithm** to optimize:  
  - Number of **Dense layers**  
  - **Neurons per layer**  
  - **Dropout rate**  
  - **Learning rate**  

## Experiments & Results  
All models were evaluated on **three test sets**:  
- **Standard Test Set**  
- **Augmented Test Set**  
- **Codabench Evaluation**  

### **Performance Summary**  
| Model | Test Acc (%) | Aug Test Acc (%) | Codabench Acc (%) |  
|--------|------------|----------------|----------------|  
| **Baseline CNN** | 57.9 | 30.9 | 19 |  
| **Regularized CNN** | 79.9 | 42.2 | 30 |  
| **MobileNetV3** | 92.5 | 62.2 | 70 |  
| **VGG16** | 96.4 | 71.0 | 80 |  
| **EfficientNetV2M** | 98.4 | 80.1 | 92 |  
| **ConvNeXtBase** | 98.1 | 80.0 | 92 |  

### **Post-Hyperparameter Tuning Results**  
| Model | Validation Acc (%) | Aug Test Acc (%) | Codabench Acc (%) |  
|--------|----------------|----------------|----------------|  
| **EfficientNetV2M** | 89.0 | 78.0 | 91.0 |  
| **ConvNeXtBase** | 89.6 | 76.8 | 90.0 |  

## Key Findings & Discussion  
- **Transfer learning significantly outperformed** our baseline CNN models.  
- **Data augmentation was critical** for handling dataset variability but required careful tuning.  
- **Hyperparameter tuning yielded marginal improvements**, indicating that our empirically chosen configurations were already close to optimal.  
- **EfficientNetV2M and ConvNeXtBase were the best-performing models**, achieving **92% Codabench accuracy**.  
- **Regularized CNNs improved generalization**, but still underperformed compared to pretrained models.  

## Future Improvements  
To further refine the model, future work could include:  
- **Longer training schedules** with **adaptive learning rate schedules**.  
- **More extensive hyperparameter tuning**, including optimizer selection.  
- **Exploring additional architectures**, such as ViTs or hybrid CNN-transformer models.  
- **Advanced regularization techniques**, such as MixUp and CutMix.  

## References  
1. Vinay Kumar, Abul K. Abbas, and Jon C. Aster. *Robbins & Cotran Pathologic Basis of Disease*, 9th edition.  
2. P. Subudhi and K. Kumari. *Large-scale near duplicate image retrieval using perceptual hashing*. Signal, Image and Video Processing, 2024.  
3. L. Li et al. *Hyperband: A novel bandit-based approach to hyperparameter optimization*. JMLR, 2018.  
