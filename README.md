# MedSAM-U: UGMP Prototype

This repository contains an implementation prototype of the **Uncertainty-Guided Multi-Prompt (UGMP)** stage described in the MedSAM-U research paper, *MedSAM-U: Uncertainty-Guided Auto Multi-Prompt Adaptation for Reliable MedSAM*.

The project was completed as part of the **IEEE SMC SBC KGEC Research Internship Programme 2026**.

## Project Scope

The original MedSAM-U framework contains three major components:

1. **MPA-MedSAM**: adapts MedSAM to handle multiple prompt types.
2. **UGMP**: estimates segmentation uncertainty from multiple perturbed prompts.
3. **UGPA**: uses the estimated uncertainty to automatically refine prompts.

This internship implementation focuses on the **UGMP stage** as a practical prototype. It does not claim to reproduce the complete training, MPA-MedSAM, UGPA, or the paper's full benchmark experiments.

## Objective

The objective is to estimate pixel-level segmentation uncertainty by testing MedSAM with multiple slightly perturbed bounding-box prompts.

The implementation follows this pipeline:

```text
Initial Bounding Box
        ↓
Generate 5 Bounding-Box Prompts
        ↓
Run MedSAM for Each Prompt
        ↓
Generate 5 Segmentation Masks
        ↓
Generate Probability Maps
        ↓
Average Probability Maps
        ↓
Calculate Pixel-wise Entropy
        ↓
Generate Uncertainty Map / Overlay
```

## Method

### 1. Multiple Prompt Generation

Starting from an initial bounding box, the notebook generates five boxes. The original box is retained and the remaining boxes are created using small random coordinate perturbations.

### 2. MedSAM Inference

Each bounding box is passed independently to MedSAM. The implementation collects the corresponding segmentation masks and probability maps.

### 3. Probability Aggregation

The probability maps are stacked and their mean is calculated pixel by pixel.

### 4. Entropy-based Uncertainty

For each pixel, uncertainty is estimated using binary entropy:

```text
U = -[p log(p) + (1-p) log(1-p)]
```

where `p` is the mean predicted probability at that pixel.

Higher entropy indicates greater uncertainty, while lower entropy indicates greater confidence.

### 5. Visualization

The resulting entropy map is visualized and overlaid on the original medical image. In the demonstrated result, uncertainty is particularly visible around the target boundary, where predictions can vary more under prompt perturbations.

## Repository Contents

```text
MedSAM-U-UGMP/
│
├── README.md
├── UGMP.ipynb
├── requirements.txt
├── .gitignore
│
├── docs/
│   ├── Final_Internship_Report.pdf
│   └── MedSAM-U_Presentation.pptx
│
└── results/
    └── uncertainty_overlay.png
```

## Notebook

`UGMP.ipynb` contains the complete implementation used for the internship prototype, including:

- MedSAM setup and model loading
- image loading and preprocessing
- bounding-box generation
- five prompt perturbations
- MedSAM inference
- mask collection
- probability-map generation
- mean probability calculation
- entropy calculation
- uncertainty-map visualization
- uncertainty overlay visualization

## Requirements

The notebook was developed for a Google Colab environment with GPU support.

Install the Python dependencies listed in `requirements.txt`, then install/setup MedSAM using the official repository instructions.

### Official MedSAM repository

https://github.com/bowang-lab/MedSAM

The MedSAM checkpoint is **not included** in this repository because it is a large model file. Download the required checkpoint according to the official MedSAM instructions and update the checkpoint path in the notebook for your environment.

## Input Data

The prototype uses the MedSAM demo medical image for demonstrating the UGMP pipeline. A large medical-image dataset is intentionally not included in this repository.

## Running the Notebook

1. Open `UGMP.ipynb` in Google Colab or Jupyter.
2. Set up the MedSAM repository and dependencies.
3. Download the required MedSAM checkpoint.
4. Update the local image and checkpoint paths if necessary.
5. Run the notebook cells sequentially.
6. Inspect the generated masks, probability maps, entropy map, and uncertainty overlay.

## Limitations

This repository is a focused implementation of the UGMP uncertainty-estimation stage. It is not a complete reproduction of the MedSAM-U framework presented in the research paper.

Specifically, this implementation does not include:

- training of MPA-MedSAM
- the complete UGPA prompt-refinement stage
- multi-dataset training
- the complete quantitative benchmark evaluation reported in the paper

## Research Paper Context

The original paper proposes MedSAM-U as an uncertainty-guided framework for reliable medical image segmentation. It combines MPA-MedSAM, UGMP, and UGPA. The paper describes generating multiple perturbed bounding boxes, obtaining multiple segmentation predictions, aggregating the predictions, and estimating uncertainty using entropy before using uncertainty to guide prompt adaptation.

This repository implements the UGMP uncertainty-estimation idea as a standalone prototype for study and experimentation.

## Reference

Zhou, N., Zou, K., Ren, K., Luo, M., He, L., Wang, M., Chen, Y., Zhang, Y., Chen, H., and Fu, H., **MedSAM-U: Uncertainty-Guided Auto Multi-Prompt Adaptation for Reliable MedSAM**, arXiv:2409.00924, 2024.

Paper: https://arxiv.org/abs/2409.00924

## Internship

**IEEE SMC Student Branch Chapter, Kalyani Government Engineering College**  
**Research Internship Programme 2026**

**Intern:** Sudhanshu Kumar
