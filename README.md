# Do Self-Help Groups Help Women Stay in the Formal Credit System?

**Author:** Aarushi Gupta  
**Course:** Economics 191 (Applied Economics Research)  
**Institution:** UC Berkeley  
**Date:** Spring 2026

---

## Project Overview

This repository contains data, code, and documentation for a research paper investigating whether district-level Self-Help Group (SHG) exposure—through the National Rural Livelihoods Mission (NRLM)—increases the likelihood that rural Indian women engage in **repeat formal borrowing** (persistence in organized credit).

### Research Question
Among rural Indian women who already have bank accounts, does greater exposure to SHGs increase the likelihood that they engage in repeat formal borrowing?

### Key Contribution
The paper shifts focus from the **entry margin** (access to credit) to the **persistence margin** (sustained engagement in formal credit). While prior literature documents that SHGs improve access and reduce reliance on informal moneylenders, this paper asks: **Do women stay in the formal credit system?**

---

## Data Sources

### Primary Data
- **Consumer Pyramids Household Survey (CPHS)**
  - Conducted by Centre for Monitoring Indian Economy (CMIE)
  - Panel structure: 2017–2022 (6 annual waves)
  - Coverage: ~174,000 households nationally; ~60,000 rural banked women in analysis sample
  - Variables: borrowing source (organized vs. unorganized), household characteristics, income, occupation, education, assets
  
### Secondary Data
- **NRLM MIS Portal (nrlm.gov.in)**
  - State-year data on SHGs mobilized
  - Treatment variable: SHGs per 1,000 state population
  - Period: 2017–2022

### Sample
- Restricted to rural women with formal bank accounts
- Final regression sample: 84,681 household-wave observations (43,473 unique households)
- Geographic coverage: 488 districts, 27 states

---

## Repository Structure

```
shg-credit-persistence/
│
├── data/
│   ├── raw/                          # Original data files (CPHS, NRLM MIS)
│   │   ├── cphs_2017_2022.csv       # [User to populate]
│   │   └── nrlm_shg_data.csv        # [User to populate]
│   │
│   └── processed/
│       ├── cphs_rural_banked_female.csv     # Analysis sample after restrictions
│       ├── shg_intensity_state_year.csv     # Treatment variable
│       └── analysis_dataset.csv             # Merged final dataset for regression
│
├── code/
│   ├── 01_data_cleaning/
│   │   ├── 01_cphs_prepare.R        # Load CPHS, restrict to rural banked women
│   │   ├── 02_nrlm_prepare.R        # Prepare NRLM SHG intensity data
│   │   └── 03_merge_datasets.R      # Merge CPHS + NRLM, construct outcomes
│   │
│   ├── 02_analysis/
│   │   ├── 01_summary_statistics.R  # Table 1: Descriptive statistics
│   │   ├── 02_main_results.R        # Table 2: TWFE regressions (entry & persistence)
│   │   └── 03_outcomes_construction.R # Define AnyFormalBorrow, RepeatFormalBorrow
│   │
│   ├── 03_robustness/
│   │   ├── 01_logit_model.R         # Logit with average marginal effects
│   │   ├── 02_raw_shg_counts.R      # Robustness: raw counts + log pop control
│   │   ├── 03_attrition_weights.R   # IPW for panel attrition
│   │   └── 04_heterogeneous_effects.R # Effects by age, occupation, baseline engagement
│   │
│   └── 04_figures/
│       ├── 01_borrowing_trends.R    # Figure 1a: Borrowing by source over time
│       ├── 02_shg_intensity_map.R   # Figure 1b: Cross-state SHG intensity
│       └── 03_identification_plot.R # Figure 1c: Within-state SHG trajectories + repeat rate
│
├── output/
│   ├── tables/
│   │   ├── table_1_summary_stats.csv
│   │   ├── table_2_main_results.csv
│   │   └── table_3_robustness.csv
│   │
│   └── figures/
│       ├── figure_1a_borrowing_trends.png
│       ├── figure_1b_shg_intensity.png
│       └── figure_1c_identification.png
│
├── docs/
│   ├── proposal_v5.docx             # Final research proposal
│   ├── codebook.md                  # Data documentation
│   └── notes.md                      # Research notes and decisions
│
├── README.md                         # This file
├── .gitignore                        # Git configuration
└── LICENSE                           # MIT or CC-BY (your choice)
```

---

## Methodology

