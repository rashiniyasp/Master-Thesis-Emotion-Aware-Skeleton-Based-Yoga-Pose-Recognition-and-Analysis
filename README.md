# Emotion Aware Skeleton-Based Yoga Pose Recognition and Analysis

## 📌 Overview
This repository contains the methodology, pipeline architectures, and conceptual overview of the holistic yoga assistance system developed for emotion-aware skeleton-based yoga pose recognition and analysis. 

By replacing raw video feeds with privacy-preserving skeleton keypoints, this project constructs a complete pipeline that not only identifies the yoga pose but also analyses the user's emotional state and provides interpretable, joint-level corrective feedback.

The system is built upon three core pillars:
1. **Facial Emotion Recognition (FER)**
2. **Skeleton-Based Pose Recognition** 
3. **Explainable Pose Correction**

---

## 🏗️ System Pipeline

The end-to-end pipeline operates in three stages:
1. **Feature Extraction**: Captures facial landmarks and 33-joint full-body skeletal topologies (e.g., using MediaPipe) to ensure user privacy by avoiding raw RGB transmission.
2. **Recognition Modules**: Simultaneously classifies the practitioner's emotion and the specific yoga pose being performed.
3. **Correction & Guidance**: If the pose is identified as incorrect or misaligned, the correction module leverages the classifier's attention mechanisms and nearest-neighbor exemplars to generate biomechanically plausible, joint-level adjustments.



---

## 🧠 1. Facial Emotion Recognition (FER)
The FER module aims to assess the practitioner's psychological state to provide context-aware yoga recommendations. The project evaluates two distinct paradigms:
*   **Pixel-Based Models (DenseNet-201, EfficientNet-B4)**: Achieve superior accuracy (up to 81.74%) by capturing transient texture cues like skin wrinkling and shading.
*   **Geometry-Based Models (MediaPipe + XGBoost)**: Offer a lightweight, privacy-preserving alternative that operates purely on facial landmark coordinates.

**Selection:** The final pipeline prioritizes DenseNet-201 for maximum fidelity, while retaining the geometry-based approach as a fallback for strict privacy settings.

---

## 🧘‍♂️ 2. Skeleton-Based Pose Recognition
To handle the complex spatial and temporal dynamics of yoga postures, the project introduces two novel architectural frameworks:

### Yoga-MAtNODE (Multi-View Attention Neural ODE)
A continuous-time modeling approach designed to capture smooth skeletal dynamics.
*   **Multi-View Generation**: Rotates the canonical skeleton to generate multiple synthetic viewpoints, ensuring rotational invariance.
*   **Neural ODE Encoding**: Models the temporal evolution of the skeletal sequence as a continuous differential equation, effectively handling variable frame rates.
*   **Attention Aggregation**: Fuses the multi-view latent representations to suppress noise and highlight informative perspectives.

> [!NOTE]
> For a detailed analysis and the architecture diagram of this methodology, please check the [Yoga-MAtNODE Repository](https://github.com/rashiniyasp/Yoga-MAtNODE).
> *(Accepted at ICPR 2026)*

### DUAL-Pose (Dual-Branch Graph Networks)
A parallel-branch architecture for static or semi-static pose classification.
*   **Local Branch**: Uses Graph Convolutional Networks (GCNs) to capture topological relationships between connected joints.
*   **Global Branch**: Extracts holistic kinematic features such as specific joint angles and limb orientations.
*   **Fusion**: Concatenates both streams to resolve fine-grained geometric ambiguities that single-branch models miss.

> [!NOTE]
> For a detailed analysis and the architecture diagram of this methodology, please check the [DUAL-Pose Repository](https://github.com/rashiniyasp/Dual_Pose).
> *(Accepted at CVPRW 2026)*

---

## 🔧 3. Explainable Pose Correction
Pose recognition alone cannot ensure safe practice. The system provides actionable, joint-level feedback for incorrect postures using a training-free optimization loop.

### ACORN Framework
**A**ttention-**C**onstrained **O**ptimization for **R**efinement of Incorrect Poses via **N**earest-Exemplar Guidance.
*   **JCAT (Joint Cross-Attention Transformer)**: A lightweight classifier that provides the corrective gradient signal and computes per-joint importance weights (attention).
*   **Exemplar Guidance**: Finds the closest correctly-performed pose in the dataset to act as a geometrical anchor.
*   **Multi-Objective Optimization**: Iteratively adjusts the user's incorrect skeleton to satisfy target-class recognition, while enforcing strict biomechanical constraints (bone-lengths and joint-angles) so the suggested correction is physically possible.
*   **Segment Refinement**: Isolates the most critical joints (e.g., just the arms or hips) to provide sparse, interpretable feedback rather than overwhelming the user with full-body adjustments.

---

## 🛡️ Privacy and Ethics
A core tenant of this framework is the elimination of RGB video storage. By immediately mapping raw camera inputs to skeletal graphs and facial landmarks at the edge, the system inherently protects user identity and environment context, making it suitable for unsupervised home deployment.
