# SynLVSD: Synthetic Liver Vessel Segmentation Dataset Generation

## Overview
This repository contains the code, reference datasets, and supporting files for **SynLVSD**, a physics-based and knowledge-driven simulation pipeline for generating synthetic liver CT images with realistic vascular structures and paired vessel segmentation labels.

The project was developed to address the limitations of existing public datasets for liver vessel segmentation (e.g., MSD, LiVS, 3D-IRCADb), which often suffer from annotation incompleteness, sparse coverage, or insufficient case numbers. By simulating realistic vascular trees, digital phantoms, and CT imaging processes, SynLVSD provides high-quality, fully annotated, and customizable training data for liver vessel segmentation models.

Our results (see manuscript) demonstrated that models trained on SynLVSD achieved superior performance compared to models trained on MSD and LiVS, particularly for smaller-caliber vessels. This underscores the dataset’s value in improving anatomical completeness and clinical applicability.

---

## Key Features
- **Vessel tree generation**: Anatomically realistic vascular trees simulated with controllable branching complexity.  
- **Digital phantom construction**: Voxel intensities assigned using distance-based intensity decay with partial volume effects to reflect boundary uncertainty.  
- **CT simulation**: Multi-slice helical CT geometry with Poisson + Gaussian noise and filtered back-projection reconstruction to mimic clinical imaging artifacts.  
- **Customizability**: Users can configure vessel branching, voxel resolution, HU assignment, contrast levels, motion blur, and acquisition parameters.  
- **Scalability**: Unlimited dataset generation with full control over anatomy and imaging conditions.  

---

## Repository Structure
```
SynLVSD/
├─ README.md
├─ code/
│ ├─ simulate_liver_vessels_CT.m # main driver (edit base paths here)
│ ├─ IRT_TIGRE_iFanbeamHelical.m # helical projection + rebin + FBP
│ ├─ IRT_TIGRE_iFanbeamHelical_mask.m # same for masks
│ ├─ tree_to_3D_vol_faster_3.m # distance-based vessel volume + radius map
│ ├─ InsertLiver2Surroundings.m # embed liver into torso background
│ ├─ crop_organ_by_body_mask.m # utility
│ └─ (other required helpers)
├─ data/
│ ├─ Reference_Organs/ # 40 preregistered Couinaud livers
│ ├─ BloodDemandMaps/ # Phantomcustom*.dat (10-phase)
│ └─ Intensity_Values/
│ └─ <case_name>/
│ └─ intensity.mat # Background_e_Mean, Vessel_e_Mean
├─ outputs/
│ ├─ image/ # simulated CT (MAT)
│ ├─ vessel_mask/ # vessel masks (MAT)
│ └─ liver/ # liver masks (MAT)
└─ examples/
├─ generate_single_case.m
└─ generate_batch_40_cases.m
```


> **SynLVSD Pipeline:** <img width="975" height="299" alt="image" src="https://github.com/user-attachments/assets/feebdfb6-c3a4-496b-935b-c259d9d5d9ac" />


---

---

## Installation & Requirements
- **MATLAB R2022b or later**  
- Toolboxes: Image Processing Toolbox, Statistics & Machine Learning Toolbox  
- External dependencies:  
  - [TIGRE toolbox](https://github.com/CERN/TIGRE) – GPU-accelerated CT projection & reconstruction  
  - [IRT toolbox](https://web.eecs.umich.edu/~fessler/irt/fessler.tgz) – Helical rebinning  

---

## Usage

### Step 1: Configure parameters
You can edit or pass parameters when calling `simulate_liver_vessels_CT.m`:

```matlab
params = struct( ...
    'num_endpoints', 1000, ...
    'voxel_res', 0.5, ...
    'output_res', 0.3, ...
    'include_motion_blur', true ...
);

simulate_liver_vessels_CT(5, params);   % Generate 5 vascular trees
```

### Step 2: Outputs

The pipeline will generate three files per simulation:

*   **CT.mat** – synthetic liver CT volume
*   **vessel_mask.mat** – binary vessel segmentation mask
*   **liver.mat** – binary liver mask

---

## Results

*   Models trained on SynLVSD outperformed those trained on MSD and LiVS in terms of **Dice score** and **vessel continuity**.  
*   Size-stratified analysis showed **significant improvements in small vessel segmentation**, demonstrating anatomical completeness.  
*   Synthetic data generalized effectively to **real clinical CT scans**, capturing vessels missed by models trained on under-labeled datasets.  

---

## 
