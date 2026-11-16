# 🌟 PRISM-Net: Parallel ResNet-Integrated-SAM with Multi-attention

PRISM-Net is a specialized, vision-forward deep learning architecture engineered for precision breast ultrasound lesion segmentation. By running a foundational zero-shot model (FastSAM) in parallel with a robust convolutional backbone (ResNet-50), it captures both sweeping semantic context and granular anatomical nuances. Enhanced by edge-aware operators and a multi-attention fusion mechanism, PRISM-Net homes in on critical tumor boundaries while actively silencing background acoustic noise.

---

## 🏛️ The Architecture

> <img width="960" height="540" alt="Architecures" src="https://github.com/user-attachments/assets/b46b4639-0fb9-4e31-9f24-33cf1abed117" />



## 🏆 Performance Benchmarks

Precision is everything in medical imaging. Here is how PRISM-Net holds up against standard benchmark datasets:

| Dataset | Dice Coefficient (DSC) | Intersection over Union (IoU) |
| :--- | :---: | :---: |
| **BUSI** | `85.10 ± 1.28` | `74.06 ± 1.17` |
| **BUSBRA** | `91.31 ± 2.01` | `84.27 ± 1.93` |

*Image-level 80:10:10 split. All scores are in % and reported as mean ± standard deviation over five random seeds.*


## 🧪 Ablation Study (Patient-Level Split)

In a patient-level split, every image from a given patient is kept in a single partition (train, validation, or test), so no patient is seen in both training and testing. The table below repeats the component-wise ablation under this stricter protocol. All scores are in % and reported as mean ± standard deviation over five random seeds.

| Model Variant | R50 | Edge | FSAM | Attn | DS | BUSI Dice | BUSI IoU | BUSBRA Dice | BUSBRA IoU |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| M1: FastSAM-only U-Net | ✗ | ✗ | ✓ | ✗ | ✗ | `72.18 ± 2.64` | `61.64 ± 2.09` | `84.73 ± 1.13` | `74.65 ± 2.03` |
| M2: ResNet50-only U-Net | ✓ | ✗ | ✗ | ✗ | ✗ | `74.57 ± 1.35` | `61.26 ± 2.04` | `84.36 ± 1.34` | `76.36 ± 2.31` |
| M3: ResNet50 + Roberts Edge | ✓ | ✓ | ✗ | ✗ | ✗ | `76.87 ± 1.75` | `64.25 ± 0.98` | `85.90 ± 1.72` | `8.69 ± 1.47` |
| M4: Dual-path (M3 + FSAM) | ✓ | ✓ | ✓ | ✗ | ✗ | `78.52 ± 1.31` | `65.29 ± 1.76` | `85.94 ± 0.82` | `77.93 ± 1.88` |
| M5: Dual-path + Attention | ✓ | ✓ | ✓ | ✓ | ✗ | `80.58 ± 1.85` | `68.55 ± 1.95` | `87.10 ± 1.61` | ` 79.61± 1.83` |
| **M6: PRISM-Net (Full)** | ✓ | ✓ | ✓ | ✓ | ✓ | **`82.63 ± 1.74`** | **`71.06 ± 1.51`** | **`88.31 ± 1.93`** | **`80.43 ± 2.68`** |

*R50: ResNet-50 encoder · Edge: Roberts edge early fusion · FSAM: FastSAM branch · Attn: PCBAM + SGA + Attention Gates · DS: deep supervision. ✓ = present, ✗ = absent. All scores are in % and reported as mean ± standard deviation over five random seeds.*

FIGURE 1: BUSI ERROR ANALYSIS
<img width="971" height="315" alt="BUSIERROR" src="https://github.com/user-attachments/assets/0b58364e-b681-4655-b6f2-28e68afdbe54" />

FIGURE 2: BUSBRA ERROR ANALYSIS
<img width="950" height="315" alt="BUSERROR" src="https://github.com/user-attachments/assets/6542b6ad-a5ea-48c1-a9d2-eb9bddeb4585" />


* ⬜  White: True Positives *(Spot-on tissue identification)*
* 🟥  Red: False Positives *(Background hallucinated as a tumor / Over-segmentation)*
* 🟦  Blue: False Negatives *(Missed tumor regions / Under-segmentation)*

---

## 🧠 Model Interpretability (Grad-CAM)

We don't just want a black box. Using Gradient-weighted Class Activation Mapping (Grad-CAM), we peek into the network's "mind" to ensure it is focusing on true pathological features rather than irrelevant image artifacts.

FIGURE 3: BUSI GRAD-CAM
><img width="950" height="315" alt="BUSIGRADCAM" src="https://github.com/user-attachments/assets/732b7f34-e8a3-41cd-8e8e-aa457ef4ceca" />

FIGURE 4: BUSBRA GRAD-CAM
<img width="950" height="315" alt="BUSGRADCAM" src="https://github.com/user-attachments/assets/8ab2f58a-59c2-40d7-b8fd-5030f5525df5" />




---

## ✍️ Authors

* **Rounak Saha** 
* **Britika Khati**
* **Prof. Ram Sarkar**
