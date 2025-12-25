# 🏉Top 14 Predictions 2026 - Machine Learning Model

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-red?style=flat&logo=pytorch)
![Status](https://img.shields.io/badge/Status-Active-success)

## 📖 Overview

**Top 14 Predictions 2026** is a data science and machine learning project designed to forecast the outcomes of the **French Top 14** rugby season (2024-2025).

Unlike traditional statistical models that rely solely on win rates, this project implements a **Deep Neural Network (MLP)** using **PyTorch**. It analyzes granular performance metrics (Offloads, Defensive Efficiency, Discipline) to predict the exact **point differential** of future matches.

The core engine includes a **Season Simulator** that applies official Top 14 rules (including Offensive and Defensive Bonus points) to generate a projected final leaderboard.

---

## 📂 Project Architecture

The project is built around a robust data pipeline and a regression-based neural network.

### 1. Advanced Data Engineering
* **Data Source:** Statistical aggregates from the current season (`Data top 14 with modifications.csv`) scraped by myself.
* **Home/Away Split:** The model distinguishes between a team's performance at home versus away, crucial for rugby analysis.
* **Feature Engineering:** Creation of contextual metrics:
    * `Diff_Context`: Home Attack vs. Visitor Defense.
    * `Diff_Inverse`: Home Defense vs. Visitor Attack.
    * `Diff_Discipline`: Penalty differentials.
* **Preprocessing:** Implementation of **`RobustScaler`** (based on Interquartile Range) to normalize data while ignoring statistical outliers (e.g., blowout scores of 50+ points), preserving the nuance of close matches.

### 2. Deep Learning Model (PyTorch)
The model is a **Regression Multi-Layer Perceptron (MLP)** designed to predict a continuous value (the score gap).

**Key Technical Concepts:**
* **Architecture:** Custom `nn.Module` with linear layers, **ReLU** activation, and **Batch Normalization** for stability.
* **Regularization:**
    * **Dropout:** Randomly deactivates neurons during training to prevent the model from memorizing past matches.
    * **Weight Decay (L2):** Penalizes complex models to reduce overfitting on small datasets.
* **Training Strategy:**
    * **Loss Function:** `MSELoss` (Mean Squared Error) to minimize the gap between predicted and actual scores.
    * **Early Stopping:** Monitors a validation set (20% of data) and automatically halts training when the model stops generalizing, preventing "rote learning."

### 3. Simulation Engine
The predictor feeds into a simulation loop that:
1. Predicts the score gap for all remaining matches in the calendar.
2. Converts score gaps into **Try Differentials** (approx. 7 pts/try).
3. Allocates points based on Top 14 Rules:
    * **4 Pts** for a Win.
    * **+1 Pt** for Offensive Bonus (3+ tries gap).
    * **+1 Pt** for Defensive Bonus (Loss by ≤ 5 points).
4. Updates the live standings to predict the final Champion, semi-finalists, and relegated teams.

---

## 🛠 Technologies & Libraries

* **Core:** Python 3.x
* **Deep Learning:** [PyTorch](https://pytorch.org/) (`torch`, `torch.nn`, `torch.optim`)
* **Data Processing:** Pandas, NumPy
* **Preprocessing:** Scikit-Learn (`RobustScaler`, `train_test_split`)

---

## 🚀 Results & Performance

The model demonstrates a capability to capture the **Home Advantage** phenomenon typical of the Top 14.
* **Training Loss:** Stabilizes around ~100-150 MSE (indicating an average error margin of ~10-12 points, consistent with rugby volatility).
* **Validation:** Early stopping typically triggers around 100-150 epochs to maintain optimal generalization.

### Sample Output (Simulation)
```text
🏉 --- PRÉDICTIONS DES MATCHS À VENIR ---
DOMICILE        vs EXTÉRIEUR       | VAINQUEUR    | SCORE
-----------------------------------------------------------------
Toulouse        vs La Rochelle     | Toulouse     | +14
Vannes          vs Lyon            | Lyon         | +6
Castres         vs Bordeaux        | Castres      | +4 (BD)

🏆 --- CLASSEMENT FINAL PROJETÉ ---
1. Toulouse : 84 pts ✅ Demies
2. Stade Français : 78 pts ✅ Demies
...
13. Montpellier : 42 pts ⚠️ Barrage Maintien
```

### 👤 Author
Arthur Quairel  
Student at **Espci Paris - PSL**, data & rugby Lover ;)

