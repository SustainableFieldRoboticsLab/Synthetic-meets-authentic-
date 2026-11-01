# Synthetic Meets Authentic (Part II)

## Field-Imaging-Free Auto-Annotation and Multi-Orchard Field Validation for Apple Detection and Segmentation

This repository accompanies our study of apple instance segmentation using **synthetically generated training images and automatically generated annotations**.

The workflow combines DALL·E-generated orchard imagery, pretrained YOLO11-based apple localization, and SAM2-based instance-mask generation to prepare training data for standalone YOLO11-seg models. Evaluation uses real orchard imagery from Washington State and New York.

**Scope:** “Field-imaging-free” refers to task-specific training-data preparation. Real field images and reference annotations remain necessary for independent evaluation. Pretrained components also retain knowledge acquired from their original training datasets.

## Research Objective

Can generated orchard images and automatic mask annotations provide useful training supervision for models subsequently applied to real orchard imagery?

The contribution is an evaluated **training-data preparation workflow**, rather than a new detection algorithm or a demonstration of superiority over all real-data training alternatives.

The study investigates:

- Synthetic orchard-image generation for training.
- Automatic apple localization and instance-mask annotation.
- Training and evaluation of five YOLO11-seg configurations.
- Transfer to real orchard imagery.
- Counting agreement across five additional apple cultivar/accession groups.

## Workflow

1. **Generate synthetic images:** Create orchard scenes using text prompts describing apple appearance, fruit density, canopy conditions, lighting, and occlusion.
2. **Review generated images:** Screen images for visual plausibility and suitability. This curation involves human judgment.
3. **Localize apples:** Apply a pretrained YOLO11 detector to generate bounding-box prompts.
4. **Generate annotations:** Supply these boxes to SAM2 to produce instance masks and export segmentation labels.
5. **Train segmentation models:** Train YOLO11n-seg, YOLO11s-seg, YOLO11m-seg, YOLO11l-seg, and YOLO11x-seg using the synthetic, automatically annotated dataset.
6. **Evaluate on real images:** Assess segmentation on Washington State imagery and counting agreement on additional New York imagery.

**SAM2 is used during annotation preparation, not during inference by the trained YOLO11-seg models.**

## Evaluation Data

### Washington State: Segmentation Evaluation

The original field evaluation uses 42 real images from a commercial Scifresh apple orchard in Prosser, Washington, USA.

Images were acquired using a Microsoft Azure Kinect DK machine-vision camera. This evaluation assesses box- and mask-level performance against reference annotations.

### New York: Additional Counting Evaluation

The additional evaluation includes 919 RGB images from Cornell orchards in New York.

| Cultivar/accession label | Processed folder | Images |
|---|---|---:|
| Rootbag Porter’s Perfection | `rgb_videoblock70` | 191 |
| Fuji | `rgb_videoFuji2016` | 190 |
| Honeycrisp | `rgb_videohoneycrisp` | 162 |
| Snapdragon | `rgb_videosnapdragon` | 195 |
| Spijon-pomiferous | `rgb_videospijon` | 181 |
| **Total** | | **919** |

These are the supplied cultivar/accession labels; botanical species names are not inferred from folder names.

The New York evaluation measures **per-image counting agreement**. Neighboring frames may contain repeated views of the same apples, so summed counts are not unique tree or orchard fruit totals.

## Selected Results

Automatic mask annotation achieved:

- **Dice coefficient:** 0.9513
- **Intersection-over-union:** 0.9303

For the Washington State field evaluation, the manuscript’s current results table reports:

- **Highest mask precision:** YOLO11m-seg, 0.902.
- **Highest mask mAP@50:** YOLO11n-seg, 0.848.

These metrics identify different performance characteristics; no single configuration is described as universally best.

### New York Counting Results

| Group | MAE | RMSE | Mean bias | R² |
|---|---:|---:|---:|---:|
| Rootbag Porter’s Perfection | 2.921 | 14.747 | −2.723 | 0.155 |
| Fuji | 1.211 | 1.658 | −0.905 | 0.947 |
| Honeycrisp | 0.593 | 1.024 | −0.519 | 0.950 |
| Snapdragon | 0.605 | 1.086 | −0.482 | 0.942 |
| Spijon-pomiferous | 0.398 | 0.780 | −0.320 | 0.948 |
| **Pooled** | **1.169** | **6.806** | **−1.010** | **0.542** |

MAE, RMSE, and bias are expressed in apples/image. Negative bias indicates undercounting.

Across all 919 images:

- Predicted frame-summed count: **10,746**
- Reference frame-summed count: **11,674**
- Net undercount: **7.95%**
- Exact count agreement: **44.07%**
- Agreement within ±1 apple: **78.67%**
- Agreement within ±2 apples: **89.77%**

### Important Data-Review Note

The analyzed workbook contains a reference count of **222** and a prediction of **21** for:

`rgb_videoblock70/image18.jpg`

This observation remains included and contributes approximately **94.90% of pooled squared error**. The statistics above describe that retained-data analysis.

Verify the entry against the original image and reference record before using these results in a final publication. If a correction is confirmed, update the workbook and regenerate all dependent tables, figures, and summaries. Do not remove the observation solely because it worsens performance.

## Running the Counting Analysis

The accompanying analysis files are:

- `ny_apple_validation.py`
- `NY_Apple_Validation.ipynb`

These analyze an existing prediction–reference workbook. They do **not** generate images, train models, or rerun model inference.

Install dependencies:

```bash
python -m pip install numpy pandas scipy matplotlib openpyxl
