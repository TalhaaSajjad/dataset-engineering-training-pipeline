<h1 align="center">Dataset Engineering &amp; Model Training Pipeline</h1>

<p align="center">
  <i>An end-to-end workflow for turning raw, unstructured image/video data into clean, well-labelled, augmented datasets — and training production-ready computer-vision detection models on top of them.</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white"/>
  <img src="https://img.shields.io/badge/YOLO_/_YOLOX-00FFFF?style=flat-square&logo=yolo&logoColor=black"/>
  <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=opencv&logoColor=white"/>
  <img src="https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white"/>
  <img src="https://img.shields.io/badge/CVAT-0095FF?style=flat-square&logoColor=white"/>
</p>

---

## Overview

Model quality is decided long before training starts — in how the data is collected, sorted, cleaned, labelled and augmented. This project documents the dataset-engineering and training pipeline I use to take raw footage and scattered image dumps and turn them into balanced, high-quality datasets that train reliable object-detection models (PPE compliance, smoking, violence, and weapon/knife detection, among others).

The pipeline is organised into five stages, each with a clear input, a clear output, and quality gates in between.

---

## Pipeline

### 1 · Data Sorting &amp; Organisation
- Ingest raw images and extracted video frames from multiple sources into a consistent directory and naming convention.
- Sort by class, scene, camera source, and split (`train` / `val` / `test`), keeping splits disjoint to prevent leakage.
- De-duplicate near-identical frames (e.g. consecutive CCTV frames) so the dataset stays diverse rather than redundant.
- Track dataset composition and per-class counts to catch imbalance early.

### 2 · Data Cleaning
- Remove corrupt, blurred, truncated, or mislabelled samples.
- Normalise image formats, resolutions, and colour spaces.
- Validate every annotation: correct class IDs, in-bounds boxes, no empty or degenerate labels, consistent label/image pairing.
- Reconcile labelling done in **CVAT** with the training format and fix inconsistencies before they reach the model.

### 3 · Data Augmentation
- Expand and balance the dataset with geometric transforms (flips, rotations, scaling, crops) and photometric transforms (brightness, contrast, hue, noise, blur).
- Apply augmentations that reflect real deployment conditions — low light, motion blur, varied camera angles — so the model generalises to live CCTV rather than just the training set.
- Over-sample under-represented classes to reduce imbalance while preserving label integrity through every transform.

### 4 · Model Training
- Train object-detection models (**YOLO / YOLOX** family) on the prepared datasets.
- Configure anchors, input resolution, batch size, and learning-rate schedules for the target task and hardware.
- Use transfer learning from pre-trained weights, then fine-tune on the domain-specific dataset.
- Monitor training with per-class metrics and guard against over-fitting.

### 5 · Evaluation &amp; Iteration
- Evaluate on a held-out test split using precision, recall, and mAP, broken down per class.
- Analyse failure cases, feed them back into sorting/cleaning/augmentation, and retrain — a continuous loop that steadily raises dataset and model quality.

---

## Tech Stack

| Area | Tools |
|------|-------|
| Language | Python |
| Training | PyTorch, YOLO / YOLOX |
| Image &amp; video processing | OpenCV, NumPy, Pillow |
| Annotation | CVAT |
| Workflow | Dataset splitting, de-duplication, augmentation, evaluation |

---

## About

This repository documents the methodology and engineering practices behind the datasets and detection models in my computer-vision work. It focuses on the parts of the ML lifecycle that most determine real-world performance: disciplined data sorting, rigorous cleaning, realistic augmentation, and careful training and evaluation.

<sub>Maintained by <a href="https://github.com/TalhaaSajjad">TalhaaSajjad</a> · More work at <a href="https://talhaasajjad.github.io/portfolio/">talhaasajjad.github.io/portfolio</a></sub>
