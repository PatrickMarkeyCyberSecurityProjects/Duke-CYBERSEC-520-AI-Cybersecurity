# Duke CYBERSEC 520 — AI/ML for Cybersecurity

Machine-learning coursework and portfolio project from **Duke University's CYBERSEC 520** course, focused on applying supervised learning to network-security data.

## Project Overview

This project builds a binary network-traffic classifier that distinguishes **BENIGN** traffic from **ATTACK** traffic using CICIDS flow data. The notebook walks through the full ML workflow: data loading, cleaning, labeling, balanced sampling, feature preparation, model training, evaluation, and Random Forest hyperparameter experiments.

The goal was not simply to maximize accuracy, but to evaluate the model using the **F1 score for the ATTACK class**, which is more meaningful in a security context because missed attacks matter.

## What I Built

- Loaded and combined benign Monday traffic with a web-attack dataset.
- Cleaned missing and infinite values from the network-flow data.
- Converted multiclass labels into a binary **BENIGN / ATTACK** target.
- Created a balanced working sample of **2,000 flows**: 1,000 BENIGN and 1,000 ATTACK.
- Used **78 numerical network-flow features** for modeling.
- Created a fixed stratified **80/20 train-test split**.
- Trained a **Random Forest** baseline model.
- Tested five additional Random Forest hyperparameter configurations.
- Compared every run using **accuracy** and **ATTACK-class F1**.

## Results

| Model | Key settings | Accuracy | ATTACK F1 |
|---|---|---:|---:|
| Baseline | Random Forest defaults, `random_state=42` | **0.9925** | **0.9925** |
| Configuration 1 | 50 trees, `log2`, depth 15, 50% bootstrap sample | 0.9900 | 0.9899 |
| Configuration 2 | 90 trees, `sqrt`, depth 30, larger leaf/split constraints | 0.9900 | 0.9899 |
| Configuration 3 | 30 trees, no bootstrap, `log2`, depth 15 | 0.9900 | 0.9900 |
| Configuration 4 | 5,000 trees, `log2`, depth 30, larger leaf/split constraints | 0.9850 | 0.9848 |
| Configuration 5 | 300 trees | **0.9925** | **0.9925** |

The default Random Forest baseline and the 300-tree configuration produced the same highest observed ATTACK F1 score of **0.9925** on the fixed test set. The experiments also showed that increasing model complexity did not automatically improve performance.

## Cybersecurity / SOC Relevance

In a SOC, a model like this can be viewed as a first-pass classifier for network flows. Each flow contains measurable characteristics such as duration, packet counts, packet lengths, timing behavior, and TCP-related statistics. A supervised model can learn patterns in those features and help separate normal activity from suspicious traffic.

The project reinforced several security-analytics ideas:

- **False negatives matter:** a high overall accuracy score can still hide missed attacks.
- **F1 is useful for detection tasks:** it balances precision and recall for the ATTACK class.
- **Class imbalance must be handled carefully:** unrestricted sampling can remove rare attack examples.
- **Data leakage must be avoided:** the original `Label` and derived `BinaryLabel` columns are removed from the feature matrix.
- **More complexity is not always better:** the 5,000-tree experiment performed worse than the baseline on this test split.

## Repository Structure

```text
Duke-CYBERSEC-520-AI-Cybersecurity/
├── README.md
├── CYBERSEC_520_Assignment_1_ML_Basics.ipynb
├── requirements.txt
├── .gitignore
└── data/
    └── README.md
```

## Running the Notebook

1. Clone the repository.
2. Create a Python environment and install the dependencies:

```bash
pip install -r requirements.txt
```

3. Place the required CICIDS CSV files in `data/`:
   - `Monday-WorkingHours.pcap_ISCX.csv`
   - `Thursday-WorkingHours-Morning-WebAttacks.pcap_ISCX.csv`
4. Open the notebook in Jupyter or VS Code and run the cells in order.

The notebook uses repository-relative paths, so it does not depend on a specific local Windows directory.

## Technologies

**Python · Pandas · NumPy · scikit-learn · Matplotlib · Seaborn · Jupyter Notebook · Random Forest · Network Traffic Analysis**

## Data

The assignment uses CICIDS network-traffic CSV files distributed for the course. The raw datasets are intentionally **not included** in this repository. This keeps the repository lightweight and avoids redistributing course-provided data.

## Academic Attribution

The original assignment notebook was created by **Michael Roman** for Duke University's **CYBERSEC 520** course and adapted from course lecture notes. The completed analysis, experiments, comments, and portfolio presentation reflect Patrick Markey's coursework. The notebook retains the original attribution included in the assignment.

## About This Portfolio Project

This repository is intended to demonstrate practical AI/ML fundamentals in a cybersecurity setting: preparing security telemetry, training a classifier, evaluating detection quality, and reasoning about model tradeoffs from a SOC perspective.
