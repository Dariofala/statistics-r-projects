# 📊 Statistical Learning & Time Series in R

![R](https://img.shields.io/badge/R-276DC3?logo=r&logoColor=white)
![R Markdown](https://img.shields.io/badge/R%20Markdown-reports-75AADB)
![caret](https://img.shields.io/badge/caret-classification-orange)
![forecast](https://img.shields.io/badge/forecast-time%20series-green)

Two data analysis projects developed for the **Statistics II** course — MSc in Artificial Intelligence and
Data Engineering, University of Pisa, academic year 2025/26. Each project is a reproducible R Markdown report.

| Project | Question | Methods |
|---------|----------|---------|
| [🎮 LoL role classification](lol-role-classification/) | Can a pro player's role be predicted from end-of-game statistics? | PCA, k-NN, LDA, 10-fold cross-validation |
| [🏭 Japan industrial production](japan-industrial-production/) | How well can 70 years of monthly industrial output be forecast? | Decomposition, naive baselines, Holt-Winters, bootstrap intervals |

---

## 🎮 Role classification in League of Legends

📄 **Report:** [`classificazione_ruoli_lol.md`](lol-role-classification/classificazione_ruoli_lol.md) ·
💻 **Source:** [`classificazione_ruoli_lol.Rmd`](lol-role-classification/classificazione_ruoli_lol.Rmd)

Every player in a *League of Legends* match takes one of five roles (Top, Jungle, Mid, Bot, Support).
Using **960 player-games from the 2025 World Championship**, the project builds multiclass classifiers that predict
the role from 10 per-minute and share-based statistics (damage, gold, farm, vision, kills, deaths, assists…).

- **Exploratory analysis**: correlation matrix and PCA on standardised features (three components retained by
  Kaiser's criterion and the elbow method). Projected on the first two components, Support and Jungle form two
  clearly separated clusters, while Bot, Mid and Top overlap.
- **Models**: k-NN tuned over odd k ∈ [1, 31] and Linear Discriminant Analysis, both evaluated with the same
  10-fold cross-validation and standardisation inside each fold.
- **Results** (cross-validated, per-role metrics on out-of-fold predictions):

  | Model | Accuracy | F1 Jungle | F1 Support | F1 Top | F1 Bot | F1 Mid |
  |-------|---------:|----------:|-----------:|-------:|-------:|-------:|
  | k-NN (k = 15) | **82.0%** | 1.00 | 1.00 | 0.81 | 0.73 | 0.55 |
  | LDA | 81.5% | 1.00 | 1.00 | 0.81 | 0.72 | 0.55 |

  Jungle and Support are recognised perfectly; most errors come from Mid vs Bot, two roles with very similar resource
  and damage profiles. Positional features (e.g. experience at 10 minutes) would be the natural next step to separate them.

## 🏭 Forecasting Japan's industrial production

📄 **Report:** [`serie_storiche_giappone.md`](japan-industrial-production/serie_storiche_giappone.md) ·
💻 **Source:** [`serie_storiche_giappone.Rmd`](japan-industrial-production/serie_storiche_giappone.Rmd)

Monthly, not seasonally adjusted **industrial production index of Japan (1955–2024)**: post-war boom, the stagnation
after the 1990s and the 2008 and 2020 shocks. Data up to December 2022 (68 full years) is used for training and
January 2023 – March 2024 as the test set.

- **Exploratory analysis**: ACF, seasonal and sub-series plots, multiplicative decomposition (seasonal swings grow with the trend).
- **Models**: naive, seasonal naive and drift baselines, multiplicative Holt-Winters.
- **Diagnostics**: Ljung-Box and Shapiro-Wilk tests on the residuals reject white noise and normality because of the
  economic shocks, so prediction intervals are recomputed with the **bootstrap**.
- **Results** on the 2023–2024 test set:

  | Model | RMSE | MAE | MAPE |
  |-------|-----:|----:|-----:|
  | **Seasonal naive** | **3.25** | **2.64** | **2.84%** |
  | Holt-Winters (multiplicative) | 3.44 | 2.95 | 3.16% |
  | Naive | 7.16 | 5.72 | 6.32% |
  | Drift | 7.78 | 6.44 | 7.11% |

  The simple seasonal naive forecast beats Holt-Winters: a strongly seasonal series hit by structural shocks does
  not necessarily reward a more complex model.

## 🚀 Reproducing the reports

Requirements: R and the packages used in the reports.

```r
install.packages(c("ggplot2", "ggcorrplot", "caret", "tidyr", "forecast", "patchwork", "rmarkdown"))
```

Open an `.Rmd` file in RStudio and press **Knit**, or from the project folder:

```r
rmarkdown::render("lol-role-classification/classificazione_ruoli_lol.Rmd")
rmarkdown::render("japan-industrial-production/serie_storiche_giappone.Rmd")
```

Both reports run on the data included in the repository. Reports and code comments are in Italian.

## 🔧 Post-exam revision

- Per-role precision, recall and F1-score of the classifiers are now computed on the **out-of-fold** cross-validation
  predictions; the original version computed them on the training data, which made them optimistic.
- Added the PCA projection of the players coloured by role.
- The forecasting comparison now reports RMSE, MAE and MAPE for **all** models.
- The classification report runs from the included cleaned dataset when the raw Oracle's Elixir file is not available.

## 📚 Data sources

- **League of Legends** — [Oracle's Elixir](https://oracleselixir.com/tools/downloads), 2025 match data
  (only the cleaned subset used in the analysis is included).
- **Industrial production** — OECD Main Economic Indicators via
  [FRED, series JPNPRINTO01IXOBM](https://fred.stlouisfed.org/series/JPNPRINTO01IXOBM), © OECD, CC BY 4.0.

## 👤 Author

**Dario Falaschi** — [@Dariofala](https://github.com/Dariofala)
