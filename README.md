# 🐟 Fish Weight Prediction — Regression Analysis

A comparative regression study predicting the weight of Northern European freshwater fish from physical measurements, completed as part of the **MSc in Open Data Practice** at **National College of Ireland**.

---

## 📌 Project Overview

Can you predict how much a fish weighs just by measuring it?

This project builds and compares three classes of regression model on the [Fish Market Dataset](https://www.kaggle.com/datasets/vipullrathod/fish-market) to estimate fish weight (in grams) from total length, height, width, and species.

| Model | RMSE (g) | Adj. R² |
|---|---|---|
| Linear Regression | 91.81 | 0.930 |
| Linear with Interactions | 51.74 | 0.977 |
| **Log-Log Regression** ✅ | **50.70** | **0.996** |

> **Best model:** Logarithmic (log-log) regression  
> **Predicted weight** of a Perch (H=12.8cm, W=6.9cm, L=41.9cm) → **907.62 grams**

---

## 📂 Repository Structure

```
fish-weight-regression/
│
├── Statistic_CA1.ipynb       # Full Jupyter Notebook with all code and outputs
├── Fishmarket.csv            # Dataset
├── report.pdf                # IEEE-format report (6 pages)
│
├── figures/
│   ├── fig1_weight_distribution.png
│   ├── fig2_weight_by_species.png
│   ├── fig3_scatter_plots.png
│   ├── fig4_correlation_heatmap.png
│   ├── fig5_linear_diagnostics.png
│   ├── fig6_interaction_diagnostics.png
│   ├── fig7_log_scatter.png
│   ├── fig8_log_diagnostics.png
│   └── fig9_rmse_comparison.png
│
└── README.md
```

---

## 📊 Dataset

The **Fish Market Dataset** contains **158 observations** of 7 Northern European freshwater fish species:

| Column | Unit | Description |
|---|---|---|
| Species | — | Bream, Roach, Whitefish, Parkki, Perch, Pike, Smelt |
| Weight | g | Fish weight (target variable) |
| Length1 | cm | Standard length (snout → last vertebra) |
| Length2 | cm | Fork length (snout → tail fork) |
| Length3 | cm | Total length (snout → longest tail end) |
| Height | cm | Body height (belly to top) |
| Width | cm | Body width (thickness) |

---

## 🔍 Methodology

### 1. Exploratory Data Analysis
- Descriptive statistics for all variables
- Removed **1 erroneous observation** (Weight = 0g — a data entry error)
- Detected severe multicollinearity among Length1, Length2, Length3 (all r > 0.99)
- Dropped Length1 and Length2 — retained **Length3** only
- Visualised weight distributions, species boxplots, and scatter plots

### 2. Regression Models

**Model 1 — Multiple Linear Regression**
```
Weight = β₀ + β₁·Length3 + β₂·Height + β₃·Width + Σβₛ·Species + ε
```
- RMSE: 91.81g | R²: 0.934
- Residuals showed clear heteroscedasticity and non-normality

**Model 2 — Linear Regression with Interactions**
```
Weight = β₀ + β₁L + β₂H + β₃W + β₄L·H + β₅L·W + β₆H·W + Σβₛ·Species + ε
```
- RMSE: 51.74g | R²: 0.979
- Captures volume-like multiplicative relationship between predictors

**Model 3 — Logarithmic (Log-Log) Regression** ✅
```
log(Weight) = β₀ + β₁·log(Length3) + β₂·log(Height) + β₃·log(Width) + Σβₛ·Species + ε
```
- RMSE: 50.70g | R²: 0.996
- Grounded in allometric biology (fish weight ~ power law of body dimensions)
- Sum of exponents ≈ 3.03 — consistent with cubic volume scaling
- Best residual diagnostics: near-homoscedastic, near-normal Q-Q plot

### 3. Final Prediction

Using the log-log model, the predicted weight of a **Perch** with:
- Height = 12.8 cm
- Width = 6.9 cm  
- Total Length = 41.9 cm

```
log(Ŵ) = −2.967 + 1.794·log(41.9) + 0.706·log(12.8) + 0.526·log(6.9) + 0.260
Ŵ = exp(log(Ŵ)) ≈ 907.62 grams
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.13 | Core language |
| pandas | Data loading and manipulation |
| numpy | Numerical operations and log transforms |
| matplotlib | Plotting charts and figures |
| seaborn | Heatmaps and styled visualisations |
| statsmodels | OLS regression model fitting and diagnostics |
| scikit-learn | RMSE calculation |
| Jupyter Notebook | Interactive development environment |

---

## ▶️ How to Run

1. **Clone the repository**
```bash
git clone https://github.com/your-username/fish-weight-regression.git
cd fish-weight-regression
```

2. **Install dependencies**
```bash
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn jupyter
```

3. **Launch the notebook**
```bash
jupyter notebook Statistic_CA1.ipynb
```

4. **Run all cells in order**  
   Go to `Kernel → Restart & Run All` to reproduce all results from scratch.

---

## 📈 Key Results

<table>
  <tr>
    <th>Metric</th>
    <th>Linear</th>
    <th>Interaction</th>
    <th>Log-Log ✅</th>
  </tr>
  <tr>
    <td>RMSE (g)</td>
    <td>91.81</td>
    <td>51.74</td>
    <td><strong>50.70</strong></td>
  </tr>
  <tr>
    <td>Adj. R²</td>
    <td>0.930</td>
    <td>0.977</td>
    <td><strong>0.996</strong></td>
  </tr>
  <tr>
    <td>Condition No.</td>
    <td>936</td>
    <td>14,800</td>
    <td><strong>208</strong></td>
  </tr>
  <tr>
    <td>Residuals</td>
    <td>Heteroscedastic</td>
    <td>Improved, heavy tails</td>
    <td><strong>Near-normal ✅</strong></td>
  </tr>
</table>

---

## 📄 Report

The full IEEE-format report is included as `report.pdf`. It covers:
- Abstract and keywords
- Introduction and methodology
- Exploratory Data Analysis with figures
- Three regression model sections (formula, refinement, diagnostics, RMSE)
- Conclusion with model comparison and Perch weight prediction

---

## 👤 Author

**Hafiz Muhammad Ibraheem**  
MSc Open Data Practice — National College of Ireland  
Student ID: 24167860  
Module: MSCODP1 — Statistics  
Lecturer: Dr Christian Horn  

---

## 📜 References

1. V. Rathod, "Fish Market Dataset," Kaggle, 2021. https://www.kaggle.com/datasets/vipullrathod/fish-market  
2. W. E. Ricker, "Computation and Interpretation of Biological Statistics of Fish Populations," *Bulletin of the Fisheries Research Board of Canada*, vol. 191, 1975.  
3. G. James, D. Witten, T. Hastie, and R. Tibshirani, *An Introduction to Statistical Learning*, 2nd ed. Springer, 2021.  
4. S. Seabold and J. Perktold, "statsmodels: Econometric and Statistical Modeling with Python," *Proc. 9th Python in Science Conf. (SciPy)*, 2010.
