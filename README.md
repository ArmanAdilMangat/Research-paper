# Optimized Deep Learning Intrusion Detection for IoT Network Security

> A benchmark study of **CNN, RNN (LSTM), BERT, and Random Forest** for intrusion detection on the **CICIDS-2017** dataset — plus a **CNN-LSTM hybrid** exploration. Built for IoT environments where attacks like DDoS, infiltration, and botnets exploit underpowered, unencrypted devices.

[![Paper](https://img.shields.io/badge/Paper-PDF-red)](paper/Optimized_Deep_Learning_Intrusion_Detection_for_Enhanced_IoT_Network_Security.pdf)
[![Slides](https://img.shields.io/badge/Slides-PPTX-orange)](presentation/IDS_slides.pptx)
[![Dataset](https://img.shields.io/badge/Dataset-CICIDS--2017-blue)](https://www.unb.ca/cic/datasets/ids-2017.html)
[![Best Model](https://img.shields.io/badge/Best%20Model-BERT%20%E2%80%94%2099.6%25-brightgreen)]()
[![License](https://img.shields.io/badge/License-Academic-lightgrey)]()

---

## 📄 Abstract

This work benchmarks deep learning architectures against a classical baseline for **intrusion detection in IoT networks**. We fine-tune and evaluate **Convolutional Neural Networks (CNNs)**, **LSTM-based Recurrent Neural Networks**, and a **Transformer (BERT)** model, comparing them against a **Random Forest** classifier on the CICIDS-2017 dataset. All deep models clear **>98% accuracy**, with **BERT leading at 99.6%** thanks to self-attention's ability to model global feature dependencies — particularly useful for rare and subtle attack patterns. Each model exhibits distinct strengths: CNNs excel at volumetric attacks, LSTM at sequential exfiltration, and Random Forest delivers interpretable, overfitting-resistant baselines. A bonus **CNN-LSTM hybrid** architecture achieves **99.55% accuracy** on 4-class classification, demonstrating the value of combining spatial and temporal feature extraction.

---

## 📊 Results

### Published results (binary classification — benign vs. malicious)

| Model | Accuracy | Precision | Recall | F1-Score |
|---|---:|---:|---:|---:|
| **BERT** 🏆 | **99.6%** | 99.5% | 99.7% | **99.6%** |
| **CNN** | 99.2% | 99.0% | 99.4% | 99.2% |
| **RNN (LSTM)** | 98.9% | 98.7% | 99.1% | 98.9% |
| **Random Forest** | 97.8% | 97.5% | 98.0% | 97.7% |

### Bonus: CNN-LSTM hybrid (4-class)

A hybrid `Conv1D → MaxPooling → LSTM → Dense` architecture trained for multi-class classification:

| Metric | Value |
|---|---:|
| Accuracy | **99.55%** |
| Macro F1 | 1.00 |
| Test samples | 208,512 |
| Classes | 4 attack categories |

### What each model is best at

| Architecture | Strength | Best for |
|---|---|---|
| **BERT** | Self-attention captures global dependencies | Rare, subtle attack signatures · unbalanced data |
| **CNN** | Learns spatial hierarchies from flow features | Volumetric attacks (DDoS) |
| **LSTM** | Retains long-term temporal dependencies | Sequential / time-based attacks · data exfiltration |
| **Random Forest** | Interpretable, overfit-resistant ensemble | Baseline comparison · feature importance |
| **CNN-LSTM hybrid** | Combines spatial + temporal extraction | Multi-class scenarios |

> **Key finding:** no single architecture dominates across all attack types. Model choice should match the threat profile.

---

## 🛠️ Methodology

### Dataset preprocessing pipeline

The **CICIDS-2017** dataset covers normal traffic plus DDoS, infiltration, botnet, and modern attack vectors. Preprocessing:

1. **Data cleaning** — drop irrelevant, missing, inconsistent records
2. **Feature engineering** — statistical + domain-driven attribute extraction
3. **Normalization** — `StandardScaler` for feature scaling
4. **Correlation analysis** — remove redundant features to cut dimensionality
5. **Label encoding** — `LabelEncoder` for class targets
6. **Train/test split** — 80/20 stratified, `random_state=42`

### Model architectures

| Model | Framework | Key layers |
|---|---|---|
| **BERT** (fine-tuned) | HuggingFace Transformers + TensorFlow | Pre-trained embeddings + attention + classification head |
| **CNN** | TensorFlow / Keras | `Conv2D → MaxPooling2D → Flatten → Dense → Dropout` |
| **LSTM (RNN)** | TensorFlow / Keras + gensim Word2Vec | Embedding → LSTM → Dense |
| **Random Forest** | scikit-learn | Ensemble of decision trees + `SimpleImputer` |
| **CNN-LSTM hybrid** | TensorFlow / Keras | `Conv1D(64) → MaxPool → Dropout(0.3) → LSTM(50) → Dense(50) → Softmax` |

---

## 🖥️ Experimental setup

| Component | Spec |
|---|---|
| GPU | NVIDIA RTX 4060 Laptop (140 W TGP) |
| CPU | Intel Core i7-13650HX |
| RAM | 16 GB DDR5 @ 5200 MHz |
| Storage | NVMe SSD |
| Machine | ASUS ROG Strix Scar 16 |
| Environments | Isolated Anaconda envs per model |

All training accelerated via CUDA: TensorFlow CNN/hybrid, PyTorch LSTM, HuggingFace + PyTorch BERT.

---

## 📁 Repository structure

```
.
├── README.md                              ← you are here
├── paper/
│   └── Optimized_Deep_Learning_...pdf     ← full research paper
├── notebooks/
│   ├── CNN.ipynb                          ← Conv2D + MaxPool benchmark
│   ├── RNN_LSTM.ipynb                     ← LSTM with Word2Vec embeddings
│   ├── BERT_Transformer.ipynb             ← BERT fine-tuning pipeline
│   ├── Random_Forest.ipynb                ← classical ML baseline
│   └── CNN_LSTM_Hybrid.ipynb              ← hybrid architecture (bonus)
├── documentation/
│   └── project_documentation.docx         ← full project write-up
├── presentation/
│   └── IDS_slides.pptx                    ← project presentation deck
└── references/                            ← supporting IEEE / IoT papers
```

> **Note on dataset:** The CICIDS-2017 CSV files are not included in this repo due to size constraints. Download from the official source: [unb.ca/cic/datasets/ids-2017.html](https://www.unb.ca/cic/datasets/ids-2017.html). Each notebook expects `dataset.csv` in the working directory.

---

## ▶️ How to reproduce

```bash
# 1. Clone the repo
git clone https://github.com/ArmanAdilMangat/<repo-name>.git
cd <repo-name>

# 2. Create environment (Anaconda recommended)
conda create -n ids python=3.10
conda activate ids

# 3. Install dependencies
pip install tensorflow scikit-learn pandas numpy matplotlib seaborn
pip install transformers gensim nltk torch
pip install jupyter

# 4. Download CICIDS-2017 → place dataset.csv in the notebook folder

# 5. Launch Jupyter and run any notebook
jupyter notebook notebooks/
```

---

## 🔭 Future work

- **Expand the dataset** with emerging real-world IoT attack patterns
- **Federated learning** for decentralized, privacy-preserving IDS training
- **Lightweight IDS variants** for edge / resource-constrained devices (ESP32-class hardware)
- **Explainable AI (XAI)** to make detection decisions interpretable for network admins
- Real-time deployment via **FPGA acceleration** for sub-millisecond inference

---

## 📑 Citation

If this work helps your research, please cite:

```bibtex
@misc{mangat2025optimizediot,
  author       = {Arman Adil Mangat},
  title        = {Optimized Deep Learning Intrusion Detection for Enhanced IoT Network Security},
  year         = {2025},
  institution  = {University of Management and Technology (UMT), Lahore},
  howpublished = {\url{https://github.com/ArmanAdilMangat}}
}
```

---

## 👤 Author

**Arman Adil Mangat**  
BS Artificial Intelligence · University of Management and Technology (UMT), Lahore

📧 [aadilmangat@gmail.com](mailto:aadilmangat@gmail.com) · 🔗 [GitHub](https://github.com/ArmanAdilMangat) · [LinkedIn](https://linkedin.com/in/arman-adil-mangat-82221a253)

---

<div align="center">

*Built in Lahore, Pakistan* 🇵🇰

</div>
