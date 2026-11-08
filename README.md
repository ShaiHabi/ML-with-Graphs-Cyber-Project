# **When Zero-Day Becomes Training Data: Learning-Curve Stability under Out-of-Distribution and Domain Adaptation on the MalNet-Tiny datasets**
**By: Shai Habi, Itamar Link and Tomer Gal Netser**

## Introduction

This repository contains the complete experimental pipeline, models, evaluation protocols, and final report for our “ML with Graphs” course project.

We investigate how reliably GCN, GIN, GAT, and Graph Transformer (GT) classify Android Function Call Graphs (FCGs) as benign or malicious across MalNet-Tiny (referred to as "Original"), MalNet-Common, and MalNet-Distinct. We evaluate detection of unseen malware families and examine how performance changes as labeled examples from a new family are added to training.

Two approaches are studied: Out-of-Distribution (OOD) and Domain Adaptation.

### 1. Research Content

We ask how many labeled examples from an unseen malware family are needed to detect its held-out samples, and when macro-F1 learning curves stabilize. We conduct three analyses:

1. **Dataset Analysis:** Evaluate models trained on Original on Common and Distinct before exposure to the new families (e.g., before "domain adaptation").
2. **Out-of-Distribution and Domain Adaptation Learning Analysis:** Add increasing numbers of labeled examples from a new family while training alongside known malware.
3. **Per-Family Analysis:** Train on benign graphs and increasing numbers of examples from a single new malware family.

### 2. Datasets and References:
### Dataset Overview

We use three datasets with 5,000 graphs each and 1,000 samples per class. Original and Common contain the same five classes, while Distinct contains five unseen malware families and no benign graphs.


| Dataset | Family | Avg. Nodes | Avg. Edges |
|---------|--------|-----------:|-----------:|
| Original | Addisplay | 1,631.22 | 2,920.03 |
| Original | Adware | 2,506.74 | 5,597.27 |
| Original | Benign | 2,064.03 | 3,886.76 |
| Original | Downloader | 49.38 | 55.31 |
| Original | Trojan | 799.87 | 1,840.32 |
| Common | Addisplay | 2,438.15 | 4,836.79 |
| Common | Adware | 4,159.36 | 7,990.85 |
| Common | Benign | 1,997.20 | 3,729.22 |
| Common | Downloader | 683.17 | 1,882.70 |
| Common | Trojan | 1,863.02 | 4,247.45 |
| Distinct | Clicker++Trojan | 3,806.42 | 9,109.92 |
| Distinct | Malware | 1,185.28 | 2,491.26 |
| Distinct | Riskware | 2,852.60 | 6,688.56 |
| Distinct | SPR | 4,439.16 | 6,376.55 |
| Distinct | Spyware | 1,427.86 | 2,784.22 |


### References

1. Scott Freitas, Yuxiao Dong, Joshua Neil, and Duen Horng Chau. 2021. A Large-Scale Database for Graph Representation Learning. https://arxiv.org/abs/2011.07682
   
2.  Ngoc N. Tran, Anwar Said, Waseem Abbas, Tyler Derr, and Xenofon D. Koutsoukos. 2026. Quantifying the Generalization Gap: A New Benchmark for Out-of-Distribution Graph-Based Android Malware Classification. https://arxiv.org/abs/2508.06734
   



## Main Results

We compared GCN, GIN, GAT, and Graph Transformer (GT) for malware detection under distribution shift and adaptation to unseen families.
SPR is included in the dataset statistics but excluded from family-level comparisons due to extensive structural duplication.

Two main results are:

1. Models trained on Original transferred poorly to Common and Distinct. In the Per-Family setting, training on benign graphs and just 25 labeled examples from a new malware family yielded 84–97% mean held-out family recall across models, averaged over the four retained families.

2. When learning alongside known malware, Graph Transformer achieved the highest mean normalized area under the new-family recall learning curve: 0.80, compared with 0.47–0.54 for GCN, GIN, and GAT. Overall macro-F1 changed little with sample count, masking changes in new-family detection.



## Quickstart

### 1. Setup Virtual Environment

Requires Python 3.10+.

```bash
# Clone the repository
git clone <repo-url>
cd ML-with-Graphs-Cyber-Project

# Create and activate virtual environment
python3 -m venv .venv
source .venv/bin/activate       # macOS / Linux
# .venv\Scripts\activate        # Windows
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Download Dataset (One-Time)

Downloads and preprocesses MalNet-Tiny, MalNet-Common, and MalNet-Distinct. 
Raw and processed files are stored locally in the datasets/ directory (~2.5 GB).

```bash
python scripts/datasets_loading_preprocessing.py
```

This step only needs to be run once.

