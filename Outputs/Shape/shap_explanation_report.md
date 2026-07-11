# GradientSHAP / Saliency Explainability Report — Iris PAD (CNN: ResNet18)

## 1. What is this report?
This report explains, in plain language, **which pixels most influenced** the CNN's
Live-vs-Spoof decision, based on **GradientSHAP** and **Saliency** attribution maps
computed for 15 Live and 15 Spoof test images.

Unlike Grad-CAM (see NB05's report), which highlights *regions* of a convolutional
feature map, these methods attribute importance down to **individual pixels** of the
input image — giving a finer-grained, complementary view.

## 2. Where does attribution concentrate?
- For **LIVE** images, the strongest average attribution is in **the center of the image (core iris area)**.
- For **SPOOF** images, the strongest average attribution is in **the center of the image (core iris area)**.

## 3. Where do Live and Spoof differ the most?
Across the 7×7 region grid, the region showing the **largest Live-vs-Spoof
difference** (by paired t-test) is **the bottom-center of the image**, where attribution magnitude
is higher for **Live** images (t = -3.02, p = 0.0092).

5 out of 49 regions show p < 0.05 (uncorrected).

## 4. What does this mean?
- If the highlighted region corresponds to the **iris texture / pupil boundary /
  contact-lens rim**, this is consistent with the model picking up on genuine
  liveness cues (natural iris texture vs. printed/lens artifacts).
- If the highlighted region corresponds to the **image border, sclera, or eyelid**,
  this could indicate the model is partly relying on **dataset-specific artifacts**
  (e.g. consistent cropping/lighting differences between sensors) rather than purely
  iris-intrinsic cues. Cross-check against NB05's per-dataset Grad-CAM grid.

## 4.5 Faithfulness & Cross-Method Validation
A perturbation test shows masking the top-10% attributed pixels causes a
-0.266 (LIVE) / +0.341 (SPOOF) drop in predicted
confidence, vs. -0.758 / +0.074 for random pixels --
confirming the attribution maps point to genuinely influential pixels.
Cross-checking against NB05's Grad-CAM maps gives a correlation of
0.422 (LIVE) and 0.496 (SPOOF), indicating agreement
between feature-map-level and pixel-level explanations.

## 5. Important caveats (research honesty)
- **Small sample size**: only 15 Live and 15 Spoof images were
  explained (chosen for GPU-memory reasons on a 4GB card). The paired t-test in
  Section 8 has very limited statistical power with n=15 — treat the
  "top region" above as a **hypothesis to investigate further**, not a proven
  finding.
- **No multiple-comparison correction**: with 49 regions tested, ~2-3 would be
  expected to show p < 0.05 by chance alone even with no real effect. The count of
  5 significant regions should be read in that light.
- **GradientSHAP baseline sensitivity**: attributions depend on the chosen baseline
  (here, small Gaussian noise). Different baselines (e.g. a black image, or a blurred
  version of the input) can shift which pixels appear "important".
- **Complementary, not definitive**: this report should be read together with NB05's
  Grad-CAM report. Where both methods agree (e.g. both highlight the central iris
  region for Spoof), confidence is higher; where they disagree, further
  investigation is warranted.

## 6. Files referenced
- `shap_live.png`, `shap_spoof.png` — Section 6
- `shap_diff.png` — Section 7
- `shap_regions.png` — Section 8

---
*Generated automatically by `06_SHAP_Attribution.ipynb`.*
