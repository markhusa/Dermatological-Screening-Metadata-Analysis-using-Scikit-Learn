1. Development of a Medical Screening Support System for Rural Areas Using Classical Machine Learning

A research project evaluated the applicability of patient clinical metadata for the primary screening of skin lesions in environments with limited access to specialized healthcare.



2. Motivation and Social Impact

In Kazakhstan, high-quality oncological and dermatological care is primarily concentrated in major urban centers such as Almaty and Astana. Residents of remote rural areas (auls) face a severe shortage of specialized oncologists, which often leads to delayed detection of dangerous skin conditions, including melanoma.

The goal of this work is to investigate how effectively basic patient metadata (age, sex, and anatomical localization of the lesion) can serve as the foundation for a lightweight primary screening system. Such an algorithm is designed to assist medical staff in rural clinics in identifying high-risk patients early and ensuring their rapid referral to regional specialized centers.



3. Tech Stack

Programming Language: Python

Data Manipulation & Mathematics: Pandas, NumPy

Machine Learning: Scikit-Learn (utilizing ColumnTransformer for feature-specific processing and Pipeline to prevent data leakage between splits)

Visualization: Matplotlib, Seaborn



4. Experimental Evaluation and Model Comparison

To find the optimal balance between overall classification accuracy and clinical safety, three different architectures were evaluated on the HAM10000 dataset (10,015 clinical cases). The data split was performed using stratification (stratify=y) to maintain identical proportions of rare conditions in both the training and test sets.



5.1. Logistic Regression (Baseline)

Overall Accuracy: 22%

Melanoma (mel) Recall: 9%

Analysis: The linear approach completely failed. The dependencies within medical metadata are highly non-linear and cannot be effectively separated by a hyperplane.



5.2. Gradient Boosting
  
Overall Accuracy: 70%
  
Melanoma (mel) Recall: 9%
  
Analysis: This experiment demonstrates the core pitfall of optimizing standard metrics on highly imbalanced datasets. The standard GradientBoostingClassifier in Scikit-Learn optimizes for overall accuracy by default. Consequently, it ignored minor classes and predicted the majority class (benign nevi) for almost all objects. From a clinical perspective, this model is unacceptable as it misses 91% of critical cases despite a high formal Accuracy score.



5.3. Random Forest (The Selected Model)
  
Overall Accuracy: 38%
  
Melanoma (mel) Recall: 18%
  
Analysis: This architecture was selected as the optimal model for the current data configuration. By enabling class weight balancing (class_weight='balanced'), the algorithm was forced to account for rare and dangerous conditions. The model captured non-linear patterns, doubling the Recall score for melanoma compared to alternative methods.
  
  
  
6. Feature Importance and Analysis

Feature importance scores were extracted from the final Random Forest model to analyze its decision-making logic:

Patient Age (num__age) emerged as the absolute dominant factor in the decision-making structure (importance score > 0.55). The algorithm independently established a fundamental medical fact: with age, epithelial cells accumulate UV-induced mutations, exponentially increasing the risk of oncopathology.

Anatomical zones highly exposed to solar radiation or mechanical friction (lower extremities, face, trunk) showed significantly higher importance than protected body areas, which correlates with real-world clinical statistics.



7. Conclusion and Future Work

This research mathematically demonstrates that an isolated analysis of text-based patient metadata is insufficient for building fully autonomous diagnostic systems (an 18% recall rate for melanoma is unacceptable for independent clinical deployment).

However, the project successfully proved that classical machine learning methods can extract verified clinical insights from simple tabular data. The logical development of this project lies in integrating this metadata processing pipeline with Computer Vision models to analyze the textural features of digital lesion images simultaneously.
