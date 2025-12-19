# Self-Supervised Learning on STL-10: SimCLR vs BYOL

This repository contains an end-to-end implementation and evaluation of **self-supervised learning (SSL)** methods — **SimCLR** and **BYOL** — on the **STL-10** dataset. The work focuses on representation learning using unlabeled data and evaluates learned embeddings using **linear probing** and **k-NN classification**.

---

## 📌 Project Motivation

In many real-world and research settings, labeled data is scarce or expensive to obtain. Self-supervised learning addresses this limitation by learning meaningful representations from **unlabeled data** using carefully designed pretext tasks.

This project was carried out as a **research-oriented implementation**, with the goal of:

* Understanding modern SSL algorithms in practice
* Identifying practical training constraints
* Quantitatively comparing representation quality

---

## 🧠 Methods Implemented

### 1. SimCLR (Contrastive Learning)

* Learns representations by **maximizing agreement** between two augmented views of the same image
* Uses **NT-Xent contrastive loss**
* Requires **large batch sizes** for effective negative sampling

### 2. BYOL (Bootstrap Your Own Latent)

* Avoids negative samples entirely
* Uses an **online network** and a **target network** updated via EMA
* Demonstrates better stability and performance under smaller batch sizes

> Both methods use a **ResNet-18** backbone trained on the **unlabeled split of STL-10**.

---

## 📊 Evaluation Protocols

To assess the quality of learned representations, two standard SSL evaluation strategies were used:

### 🔹 Linear Probing

* Encoder weights frozen
* A linear classifier trained on labeled STL-10 training set
* Measures **linearly separable semantic information**

### 🔹 k-NN Evaluation

* No training involved
* Classification via cosine similarity in embedding space
* k = 20

---

## 🧪 Results

| Method     | Linear Probe Accuracy | k-NN Accuracy (k=20) |
| ---------- | --------------------- | -------------------- |
| **SimCLR** | 63.06%                | 58.66%               |
| **BYOL**   | **70.14%**            | **68.66%**           |

**Observation:** BYOL consistently outperformed SimCLR, particularly under constrained batch sizes and limited training epochs.

---

## ⚙️ Training Details

* Dataset: STL-10 (unlabeled split for SSL)
* Image size: 96×96
* Backbone: ResNet-18
* Optimizer: Adam / SGD (experiment dependent)
* Augmentations: Random crop, color jitter, grayscale, Gaussian blur
* Hardware: Kaggle Free Tier / Local GPU

---

## 🚧 DINO-ViT: Attempted but Not Completed

An attempt was made to implement **DINO with Vision Transformers (ViT)**. However, training was **not feasible** due to:

* High memory requirements from multi-crop strategy
* ViT input resolution constraints (224×224 vs STL-10 96×96)
* Extremely slow convergence on Kaggle free tier GPUs
* Gradient checkpointing instability

This limitation is **hardware-related**, not algorithmic, and is documented transparently as part of the research process.

---

## 📁 Repository Structure

```
.
├── README.md
├── SSl-Benchmark comparitive study.pdf
├── configs
│   ├── byol.yaml
│   └── simclr.yaml
└── notebooks
    ├── byol-model.ipynb
    └── simclr.ipynb
```

---

## 📌 Key Takeaways

* BYOL is more robust than SimCLR under limited batch sizes
* Contrastive methods are sensitive to hardware constraints
* Linear probing and k-NN give complementary views of representation quality

---

## 📖 References

* Chen et al., *A Simple Framework for Contrastive Learning of Visual Representations* (SimCLR)
* Grill et al., *Bootstrap Your Own Latent* (BYOL)
* STL-10 Dataset

---

## 👤 Author: Siddharth Maurya

This project was implemented as a **research-focused exercise** to demonstrate practical understanding of modern self-supervised learning methods and their limitations under real-world constraints.

---

If you are reviewing this repository as part of a **research internship or academic evaluation**, the accompanying report provides a detailed methodological and analytical discussion.
