# Topological-Data-Analysis-and-CNN-Fusion-for-Explainable-Iris-Presentation-Attack-Detection
An explainable iris presentation attack detection system that fuses CNN deep features with persistent-homology-based topological features (Grad-CAM, SHAP, and TDA explainability), evaluated across NDCLD15, IIITD-CLI, and PrintScan datasets.
# Explainable Iris PAD using CNN + TDA Fusion

This is my internship/project work on iris presentation attack detection (PAD).
The idea is simple: a CNN learns to tell live iris images apart from spoof
ones (printed photos, colored contact lenses, etc.), and I'm also adding
topological data analysis (TDA) on top of the CNN to see if it adds anything
useful, along with some basic explainability (GradCAM, SHAP) so the model's
decisions aren't a complete black box.

## Why this project

Most iris liveness detection work just uses a CNN and stops there. I wanted
to try combining CNN features with topological features (from persistent
homology) and see if fusing the two actually helps, especially when training
on one sensor and testing on a different one - which is usually where models
fall apart.

## Datasets used

- **NDCLD15** - contact lens dataset, live vs colored/cosmetic lens
- **IIITD-CLI** - contact lens dataset with two sensors (Cogent and Vista),
  used here mainly for the cross-sensor generalization experiment
- **PrintScan** (Cogent + Vista) - printed iris photo attacks

All raw data lives outside this repo (too large to push to GitHub) - the
notebooks expect it under `Data/Raw/` following the folder structure
described inside `01_Data_Exploration.ipynb`.

## Project structure

```
notebooks/
    01_Data_Exploration.ipynb        - scans all datasets, builds master_index.csv
    02_Preprocessing.ipynb           - resize, split, normalization stats
    03_Baseline_CNN.ipynb            - ResNet18 + PBS baseline, APCER/BPCER/ACER
    04_Model_Comparison.ipynb        - ResNet18 vs MobileNetV2 vs EfficientNetB0
    05_GradCAM_XAI.ipynb             - GradCAM heatmaps
    06_SHAP_Analysis.ipynb           - Captum-based attribution maps
    07_PointCloud_Generation.ipynb   - image -> point cloud for TDA
    08_Persistent_Homology.ipynb     - Vietoris-Rips persistence diagrams
    09_TDA_Feature_Extraction.ipynb  - feature selection from raw TDA descriptors
    10_CNN_TDA_Fusion.ipynb          - attention-gated CNN+TDA fusion model
    15_Robustness_Tests.ipynb        - noise/blur/compression stress tests

Experiments/
    Exp5_IIITD_CrossSensor/   - train on Cogent test on Vista (and reverse),
                                 multiple CNN backbones, TDA fusion on top
    Exp6_NDCLD15_MultiBackbone/ - same idea but on NDCLD15

Data/
    splits/      - train.csv, val.csv, test.csv, master_index.csv etc.
    Processed/   - resized images (not pushed to git, regenerate locally)

models/    - saved .pth checkpoints (not pushed to git, see .gitignore)
outputs/   - generated plots, metrics CSVs, reports
```

## How to run

1. Create the conda environment and install requirements:
   ```
   conda create -n iris_xai python=3.11
   conda activate iris_xai
   pip install -r requirements.txt
   ```

2. Put the raw datasets under `Data/Raw/` (see paths used in
   `01_Data_Exploration.ipynb`).

3. Run the notebooks in order: 01 -> 02 -> 03 -> ... They each save their
   output (CSVs, models, plots) so later notebooks don't need to redo
   earlier work.

4. For the cross-sensor TDA fusion experiment specifically, go into
   `Experiments/Exp5_IIITD_CrossSensor/` and run Notebook A, then B, then C
   in that folder.