### Research Design
- **Two-way Fixed Effects (TWFE) Difference-in-Differences**
- District fixed effects absorb time-invariant district characteristics
- Year fixed effects absorb nationwide shocks (demonetization, COVID-19)
- Identification from within-state, over-time variation in SHG intensity

### Main Outcomes
1. **AnyFormalBorrow** (entry margin): 1 if household borrowed from organized sources in wave t, 0 otherwise
2. **RepeatFormalBorrow** (persistence margin): 1 if household borrowed from organized sources in both waves t and t−1, 0 if only informal in wave t; non-borrowers coded missing

### Estimating Equation
```
RepeatFormalBorrow_idt = β · SHGIntensity_st + γ · X_idt + α_d + λ_t + ε_idt
```

where:
- i = household
- d = district
- t = survey round
- X = household controls (income, education, occupation, household size, assets)
- Standard errors clustered at state level

---

## Running the Analysis

### Prerequisites
- R 4.0+
- Required packages: `tidyverse`, `fixest`, `sandwich`, `lmtest`

### Execution Order
```bash
# Data preparation
Rscript code/01_data_cleaning/01_cphs_prepare.R
Rscript code/01_data_cleaning/02_nrlm_prepare.R
Rscript code/01_data_cleaning/03_merge_datasets.R

# Main analysis
Rscript code/02_analysis/01_summary_statistics.R
Rscript code/02_analysis/02_main_results.R

# Robustness checks
Rscript code/03_robustness/01_logit_model.R
Rscript code/03_robustness/02_raw_shg_counts.R
Rscript code/03_robustness/03_attrition_weights.R

# Figures
Rscript code/04_figures/01_borrowing_trends.R
Rscript code/04_figures/02_shg_intensity_map.R
Rscript code/04_figures/03_identification_plot.R
```

---

## Key Findings (Preliminary)

| Outcome | Bivariate | + Controls | TWFE + FE |
|---------|-----------|-----------|-----------|
| **AnyFormalBorrow (Entry)** | 0.0062 | 0.0051 | 0.0018 (p=0.88) |
| **RepeatFormalBorrow (Persistence)** | 0.0084 | 0.0075 | −0.0383* (p=0.08) |

**Interpretation:** SHG intensity does not significantly affect entry into formal borrowing, but is associated with lower repeat formal borrowing. This suggests SHG expansion may shift credit composition (from banks to SHGs) rather than deepen sustained engagement.

---

## Data and Code Availability

### Raw Data
- **CPHS**: Available from CMIE (subscription required) — [Link](https://www.cmie.com/)
- **NRLM MIS**: Publicly available from [nrlm.gov.in](https://nrlm.gov.in)

### Processed Data
- Available in `data/processed/` upon request (CPHS restricted-use data)

### Code
- All analysis code is public and reproducible (provided CPHS access)
- Uses open-source R packages

---

## Citation

If you use this code or data, please cite:

```
Gupta, A. (2026). Do Self-Help Groups Help Women Stay in the Formal Credit System?
Evidence from Rural India. Unpublished manuscript, UC Berkeley.
```

---

## Contact

**Author:** Aarushi Gupta  
**Email:** [Your email]  
**GitHub:** [Your GitHub handle]

---

## License

MIT License — See LICENSE file for details.

---

## Reproducibility Checklist

- [ ] All data sources documented
- [ ] Sample restrictions clearly stated
- [ ] Outcome variable construction reproducible
- [ ] Regression specifications match paper
- [ ] Robustness checks included
- [ ] Standard errors correctly clustered
- [ ] Code runs without errors
- [ ] Results match paper tables
- [ ] Figures generated and match paper
- [ ] README complete with instructions

---

## Notes

### Data Limitations
1. **No individual SHG membership:** Analysis estimates intent-to-treat effect at state exposure level
2. **No application/rejection data:** Cannot distinguish demand-side from supply-side effects
3. **State-level treatment:** Coarser than ideal; district-level historical data unavailable
4. **Panel attrition:** Will explore inverse probability weighting as robustness check

### Future Directions
- Incorporate district-level SHG data if it becomes available
- Test heterogeneous effects by age, baseline financial engagement, occupation
- Explore Callaway & Sant'Anna staggered DiD as robustness
- Merge with loan-level administrative data to distinguish bank vs. SHG composition shifts

---

**Last Updated:** April 2026  
**Status:** In progress (final paper due May 2026)
