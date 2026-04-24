# QUICKSTART Guide

## Project Setup

### 1. Clone the Repository
```bash
git clone https://github.com/[YOUR-USERNAME]/shg-credit-persistence.git
cd shg-credit-persistence
```

### 2. Install Required R Packages
```R
# Run this in R/RStudio
packages <- c(
  "tidyverse",      # Data manipulation
  "data.table",     # Fast data loading
  "fixest",         # TWFE regressions
  "sandwich",       # Robust standard errors
  "survey",         # Survey methods (IPW)
  "broom"           # Clean output
)

install.packages(packages)
```

### 3. Prepare Raw Data
- Download CPHS data from [CMIE website](https://www.cmie.com/)
- Download NRLM data from [nrlm.gov.in](https://nrlm.gov.in/)
- Place raw data files in `data/raw/` directory:
  - `cphs_2017_2022.csv`
  - `nrlm_shg_data.csv`
  - `census_2011_state_population.csv`

### 4. Run Analysis Pipeline

Execute in order:

```bash
# Data preparation
Rscript code/01_data_cleaning/01_cphs_prepare.R
Rscript code/01_data_cleaning/02_nrlm_prepare.R
Rscript code/01_data_cleaning/03_merge_datasets.R

# Main analysis
Rscript code/02_analysis/01_summary_statistics.R
Rscript code/02_analysis/02_main_results.R

# Robustness checks
Rscript code/03_robustness/01_combined_robustness.R

# Figures
Rscript code/04_figures/01_borrowing_trends.R
Rscript code/04_figures/02_shg_intensity_map.R
```

Or run from R:
```R
# Source all scripts in order
source("code/01_data_cleaning/01_cphs_prepare.R")
source("code/01_data_cleaning/02_nrlm_prepare.R")
source("code/01_data_cleaning/03_merge_datasets.R")
source("code/02_analysis/02_main_results.R")
source("code/03_robustness/01_combined_robustness.R")
```

### 5. Check Output
Results will be saved to:
- Tables: `output/tables/*.csv`
- Figures: `output/figures/*.png`
- Summary stats: `output/sample_funnel.csv`

---

## Key Files to Customize

1. **Data paths**: Update file paths in data loading scripts if your data is stored elsewhere
   - `code/01_data_cleaning/01_cphs_prepare.R` (line ~10)
   - `code/01_data_cleaning/02_nrlm_prepare.R` (line ~10)

2. **Variable names**: If your CPHS dataset uses different column names, update:
   - `cphs_prepare.R` variable selection section
   - `merge_datasets.R` outcome construction

3. **Sample restrictions**: Modify sample_df restrictions in data cleaning scripts as needed

4. **Regression specifications**: Update model formulas in:
   - `code/02_analysis/02_main_results.R`
   - `code/03_robustness/01_combined_robustness.R`

---

## Troubleshooting

### Issue: "Object not found" errors
**Solution:** Make sure you've run the data preparation scripts in order before analysis scripts.

### Issue: Memory/performance issues with large CPHS dataset
**Solution:** Use `data.table::fread()` instead of `read.csv()` for faster loading.

### Issue: "No cluster variable" error in `feols()`
**Solution:** Ensure state variable is properly formatted and not missing.

---

## Next Steps

1. **Add your actual regression code** — Replace placeholder code in `code/02_analysis/` with your actual specifications
2. **Create GitHub account** (if you don't have one) and follow instructions below
3. **Push to GitHub** for version control and backup

---

## Pushing to GitHub

### Create a GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Name it `shg-credit-persistence`
3. Add description: "Do Self-Help Groups Help Women Stay in the Formal Credit System?"
4. Choose **Public** or **Private** (Public is better for reproducibility)
5. Click **Create repository**

### Push Local Repository to GitHub

```bash
# Navigate to your local repo
cd /path/to/shg-credit-persistence

# Add remote
git remote add origin https://github.com/[YOUR-USERNAME]/shg-credit-persistence.git

# Add all files
git add .

# Commit
git commit -m "Initial commit: project structure and analysis code"

# Push
git branch -M main
git push -u origin main
```

### Regular Updates

```bash
# After making changes
git add .
git commit -m "Update: [describe changes]"
git push
```

---

## Citation

If others use your code, they should cite:

```bibtex
@misc{gupta2026shg,
  author = {Gupta, Aarushi},
  title = {Do Self-Help Groups Help Women Stay in the Formal Credit System?},
  year = {2026},
  howpublished = {\url{https://github.com/[YOUR-USERNAME]/shg-credit-persistence}}
}
```

---

## Need Help?

- **R Package Questions**: Check `?package_name` in R console
- **Git Questions**: See [GitHub Guides](https://guides.github.com/)
- **TWFE/DiD Questions**: Refer to `fixest` package [vignette](https://cran.r-project.org/package=fixest)

---

**Happy coding! 🎉**
