# GitHub Repository Setup Guide

## What You Have

I've created a complete research project repository structure for your paper with:

- **README.md** — Project overview, data sources, methodology
- **QUICKSTART.md** — Step-by-step instructions to run the analysis
- **Data codebook** — Complete variable documentation
- **Code scripts** — Templates for all analysis steps:
  - Data cleaning (CPHS preparation, NRLM preparation, merging)
  - Main regression analysis (entry vs. persistence)
  - Robustness checks (logit, raw counts, attrition weighting)
  - Figure generation (placeholder structure)
- **.gitignore** — Automatically excludes large data files
- **LICENSE** — MIT license for open-source sharing

## How to Use This Repository

### Option 1: Use Locally (Without GitHub)

If you just want to use it on your computer:

```bash
# Copy the entire repo directory to your project folder
cp -r /home/claude/shg-credit-persistence ~/my-research/

# Customize the R scripts with your actual code
# Run the analysis
```

### Option 2: Push to GitHub (Recommended)

This makes your code reproducible, backs it up, and is professional for academic work.

#### Step 1: Create a GitHub Account (if you don't have one)

1. Go to [github.com](https://github.com)
2. Click **Sign up**
3. Create free account (use your Berkeley email)
4. Verify email

#### Step 2: Create the Repository on GitHub

1. Go to [github.com/new](https://github.com/new)
2. **Repository name**: `shg-credit-persistence`
3. **Description**: "Do Self-Help Groups Help Women Stay in the Formal Credit System? Evidence from Rural India"
4. **Visibility**: Choose **Public** (better for reproducibility and sharing in papers)
   - OR **Private** if you want to keep it private until publication
5. **Initialize**: Leave unchecked (we'll push existing code)
6. Click **Create repository**

#### Step 3: Connect Local Repository to GitHub

After creating the GitHub repo, you'll see instructions. Follow these:

```bash
# Navigate to the repository directory
cd /home/claude/shg-credit-persistence

# Configure git (first time only)
git config --global user.name "Your Name"
git config --global user.email "your.email@berkeley.edu"

# Add the remote (replace USERNAME with your GitHub username)
git remote add origin https://github.com/USERNAME/shg-credit-persistence.git

# Add all files
git add .

# Make initial commit
git commit -m "Initial commit: research code for SHG persistence paper"

# Push to GitHub (this creates the main branch)
git branch -M main
git push -u origin main
```

#### Step 4: Verify on GitHub

Go to https://github.com/USERNAME/shg-credit-persistence

You should see all your files listed there!

---

## Next: Customize the Code

The scripts I've created are **templates**. You need to fill in your actual code:

### Priority 1: Data Paths
Update file paths in these scripts to match where your actual data is stored:

**`code/01_data_cleaning/01_cphs_prepare.R`** (around line 10):
```R
# Change this:
cphs_raw <- fread("data/raw/cphs_2017_2022.csv")

# To wherever your CPHS data actually is:
cphs_raw <- fread("/path/to/your/CPHS/cphs_data.csv")
```

Same for **`code/01_data_cleaning/02_nrlm_prepare.R`**

### Priority 2: Variable Names
If your CPHS dataset uses different column names, update the variable selection in:
- `code/01_data_cleaning/01_cphs_prepare.R` (the `mutate()` and `select()` sections)
- `code/01_data_cleaning/03_merge_datasets.R`

### Priority 3: Regression Specifications
Once data is ready, update the actual regression code in:
- `code/02_analysis/02_main_results.R` (add your actual regression results)
- `code/03_robustness/01_combined_robustness.R`

---

## Making Updates to GitHub

After you modify files locally, push them to GitHub:

```bash
# From your repository directory
cd /home/claude/shg-credit-persistence

# See what changed
git status

# Add modified files
git add code/02_analysis/02_main_results.R  # Or use "git add ." to add all

# Commit with a message
git commit -m "Update: add actual regression results for entry margin"

# Push to GitHub
git push origin main
```

Each time you push, your GitHub repo is updated. Anyone (or just you) can view the entire history of changes.

---

## Repository Structure (For Reference)

```
shg-credit-persistence/
├── README.md                           # Project overview
├── QUICKSTART.md                       # Setup and run instructions
├── LICENSE                             # MIT license
├── .gitignore                          # Files to exclude from git
│
├── data/
│   ├── raw/                            # Original data (git-ignored)
│   │   ├── cphs_2017_2022.csv
│   │   ├── nrlm_shg_data.csv
│   │   └── census_2011_state_population.csv
│   └── processed/                      # Cleaned data (git-ignored)
│       ├── cphs_rural_banked_female.csv
│       ├── shg_intensity_state_year.csv
│       └── analysis_dataset.csv
│
├── code/
│   ├── 01_data_cleaning/
│   │   ├── 01_cphs_prepare.R          # Load and clean CPHS
│   │   ├── 02_nrlm_prepare.R          # Prepare treatment variable
│   │   └── 03_merge_datasets.R        # Merge and construct outcomes
│   │
│   ├── 02_analysis/
│   │   ├── 01_summary_statistics.R    # Table 1 (template)
│   │   └── 02_main_results.R          # Table 2: Main regressions
│   │
│   ├── 03_robustness/
│   │   └── 01_combined_robustness.R   # Robustness checks
│   │
│   └── 04_figures/                    # Figure generation (templates)
│       ├── 01_borrowing_trends.R
│       ├── 02_shg_intensity_map.R
│       └── 03_identification_plot.R
│
├── output/
│   ├── tables/                         # Regression output (git-ignored)
│   │   ├── table_1_summary_stats.csv
│   │   ├── table_2_main_results.csv
│   │   └── table_3_robustness.csv
│   └── figures/                        # Generated figures (git-ignored)
│       ├── figure_1a_borrowing_trends.png
│       ├── figure_1b_shg_intensity.png
│       └── figure_1c_identification.png
│
└── docs/
    └── codebook.md                    # Variable documentation
```

---

## Sharing Your Code

Once your repo is on GitHub, you can:

1. **Share the link**: Give `https://github.com/USERNAME/shg-credit-persistence` to collaborators
2. **Include in paper**: Add the GitHub URL to your paper's "Data Availability" section
3. **Use for citations**: Cite the code in your bibliography
4. **Collaborate**: GitHub makes it easy for others to submit issues or pull requests

---

## Best Practices Going Forward

1. **Commit regularly**: After major changes, commit with a clear message
   ```bash
   git commit -m "Fix: RepeatFormalBorrow coding for non-borrowers"
   ```

2. **Don't commit data**: Use `.gitignore` to exclude large files
   - Git is designed for code, not data
   - Raw data stays on your computer and in `.gitignore`

3. **Document your changes**: Each commit message should explain what changed and why

4. **Keep README updated**: As you add results, update the README with findings

5. **Add tags for versions**: When you submit the paper, tag it:
   ```bash
   git tag -a v1.0 -m "Final submitted version"
   git push origin v1.0
   ```

---

## Example: Your First Update

1. **Open** `code/02_analysis/02_main_results.R`
2. **Replace** the template regression code with your actual code
3. **Save** the file
4. **Commit and push**:
   ```bash
   git add code/02_analysis/02_main_results.R
   git commit -m "Add actual TWFE regression results (Table 2)"
   git push origin main
   ```
5. **Check GitHub** — https://github.com/USERNAME/shg-credit-persistence — and verify the updated file is there

---

## Troubleshooting

**Q: "fatal: not a git repository" error**
- **A**: Make sure you're in the correct directory (`cd /home/claude/shg-credit-persistence`)

**Q: "Permission denied" when pushing**
- **A**: You may need to set up SSH keys. Alternatively, use HTTPS with a personal access token:
  ```bash
  git remote set-url origin https://[TOKEN]@github.com/USERNAME/shg-credit-persistence.git
  ```
  (Create token at https://github.com/settings/tokens)

**Q: "no changes added to commit" when trying to push**
- **A**: Use `git add .` to add all changed files first

**Q: Want to undo a commit?**
- **A**: For uncommitted changes:
  ```bash
  git checkout -- filename.R
  ```
  For committed changes (more advanced), ask for help or check Git docs

---

## Resources

- **GitHub Guides**: https://guides.github.com/
- **Git Cheat Sheet**: https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf
- **Pro Git Book** (free): https://git-scm.com/book

---

## Questions?

If you get stuck:
1. Check the QUICKSTART.md file
2. Review the README.md methodology section
3. Look at the template code comments
4. Search "how to [problem] with git" on GitHub Help or Stack Overflow

---

**You're all set! Your research repository is ready to go. 🚀**

Next step: Customize the data paths and add your actual regression code, then push to GitHub.
