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
The original microscope images were used as input to a convolutional neural network (CNN) without additional image segmentation. The dataset was divided into training and testing sets, with benign and malignant images represented in each set. The CNN was trained to perform binary classification, and class weights were used to account for the unequal number of benign and malignant images. Model performance was evaluated using accuracy, confusion matrices, precision, recall, and F1-score across 10 random seeds (42–51).

### Pipeline 2: WBC Isolation
The same original images were preprocessed using OpenCV to isolate the purple-stained white blood cells (WBCs) from the surrounding image. The images were converted to HSV color space, and a color threshold was applied to create a mask identifying the WBC regions. The isolated WBCs were placed onto a white background before being provided to the same CNN classification process used in Pipeline 1. Performance was evaluated across the same 10 random seeds using the same metrics, allowing the two pipelines to be directly compared.

## Results
| Metric               |         Raw Images |       WBC Isolation |
| -------------------- | -----------------: | ------------------: |
| **Malignant Recall** | **98.29% ± 1.33%** | **90.07% ± 14.25%** |
| **Benign Recall**    |     86.97% ± 5.79% |      86.70% ± 6.26% |
| Accuracy             |     95.77% ± 1.33% |     88.78% ± 11.10% |


## Model Interpretability
Grad-CAM was used to visualize which regions of the images contributed to the CNN's predictions. These visualizations provide a qualitative look at what the models emphasized when making their classifications. Representative examples from both pipelines are shown below.

Pipeline 1 (Raw Image):
<img width="1125" height="353" alt="image" src="https://github.com/user-attachments/assets/a1bdc973-c2aa-403a-8fb3-122d8d2dcf69" />

Pipeline 2 (WBC Isolation): 
<img width="1125" height="351" alt="image" src="https://github.com/user-attachments/assets/acbc5d77-85af-4b85-82a0-ab8c66d0e708" />

## Limitations

The dataset contains more malignant than benign images, which may affect model performance and makes accuracy alone less representative of performance for each class. Class weights were used during training to help account for this imbalance.

The dataset also consists of images collected from a single hospital using the same microscope, camera, and staining procedures. As a result, the model's performance may not generalize to images collected using different equipment, staining protocols, or laboratory environments.

The WBC isolation process relied on manually selected HSV color thresholds. Differences in cell appearance, staining, and image quality may cause the segmentation to include unwanted regions or remove relevant information. Pipeline 2 also showed substantially greater variation across random seeds than Pipeline 1, indicating that the WBC-isolation approach produced less stable model performance.

The Grad-CAM visualizations also showed variability in the regions emphasized by the models. In some examples, higher activation appeared around the background rather than the cell, while other images showed stronger activation around cellular regions. This suggests that the model may sometimes rely on image features outside the cell itself and provides additional qualitative evidence of variability in the model's behavior. However, because Grad-CAM is an interpretability tool rather than a direct measure of model attention it was used as a supplementary qualitative analysis rather than a primary evaluation method. Because the main objective of this project was to compare the classification performance of the two pipelines, further optimization or quantitative evaluation of the Grad-CAM visualizations was outside the scope of this project.

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

## Further Research

Future work could investigate methods for improving the stability and performance of the WBC-isolation pipeline. One possible approach would be to use a pretrained CNN, such as ResNet, rather than training the model from scratch. Transfer learning could potentially provide more robust feature extraction and reduce the variation observed between training runs. Additional segmentation methods could also be explored to determine whether more accurate WBC isolation improves classification performance.

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
