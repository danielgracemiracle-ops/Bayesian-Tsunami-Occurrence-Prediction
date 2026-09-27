# 🌊 Bayesian Tsunami Occurrence Prediction

## 📌 Project Overview

This project develops a **Bayesian Logistic Regression model** to estimate the probability of tsunami occurrence based on earthquake characteristics.

The analysis uses earthquake-related variables such as **magnitude, depth, distance (`dmin`), seismic gap (`gap`), Modified Mercalli Intensity (`mmi`), Community Internet Intensity (`cdi`), and seismic significance (`sig`)**.

Unlike conventional logistic regression that primarily produces point estimates, the Bayesian approach models the **full posterior distribution of parameters**, allowing uncertainty to be quantified through credible intervals and posterior predictions.

The project was implemented in **R using the `brms` framework**, with posterior inference performed using **Hamiltonian Monte Carlo (HMC)** through the Stan backend.

---

## 🎯 Objectives

The main objectives of this project are:

* Estimate the probability of tsunami occurrence from earthquake characteristics.
* Apply **Bayesian Logistic Regression** to a binary classification problem.
* Quantify uncertainty in model parameters using posterior distributions.
* Analyze which earthquake characteristics have stronger associations with tsunami occurrence.
* Evaluate model convergence and predictive performance.
* Assess the model using Bayesian-specific diagnostics such as **Posterior Predictive Checks, WAIC, and LOO**.

---

## 📊 Dataset

The project uses the **Global Earthquake & Tsunami Risk Assessment Dataset** from Kaggle.

🔗 Dataset:
https://www.kaggle.com/datasets/ahmeduzaki/global-earthquake-tsunami-risk-assessment-dataset

Each observation represents an earthquake event with information about its characteristics and whether it generated a tsunami.

### Target Variable

| Variable  | Description            |
| --------- | ---------------------- |
| `tsunami` | Binary target variable |
| `0`       | No tsunami             |
| `1`       | Tsunami occurred       |

### Predictor Variables

The model considers earthquake characteristics including:

* `magnitude`
* `depth`
* `dmin`
* `gap`
* `mmi`
* `cdi`
* `sig`

These variables represent different physical and seismic characteristics associated with earthquake events.

---

## 🧠 Methodology

### 1. Data Preparation

The dataset was prepared by removing rows containing missing or inconsistent values before modeling.

The analysis then focused on earthquake characteristics relevant to tsunami generation.

---

### 2. Bayesian Logistic Regression

Because the target variable is binary, the project uses **Bayesian Logistic Regression**.

The model assumes that the tsunami outcome follows a Bernoulli distribution:

```text
yᵢ ~ Bernoulli(pᵢ)
```

The model estimates the probability of tsunami occurrence based on the earthquake predictors.

The Bayesian formulation combines:

```text
Prior × Likelihood → Posterior
```

The posterior distribution provides a probability distribution for each model parameter rather than only a single coefficient estimate.

---

### 3. Prior Distribution

The model uses **weakly informative priors** for the regression coefficients.

The purpose is to keep the coefficients reasonably constrained while still allowing the observed earthquake data to strongly influence the posterior estimates.

---

### 4. MCMC / HMC Inference

Posterior inference was performed using **Hamiltonian Monte Carlo (HMC)** through the Stan backend via the `brms` package in R.

Model configuration:

| Parameter          | Setting |
| ------------------ | ------: |
| Chains             |       4 |
| Iterations / chain |   4,000 |
| Warm-up            |   1,000 |
| `adapt_delta`      |    0.95 |
| `max_treedepth`    |      15 |

Model convergence was evaluated using:

* R-hat
* Trace plots
* Effective Sample Size (ESS)

All reported parameters achieved **R-hat ≈ 1.00**, indicating satisfactory convergence of the MCMC chains.

---

## 📈 Model Results

### Posterior Parameter Analysis

The posterior analysis showed different levels of association between earthquake characteristics and tsunami probability.

The most notable result was:

**`dmin`**

* Posterior coefficient: **β = 0.51**
* 95% Credible Interval: **[0.40, 0.62]**

The credible interval does not cross zero, indicating a clear positive posterior association in this dataset.

For **magnitude**:

* Posterior coefficient: **β = 0.44**
* 95% Credible Interval: **[-0.02, 0.89]**

Although the posterior mean shows a positive trend, its credible interval slightly overlaps zero. The report notes that this may be related to collinearity with intensity variables such as MMI and CDI.

Other findings include:

* `cdi` showed a positive effect.
* `mmi` showed a negative adjustment in the current model.
* `depth` and `sig` had estimates close to zero under the current configuration.

---

## 🔍 Bayesian Diagnostics

### Posterior Predictive Check

Posterior Predictive Checks (PPC) were performed using:

```r
pp_check()
```

The PPC analysis compared observed outcomes with replicated outcomes generated from the posterior distribution.

The report indicates good agreement between the observed and replicated distributions, with no major signs of model misfit.

---

### WAIC & LOO

Two Bayesian model evaluation criteria were used:

