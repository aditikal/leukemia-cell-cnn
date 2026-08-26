# CNN-Based Cancer Cell Classification

## Overview

This project investigates whether isolating white blood cells (WBCs) from microscope images improves CNN classification of benign and acute lymphoblastic leukemia (ALL) cells.

## Research Question

Does removing red blood cells and emphasizing white blood cells
improve classification performance?

## Dataset
This project uses the ALL (Acute Lymphoblastic Leukemia) Image Dataset from Kaggle, containing 3,256 peripheral blood smear (PBS) images from 89 suspected ALL patients collected at Taleqani Hospital in Tehran, Iran. All images were prepared and stained by laboratory staff and captured using the same Zeiss microscope and camera at 100× magnification, with images saved as JPG files.

The original dataset contains benign hematogones and three malignant ALL subtypes: Early Pre-B, Pre-B, and Pro-B. For this experiment, the Pre-B and Pro-B malignant subtypes were combined into a single malignant class, resulting in a binary classification task of benign vs. malignant. The Early Pre-B subtype was not used. 

## Methodology

### Pipeline 1: Raw Images
...

### Pipeline 2: WBC Isolation
...

## Results
| Metric               |         Raw Images |       WBC Isolation |
| -------------------- | -----------------: | ------------------: |
| **Malignant Recall** | **98.29% ± 1.33%** | **90.07% ± 14.25%** |
| **Benign Recall**    |     86.97% ± 5.79% |      86.70% ± 6.26% |
| Accuracy             |     95.77% ± 1.33% |     88.78% ± 11.10% |


## Model Interpretability

[Grad-CAM explanation and images]

## Limitations

...

## Conclusion

Two CNN classification pipelines were evaluated across 10 random seeds (42–51) to determine whether isolating white blood cells (WBCs) improved the classification of benign and malignant bone marrow cell images. Pipeline 1 used the original microscope images, while Pipeline 2 used images processed to isolate the white blood cells.


### Pipeline 1: Raw Images
Pipeline 1 demonstrated consistently strong performance in detecting malignant cells. The model achieved an average malignant recall (sensitivity) of 98.29% ± 1.33% across the 10 runs. This means that, on average, the model correctly identified approximately 98% of the malignant images.

The model's benign recall (specificity) was 86.97% ± 5.79%, indicating that it was less consistent at correctly identifying benign images than malignant images. Despite this difference, malignant recall remained high across all 10 seeds, ranging from 95.6% to 99.7%. The relatively small standard deviation also indicates that the model's ability to detect malignant cells was consistent across different random seeds.


### Pipeline 2: WBC Isolation
Pipeline 2 showed lower and substantially more variable malignant detection performance. The average malignant recall was 90.07% ± 14.25%, compared with 98.29% ± 1.33% for Pipeline 1.

The larger standard deviation indicates that the performance of the WBC-isolation pipeline was much less consistent between runs. Malignant recall ranged from 47.6% to 97.4%. In particular, Seed 48 produced a substantial increase in false-negative predictions, with 188 malignant images incorrectly classified as benign. This resulted in a malignant recall of only 47.6% for that run.

The average benign recall for Pipeline 2 was 86.70% ± 6.26%, which was very similar to Pipeline 1's 86.97% ± 5.79%. This suggests that WBC isolation did not meaningfully improve the model's ability to identify benign images either.


### Comparison of the Two Pipelines
The primary difference between the two pipelines was their performance on the benign class, which was less represented in the dataset. Pipeline 1 achieved a benign recall of 86.97% ± 5.79%, while Pipeline 2 achieved 86.70% ± 6.26%, showing that WBC isolation did not improve the model's ability to identify benign images. Although the average benign recall was nearly identical, Pipeline 2 showed greater variability across runs.

The false-negative results further support this difference. Pipeline 1 consistently produced relatively few false negatives, whereas Pipeline 2 produced considerably more in several runs, with Seed 48 producing the largest number. Since false negatives represent malignant images that the model failed to identify, this variation is particularly important when evaluating the pipeline for cancer detection.

Overall, WBC isolation did not improve malignant-cell detection in this experiment. Instead, the raw-image pipeline achieved higher malignant sensitivity and substantially greater consistency across random seeds.

## Technologies

- Python
- TensorFlow / Keras
- OpenCV
- NumPy
- Matplotlib
- scikit-learn
- Google Colab

## Dataset Citation

Mehrad Aria, Mustafa Ghaderzadeh, Davood Bashash, Hassan Abolghasemi, Farkhondeh Asadi, and Azamossadat Hosseini, “Acute Lymphoblastic Leukemia (ALL) image dataset.” Kaggle, (2021). DOI: 10.34740/KAGGLE/DSV/2175623.
