# epic-array-biolearn
# 🧬 EPIC Array — Multiple Biomarkers of Aging
### Benchmarking & Evaluation of 8 Epigenetic Aging Clocks Across Two GEO Datasets Using the Bio-Learn Library

<br>

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/epic-array-aging-clocks/blob/main/notebook/EPIC_Array_BioLearn_Final.ipynb)
&nbsp;
![Python](https://img.shields.io/badge/Python-3.8%2B-3776AB?logo=python&logoColor=white)
&nbsp;
![biolearn](https://img.shields.io/badge/biolearn-latest-4CAF50)
&nbsp;
![Platform](https://img.shields.io/badge/Platform-Google%20Colab-F9AB00?logo=googlecolab&logoColor=white)
&nbsp;
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## 📌 Project Overview

This project performs a **comprehensive benchmarking and evaluation of 8 epigenetic aging clocks** across two publicly available DNA methylation datasets from the **Bio-Learn paper** (Ying et al., 2025, *Nature Aging*). The analysis covers the full pipeline — from raw GEO data loading to multi-dimensional clock comparison — and produces publication-quality visualisations including correlation matrices, age deviation heatmaps, and prediction scatter plots.

The two selected datasets (`GSE41169` and `GSE64495`) are the **exact datasets used in Figure 3 of the Bio-Learn paper**, chosen because they are small enough to run on Google Colab free-tier without RAM issues while covering two contrasting biological tissues — **blood** and **brain** — enabling a scientifically rich cross-tissue clock comparison.


---

## 📂 Datasets

Both datasets are listed in **Table 2 of the Bio-Learn paper** and were used in **Figure 3** of the same paper to benchmark epigenetic clock performance on GEO data.

---

### Dataset 1 — GSE41169 · Blood · N = 95

> **Steegenga W.T. et al.** (2014). Genome-wide age-related changes in DNA methylation and gene expression in human PBMCs. *Age*, 36(3), 9648.
> GEO: https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE41169

| Property | Details |
|---|---|
| **GEO Accession** | GSE41169 |
| **Platform** | Illumina HumanMethylation450 (450K) BeadChip |
| **Sample Size** | N = 95 |
| **Tissue** | Peripheral blood mononuclear cells (PBMCs) — whole blood |
| **Age Range** | 14 – 79 years |
| **Mean Age** | ~46 years |
| **Sex Metadata** | ✅ Yes |
| **Disease Status** | Healthy volunteers (Dutch BBMRI biobank) |
| **CpG Sites** | ~480,000 |

**Scientific description:**
This dataset profiles DNA methylation at ~480,000 CpG sites in peripheral blood from 95 healthy individuals spanning nearly the complete adult lifespan (14–79 years). It originates from the Dutch BBMRI biobank and was one of the first studies to systematically document genome-wide age-related methylation changes in blood across the human lifespan. Because all donors are healthy volunteers with no major disease diagnoses, this dataset provides the ideal **normative baseline** against which aging clock performance in blood can be cleanly assessed. Blood is also the tissue in which most Gen-2 and Gen-3 clocks (GrimAge, DunedinPACE) were trained, making it the "home tissue" for those models.

---

### Dataset 2 — GSE64495 · Brain · N = 113

> **Horvath S. et al.** (2015). Aging effects on DNA methylation modules in human brain and blood tissue. *Genome Biology*, 16, 76.
> GEO: https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE64495

| Property | Details |
|---|---|
| **GEO Accession** | GSE64495 |
| **Platform** | Illumina HumanMethylation450 (450K) BeadChip |
| **Sample Size** | N = 113 |
| **Tissue** | Prefrontal cortex (post-mortem brain tissue) |
| **Age Range** | 16 – 98 years |
| **Mean Age** | ~57 years |
| **Sex Metadata** | ✅ Yes |
| **Disease Status** | Neurologically normal controls |
| **CpG Sites** | ~480,000 |

**Scientific description:**
This dataset from Steve Horvath's lab profiles DNA methylation in post-mortem prefrontal cortex brain tissue from 113 neurologically normal individuals covering nearly the entire human lifespan (16–98 years). It is the same paper that introduced pan-tissue clock concepts to the brain context. Brain tissue has a fundamentally different epigenetic aging trajectory from blood: neurons are **post-mitotic** (they do not divide), so methylation changes in brain tissue reflect pure aging biology rather than the mix of aging and cell-division-related drift seen in blood. This dataset is scientifically critical for testing which clocks are truly **tissue-agnostic** (e.g. HorvathV1, explicitly designed to work across tissues) versus which are **blood-specific** (e.g. HannumV1, GrimAge) and therefore show systematic bias when applied to brain.

---

### Why These Two Datasets Together?

The blood vs. brain contrast is the most scientifically informative tissue comparison available in the Bio-Learn library for small datasets. The Bio-Learn paper explicitly states both datasets were used in Figure 3 to validate multi-clock epigenetic age prediction. Contrasting them reveals:

1. Which clocks generalise across tissues (tissue-agnostic)
2. Which clocks break when applied outside their training tissue (blood-specific)
3. How chronic non-dividing tissue (brain) compares to rapidly-dividing tissue (blood) in epigenetic aging dynamics

---

## ⏱️ Aging Clocks & Models

Eight epigenetic aging clocks are evaluated, spanning all four generations of biomarker development as described in the Bio-Learn paper.

---

### Generation 1 — Chronological Age Clocks

These models were trained using penalised regression (elastic-net or Lasso) to predict chronological age as accurately as possible from methylation data. They are the original "DNAm clocks."

#### 1. HorvathV1 — Pan-Tissue Clock
> Horvath S. (2013). DNA methylation age of human tissues and cell types. *Genome Biology*, 14, R115.

| | |
|---|---|
| **CpG Sites** | 353 |
| **Training Data** | >8,000 samples from 51 tissue/cell types |
| **Method** | Elastic-net regression |
| **Output** | Chronological age (years) |
| **Tissue** | Pan-tissue (all tissues) |

The foundational epigenetic clock. Trained on an unprecedented breadth of tissue types, HorvathV1 was the first clock to demonstrate that a small set of CpG methylation values can predict chronological age across virtually all human tissues with R² ≈ 0.96. Its 353 CpG sites are among the most-studied positions in the epigenomics literature. It introduced the concept of "epigenetic age acceleration" — the difference between DNAm age and chronological age — as a biomarker of biological aging. Its tissue-agnostic design makes it the gold-standard reference for cross-tissue analyses like this one.

---

#### 2. HannumV1 — Blood Clock
> Hannum G. et al. (2013). Genome-wide methylation profiles reveal quantitative views of human aging rates. *Molecular Cell*, 49(2), 359–367.

| | |
|---|---|
| **CpG Sites** | 71 |
| **Training Data** | 656 whole-blood samples |
| **Method** | Lasso regression |
| **Output** | Chronological age (years) |
| **Tissue** | Whole blood |

Developed simultaneously with HorvathV1 on the GSE40279 cohort (656 blood samples, ages 19–101), the Hannum clock uses only 71 blood-specific CpG sites. It achieves slightly better accuracy than HorvathV1 in blood specifically, but does not generalise to other tissues because its CpGs were selected exclusively in blood. On the GSE41169 (blood) dataset it is expected to perform excellently; on GSE64495 (brain) it is expected to show systematic bias.

---

### Generation 2 — Biological Age / Mortality-Linked Clocks

These models were not trained to predict chronological age but were instead trained against health outcomes, mortality risk, or clinical biomarker composites. They capture biological aging variance beyond mere time.

#### 3. PhenoAge
> Levine M.E. et al. (2018). An epigenetic biomarker of aging for lifespan and healthspan. *Aging*, 10(4), 573–591.

| | |
|---|---|
| **CpG Sites** | 513 |
| **Training Data** | InCHIANTI cohort + NHANES validation |
| **Method** | Elastic-net against phenotypic age composite |
| **Output** | Phenotypic biological age (years) |
| **Tissue** | Blood |

PhenoAge was the first clock trained not against calendar age but against a composite "phenotypic age" derived from 9 clinical blood biomarkers: albumin, creatinine, glucose, C-reactive protein, lymphocyte percentage, mean cell volume, red cell distribution width, alkaline phosphatase, and white blood cell count. This makes PhenoAge sensitive to metabolic dysfunction, immune aging, chronic inflammation, and lifestyle factors. It consistently outperforms Gen-1 clocks in predicting morbidity, disability, and all-cause mortality.

---

#### 4. GrimAge V1
> Lu A.T. et al. (2019). DNA methylation GrimAge strongly predicts lifespan and healthspan. *Aging*, 11(2), 303–327.

| | |
|---|---|
| **CpG Sites** | ~1,030 |
| **Training Data** | Framingham Heart Study + CALERIE + multiple cohorts |
| **Method** | DNAm surrogates for plasma proteins + composite |
| **Output** | Mortality-adjusted biological age (years) |
| **Tissue** | Blood |

Named after the Grim Reaper. GrimAge first trains DNAm surrogates for smoking pack-years and 7 plasma proteins known to predict mortality (GDF15, PAI1, leptin, TIMP1, β2-microglobulin, cystatin C, adrenomedullin). These surrogates are then combined into a composite that predicts time-to-death. GrimAge has consistently been the strongest predictor of lifespan, healthspan, coronary heart disease, cancer, and cause-specific mortality across multiple large cohorts. It is the benchmark Gen-2 clock.

---

#### 5. GrimAge V2
> Lu A.T. et al. (2022). DNA methylation GrimAge version 2. *Aging*, 14(23), 9484–9549.

| | |
|---|---|
| **CpG Sites** | ~1,030 + 2 additional surrogates |
| **Training Data** | Extended cohorts including inflammatory/metabolic disease |
| **Method** | Updated DNAm surrogates |
| **Output** | Mortality-adjusted biological age V2 (years) |
| **Tissue** | Blood |

An updated version of GrimAge incorporating two new DNAm surrogates: log(C-reactive protein) — capturing systemic inflammation — and log(HbA1c) — capturing long-term glycaemic dysregulation. GrimAge2 outperforms V1 in all published evaluations, particularly in cohorts with inflammatory or metabolic comorbidities. Its inclusion alongside V1 allows us to assess whether the additional surrogates improve performance on these specific datasets.

---

#### 6. Zhang-10
> Zhang Y. et al. (2017). DNA methylation signatures in peripheral blood strongly predict all-cause mortality. *Nature Communications*, 8, 14617.

| | |
|---|---|
| **CpG Sites** | 10 |
| **Training Data** | LBC1921 + LBC1936 + ESTHER cohorts |
| **Method** | Elastic-net mortality regression |
| **Output** | Mortality risk / biological age score |
| **Tissue** | Blood |

The most parsimonious clock in this study. Zhang-10 uses only 10 CpG sites selected by elastic-net regression to predict all-cause mortality in peripheral blood. Despite its radical sparsity compared to other clocks, it achieves competitive mortality prediction. Its inclusion demonstrates how much biological aging signal is concentrated in a tiny number of methylation sites, and it produces the widest scatter in predictions (least variance explained), making it a useful contrast to the CpG-rich GrimAge variants.

---

### Generation 3 — Pace of Aging

#### 7. DunedinPACE
> Belsky D.W. et al. (2022). DunedinPACE, a DNA methylation biomarker of the pace of aging. *eLife*, 11, e73420.

| | |
|---|---|
| **CpG Sites** | 173 |
| **Training Data** | Dunedin Study 1972–73 birth cohort (N=1,037), 20-year follow-up |
| **Method** | Regression on longitudinal decline in 19 organ-system biomarkers |
| **Output** | Aging rate (years of biological aging per calendar year) |
| **Tissue** | Blood |

Fundamentally different from all other clocks in this study. DunedinPACE does not estimate biological age at a single moment but rather the **speed** at which a person is currently aging. It was developed by modelling longitudinal decline across 19 physiological indicators spanning kidney, liver, lung, metabolic, immune, and cardiovascular systems in the Dunedin birth cohort over 20 years of follow-up. A score of **1.0** means aging at exactly one biological year per calendar year. A score of **1.2** means aging 20% faster than average. Test-retest reliability is exceptionally high (r = 0.90). Because it measures rate not cumulative age, it is not expected to correlate linearly with chronological age — this is visible in its scatter plot panel.

---

### Generation 4 — Causality-Enriched Clock

#### 8. YingCausAge (CausAge)
> Ying K. et al. (2022). Causally informed epigenetic age biomarkers. *Aging Cell*.

| | |
|---|---|
| **CpG Sites** | Causally-selected CpGs |
| **Training Data** | Blood methylation + Mendelian randomisation |
| **Method** | Causal inference / Mendelian randomisation feature selection |
| **Output** | Causal biological age (years) |
| **Tissue** | Blood |

One of three fourth-generation causality-enriched clocks (CausAge, DamAge, AdaptAge) from the Bio-Learn authors themselves. Unlike all previous clocks that select CpGs based on correlation with age or outcomes, YingCausAge uses **Mendelian randomisation** — a genetic instrumental variable approach — to identify CpG sites that are *causally* associated with aging rather than merely correlated with it. CausAge specifically captures CpGs that causally increase with age. This makes it the most mechanistically justified clock in this study. It represents the frontier of epigenetic biomarker methodology and is developed by the same group behind the Bio-Learn library.

---

### Clock Summary Table

| # | Model ID | Display Name | Year | Generation | Output | CpGs | Tissue |
|---|---|---|---|---|---|---|---|
| 1 | `HorvathV1` | Horvath V1 | 2013 | Gen 1 – Chronological | Age (yrs) | 353 | Pan-tissue |
| 2 | `HannumV1` | Hannum V1 | 2013 | Gen 1 – Chronological | Age (yrs) | 71 | Blood |
| 3 | `PhenoAge` | PhenoAge | 2018 | Gen 2 – Healthspan | Biological Age (yrs) | 513 | Blood |
| 4 | `GrimAge` | GrimAge V1 | 2019 | Gen 2 – Mortality | Mortality Age (yrs) | ~1030 | Blood |
| 5 | `GrimAge2` | GrimAge V2 | 2022 | Gen 2 – Mortality | Mortality Age V2 (yrs) | ~1030 | Blood |
| 6 | `Zhang_10` | Zhang-10 | 2017 | Gen 2 – Mortality | Risk Score | 10 | Blood |
| 7 | `DunedinPACE` | DunedinPACE | 2022 | Gen 3 – Pace | Rate (yrs/yr) | 173 | Blood |
| 8 | `YingCausAge` | CausAge | 2022 | Gen 4 – Causal | Causal Age (yrs) | Causal | Blood |

---

## 📊 Analysis & Visualisations

The notebook produces four categories of analysis, each applied to both datasets separately.

---

### 1. Correlation Matrix (Notebook Section 5)

**Method:** Pearson correlation computed between every pairwise combination of clock predictions and chronological age. Columns and rows are reordered using **Ward hierarchical clustering** (based on 1 − |r| distance) so that clocks with similar co-prediction patterns group together visually. Only the lower triangle is shown to avoid redundancy.

**What it reveals:**
- **Gen-1 clocks** (HorvathV1, HannumV1) cluster together — both are trained as pure chronological age predictors and achieve r > 0.90 with each other and with chronological age in blood
- **Gen-2 mortality clocks** (GrimAge V1, GrimAge V2, PhenoAge) form a secondary cluster — they correlate with age but also capture health-related variance invisible to Gen-1 clocks
- **DunedinPACE** sits apart from all other clocks — by design it measures aging *rate* not cumulative age, producing lower correlations with chronological age and other clocks
- **Cross-tissue comparison:** In brain (GSE64495), blood-trained clocks lose calibration and the clustering pattern changes, reflecting tissue-specificity of different CpG sets

**Output files:**
- `results/figures/corr_matrix_both_side_by_side.png` — both datasets side by side
- `results/figures/corr_matrix_GSE41169.png` — GSE41169 blood only
- `results/figures/corr_matrix_GSE64495.png` — GSE64495 brain only

---

### 2. Age Deviation Heatmap (Notebook Section 6)

**Method:** For each clock, **epigenetic age acceleration** is computed as:

```
Age Deviation = Predicted Clock Age − Chronological Age
```

For **DunedinPACE** (which outputs a rate, not an age), deviation is computed as:

```
DunedinPACE Deviation = (PACE Score − 1.0) × Chronological Age
```

Samples are sorted by chronological age on the x-axis. A two-panel figure is produced: a chronological age bar chart on top (viridis coloured) and the deviation heatmap below (diverging blue–red palette, clamped to ±20 years). Decade markers are overlaid as dashed vertical lines.

**What it reveals:**
- **Positive values (red)** → clock predicts the sample as biologically older than chronological age — epigenetic age acceleration
- **Negative values (blue)** → clock predicts the sample as biologically younger — deceleration
- **HorvathV1 in brain:** Expected to show least tissue bias because it was explicitly trained pan-tissue, including brain samples
- **HannumV1, GrimAge in brain:** Expected to show systematic positive deviations — blood-trained models overestimate biological age in brain tissue
- **Within-dataset patterns:** Heterogeneous column patterns indicate individual biological variation in aging rates; consistent row colours indicate clock-level systematic bias

**Output files:**
- `results/figures/deviation_heatmap_GSE41169.png`
- `results/figures/deviation_heatmap_GSE64495.png`

---

### 3. Predictions vs Chronological Age (Notebook Section 7)

**Method:** For each of the 8 clocks, a scatter plot of predicted values (y-axis) against chronological age (x-axis) is drawn. Each panel includes:
- **Raw scatter** — individual samples, alpha-blended to handle overplotting
- **OLS regression line** — ordinary least-squares linear fit
- **95% bootstrap confidence interval** — 150 bootstrap resamples, shaded around the regression line
- **Identity line (y = x)** — the theoretically perfect prediction; points above this line are biologically older, below are younger
- **Statistics annotation box** — Pearson r, MAE (mean absolute error in years), RMSE (root mean squared error in years)

**What it reveals:**
- **Tight scatter around y = x** → clock is accurate and unbiased for this tissue
- **Regression line steeper than identity** → clock overestimates in older samples and underestimates in younger ones (regression to the mean, common in all methylation clocks)
- **DunedinPACE panel:** Scatter appears essentially flat — no linear relationship with chronological age expected by design
- **Zhang-10 panel:** Widest scatter of all clocks due to using only 10 CpGs
- **Brain dataset:** GrimAge variants show large systematic positive deviations — strong visual evidence of blood-specific calibration

**Output files:**
- `results/figures/predictions_GSE41169.png` — 8-panel figure, blood dataset
- `results/figures/predictions_GSE64495.png` — 8-panel figure, brain dataset

Performance tables (Pearson r, R², MAE, RMSE, N) are also printed and saved:
- `results/tables/performance_GSE41169.csv`
- `results/tables/performance_GSE64495.csv`

---

### 4. Cross-Dataset Benchmark (Notebook Section 8)

**Method:** Grouped bar charts directly comparing Pearson r and MAE for every clock side by side across both datasets. Clocks sorted by blood-dataset Pearson r (descending). Bar labels show exact values.

**What it reveals:**
- Which clocks maintain performance when tissue changes (tissue-agnostic clocks)
- Which clocks collapse when moved out of blood (blood-specific clocks)
- The trade-off between chronological age accuracy (Gen-1 clocks) and the deliberate divergence of mortality/pace clocks from age accuracy

**Output file:**
- `results/figures/cross_dataset_benchmark.png`
- `results/tables/benchmark_combined.csv`

---

## 🚀 How to Run

### Google Colab — Recommended (Zero Setup Required)

1. Click the **Open in Colab** badge at the top of this README
2. Go to **Runtime → Run All**
3. Wait approximately **3–5 minutes** for datasets to download from GEO and all 8 clocks to run
4. All figures render inline in the notebook
5. To save figures: right-click any plot → Save Image As, or add `plt.savefig(...)` before each `plt.show()`

> **RAM:** Both datasets are small (N = 95 and N = 113). Total memory usage is under 1 GB. Runs without issue on Colab free-tier (12 GB RAM).

### Local Jupyter

```bash
# Clone
git clone https://github.com/YOUR_USERNAME/epic-array-aging-clocks.git
cd epic-array-aging-clocks

# Environment
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# Install
pip install -r requirements.txt

# Run
jupyter notebook notebook/EPIC_Array_BioLearn_Final.ipynb
```

---

## 🧱 Notebook Structure

The notebook (`EPIC_Array_BioLearn_Final.ipynb`) is organised into 6 executable cells across 9 documented sections.

| Cell | Section | Content |
|---|---|---|
| **Cell 1** | §1 Installation & Setup | Installs `biolearn`, imports all libraries, defines all global clock registries (`CLOCK_NAMES`, `CLOCK_LABELS`, `CLOCK_COLORS`). Must run first. |
| **Cell 2** | §4 Load & Run | Defines `run_all_clocks()` helper function, then immediately loads GSE41169 and GSE64495 from GEO via `DataLibrary`, applies all 8 clocks via `ModelGallery`, prints descriptive stats. |
| **Cell 3** | §5 Correlation Matrix | Defines `plot_correlation_matrix()`, produces the side-by-side Pearson heatmap for both datasets with Ward clustering. Prints top-5 and lowest correlated pairs. |
| **Cell 4** | §6 Deviation Heatmap | Defines `compute_deviations()` and `plot_deviation_heatmap()`, produces the two-panel age deviation figure for each dataset separately. Prints mean deviation table per clock. |
| **Cell 5** | §7 Predictions vs Age | Defines `plot_predictions_vs_age()`, produces the 8-panel scatter figures for each dataset, returns performance stats DataFrames. |
| **Cell 6** | §8 Cross-Dataset Benchmark | Defines `plot_benchmark()`, produces grouped bar chart comparing Pearson r and MAE across both datasets, prints merged benchmark table. |

> ⚠️ **Critical:** Cells must be run in order (1 → 2 → 3 → 4 → 5 → 6). Cell 2 defines the data objects used by Cells 3–6. Cell 1 defines the global registries used by all cells.

---

## ⚙️ Dependencies

```
biolearn          # Bio-Learn library for aging clock models and GEO datasets
numpy >= 1.23     # Numerical computing
pandas >= 1.5     # DataFrames and CSV I/O
matplotlib >= 3.6 # All plotting
seaborn >= 0.12   # Heatmaps and styled plots
scipy >= 1.9      # Pearson correlation, hierarchical clustering, squareform
jupyter >= 1.0    # Notebook execution (local only)
```

Install all at once:
```bash
pip install -r requirements.txt
```

---

## 🔬 Scientific Background

### Why Epigenetic Clocks?

DNA methylation — the addition of a methyl group to cytosine bases at CpG dinucleotides — changes predictably with age across the human genome. These age-related methylation changes can be captured by machine learning models to predict biological age from a blood or tissue sample. Different clocks capture different aspects of aging:

- **Chronological age** (Gen-1): How old you are in years
- **Biological / phenotypic age** (Gen-2): How old your cells function — can be younger or older than chronological age
- **Mortality risk** (Gen-2): How likely you are to die within a given time window
- **Pace of aging** (Gen-3): How fast you are currently aging — a rate, not an accumulated quantity
- **Causal biological age** (Gen-4): What age the causal aging machinery thinks you are — mechanistically grounded

### Key Finding from Bio-Learn Paper (Ying et al., 2025)

> "The ability of traditional aging clocks to predict chronological age does **not** correlate with mortality prediction capacity (R = 0.12, P = 0.67), suggesting that these metrics capture distinct biological processes." — *Nature Aging, 2025*

This is the central motivation for using all four generations of clocks: they are not measuring the same thing. Our analysis visualises this divergence directly through the correlation matrix and cross-tissue deviation patterns.

---

## 📁 Results Folder — What Each File Contains

### Figures

| File | Description |
|---|---|
| `corr_matrix_both_side_by_side.png` | Full side-by-side Pearson correlation heatmap for both datasets, hierarchically clustered |
| `corr_matrix_GSE41169.png` | Correlation matrix — blood dataset only |
| `corr_matrix_GSE64495.png` | Correlation matrix — brain dataset only |
| `deviation_heatmap_GSE41169.png` | Age acceleration heatmap (predicted − chronological) — blood, samples sorted by age |
| `deviation_heatmap_GSE64495.png` | Age acceleration heatmap — brain, samples sorted by age |
| `predictions_GSE41169.png` | 8-panel scatter: predicted vs chronological age, OLS regression + 95% CI — blood |
| `predictions_GSE64495.png` | 8-panel scatter: predicted vs chronological age, OLS regression + 95% CI — brain |
| `cross_dataset_benchmark.png` | Grouped bar chart: Pearson r and MAE compared across both datasets for all 8 clocks |

### Tables

| File | Description | Columns |
|---|---|---|
| `predictions_GSE41169.csv` | Raw clock predictions + chronological age — blood | `Chronological_Age, HorvathV1, HannumV1, ...` |
| `predictions_GSE64495.csv` | Raw clock predictions + chronological age — brain | Same |
| `performance_GSE41169.csv` | Summary stats per clock — blood | `Clock, Pearson_r, R2, MAE_yrs, RMSE_yrs, N` |
| `performance_GSE64495.csv` | Summary stats per clock — brain | Same |
| `benchmark_combined.csv` | Both datasets merged on Clock name for direct comparison | `Clock, Pearson_r_blood, Pearson_r_brain, MAE_blood, MAE_brain, ...` |