| Metric |  Estimate |
| ------ | --------: |
| WAIC   | **875.0** |
| LOOIC  | **875.1** |
| SE     |  **34.8** |

The LOO diagnostics reported all Pareto-k values below 0.7, indicating reliable LOO estimates in the analysis.

---

## 📊 Classification Performance

Posterior predictive probabilities were converted into binary predictions using a **0.5 threshold**.

### Confusion Matrix

| Actual / Predicted |   0 |   1 |
| ------------------ | --: | --: |
| **0**              | 423 | 143 |
| **1**              |  55 | 161 |

The model achieved:

### AUC = **0.822**

The ROC-AUC indicates that the model has good ability to discriminate between tsunami and non-tsunami earthquake events within the evaluated dataset.

---

## 📉 Visualizations

The project includes Bayesian diagnostic and interpretation visualizations such as:

### Posterior Interval Plot

A coefficient interval / forest plot is used to visualize posterior estimates and their credible intervals.

### MCMC Trace Plots

Trace plots are used to evaluate whether the MCMC chains mix properly and reach convergence.

The reported trace plots showed well-mixed chains without visible trends or stuck chains.

### Posterior Predictive Checks

PPC visualizations compare observed data with data simulated from the posterior distribution.

---

## 🛠️ Technologies & Tools

### Programming Language

* **R**

### Main Framework

* **brms**
* **Stan**

### Statistical Method

* Bayesian Logistic Regression
* Bayesian Inference
* Markov Chain Monte Carlo (MCMC)
* Hamiltonian Monte Carlo (HMC)

### Model Evaluation

* R-hat
* Effective Sample Size
* Posterior Predictive Checks
* WAIC
* LOO / LOOIC
* ROC-AUC
* Confusion Matrix
* Credible Intervals

---

## 📁 Project Structure

```text
Bayesian-Tsunami-Prediction/
│
├── data/
│   └── earthquake_dataset.csv
│
├── R/
│   ├── preprocessing.R
│   ├── exploratory_analysis.R
│   ├── bayesian_model.R
│   └── evaluation.R
│
├── plots/
│   ├── posterior_intervals.png
│   ├── trace_plots.png
│   ├── posterior_predictive_check.png
│   └── roc_curve.png
│
├── report/
│   └── Bayesian_Tsunami_Prediction.pdf
│
└── README.md
```

> *Project structure may vary depending on the final organization of the repository.*

---

## 🚀 Workflow

```text
Earthquake Dataset
        ↓
Data Cleaning & Preparation
        ↓
Exploratory Analysis
        ↓
Bayesian Logistic Regression
        ↓
Prior + Likelihood
        ↓
MCMC / HMC Sampling
        ↓
Posterior Distribution
        ↓
Convergence Diagnostics
        ↓
Posterior Predictive Checks
        ↓
WAIC & LOO
        ↓
Classification Evaluation
        ↓
Tsunami Probability Estimation
```

---

## 💡 Key Takeaways

This project demonstrates how Bayesian statistical modeling can be applied to a real-world disaster prediction problem.

The main insights from the analysis are:

* Bayesian Logistic Regression can estimate tsunami occurrence probability while explicitly representing parameter uncertainty.
* MCMC diagnostics indicated satisfactory model convergence.
* `dmin` showed the clearest positive association with tsunami probability in the fitted model.
* Magnitude showed a positive posterior trend, although its credible interval slightly crossed zero.
* The model achieved an **AUC of 0.822**.
* WAIC and LOO provided additional tools for evaluating probabilistic model performance.
* Posterior Predictive Checks indicated no major evidence of model misfit.

---

## 🔮 Future Improvements

Potential future development includes:

1. Incorporating additional geophysical variables such as fault mechanism, rupture parameters, and tectonic setting.
2. Testing more informative priors based on seismological knowledge.
3. Comparing Bayesian Logistic Regression with models such as Random Forest, Gradient Boosting, or Gaussian Process models.
4. Adding probability calibration metrics such as Brier Score and calibration curves.
5. Testing the model with real-time seismic monitoring data for potential early-warning applications.

---

## 👨‍💻 Author

**Daniel Graceko Miracle**
Data Science — BINUS University

This project was developed as part of a data science / Bayesian statistical modeling study focusing on probabilistic tsunami prediction.

---

## 📚 References

1. Dogan, G., Slunga, R., & Elliott, J. (2019). *Tsunami probability estimation from earthquake characteristics*. Natural Hazards, 97(2), 679–694.
2. Gaillard, J. C., et al. (2018). *Disaster risk reduction and uncertainty in geophysical models*. Progress in Physical Geography, 42(1), 1–18.
3. Gelman, A., Carlin, J., Stern, H., Dunson, D., Vehtari, A., & Rubin, D. (2013). *Bayesian Data Analysis* (3rd ed.).
4. McElreath, R. (2020). *Statistical Rethinking* (2nd ed.).
5. Vehtari, A., Gelman, A., & Gabry, J. (2017). *Practical Bayesian model evaluation using leave-one-out cross-validation and WAIC*. Statistics and Computing, 27(5), 1413–1432.
