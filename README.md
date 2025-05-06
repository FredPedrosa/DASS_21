# DASS-21 Analysis: Psychometric Properties and Anxiety Predictors 
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0) <!-- Adjust license badge if needed -->

## Overview

This repository contains the R Markdown script (`DASS_analyses.Rmd`) used for conducting a comprehensive psychometric analysis of the Depression, Anxiety, and Stress Scale-21 (DASS-21) and identifying factors influencing anxiety symptoms. The analyses were performed on data from undergraduate students at the Federal University of Ouro Preto, Brazil, as part of a larger research study on music therapy.

The script performs three main sets of analyses:

1.  **Data Preparation and Descriptive Statistics:** Imports, cleans, and imputes data, followed by a summary of participant sociodemographics.
2.  **Internal Structure Validity and Reliability Analysis (DASS-21):** Investigates the three-factor structure of the DASS-21 using Confirmatory Factor Analysis (CFA) and assesses the reliability of its subscales.
3.  **Predictors of Anxiety:** Utilizes linear regression to determine which sociodemographic and lifestyle factors predict DASS-21 anxiety scores.
4.  **Network Psychometric Analysis:** Explores the interrelationships among DASS-21 anxiety symptoms using network analysis.

## File Structure

*   **`DASS_analyses.Rmd`:** The main R Markdown file containing all R code, outputs, and narrative explanations for the analyses.
*   **`comp_reliability.R`:** A custom R script containing a function to calculate composite reliability for the DASS-21 subscales. This script is sourced by `DASS_analyses.Rmd` and must be present in the same directory.
*   **Rendered Output:** This repository might also contain a rendered version of the Rmd file `DASS_analyses.pdf` for easier viewing of the full report.

## Prerequisites

To run the `DASS_analyses.Rmd` script successfully, you will need:

1.  **R:** A recent version of R installed (developed with R version 4.4.2).
2.  **RStudio (Recommended):** An IDE like RStudio facilitates working with R Markdown files.
3.  **R Packages:** Install the following R packages:
    ```R
    # Run these lines in your R console if you don't have the packages installed
    install.packages(c(
      "readxl",     # For reading Excel files
      "dplyr",      # For data manipulation
      "mice",       # For multiple imputation
      "psych",      # For descriptive statistics (like describe())
      "lavaan",     # For Confirmatory Factor Analysis (CFA)
      "semTools",   # For additional SEM tools (like reliability())
      "semPlot",    # For plotting SEM models
      "olsrr",      # For regression diagnostics and stepwise selection
      "car",        # For regression diagnostics (like Durbin-Watson)
      "lm.beta",    # For standardized beta coefficients in regression
      "bootnet",    # For network psychometric analysis
      "qgraph",     # For plotting networks (dependency of bootnet)
      "knitr",      # For creating dynamic reports
      "kableExtra", # For enhancing HTML/LaTeX tables
      "tidyr"       # For data tidying (e.g., pivot_longer)
    ))
    ```
4.  **Custom Reliability Script:** The `comp_reliability.R` file must be in the same directory as `DASS_analyses.Rmd`.

## Data

*   This repository **does not include** the raw data file (`~/dados_limpos.xlsx`) due to privacy and data sharing restrictions.
*   You must obtain this file separately and ensure it is accessible by the R script.
*   **Crucially, you MUST update the file path** inside the `DASS_analyses.Rmd` script to point to the correct location of your data file:
    *   Look for the line `data <- read_excel("~/dados_limpos.xlsx")` (or similar) and modify the path accordingly.

## Usage

1.  **Install Prerequisites:** Ensure R and all required R packages are installed (see list above).
2.  **Prepare Data:** Place your data Excel file (e.g., `dados_limpos.xlsx`) in an accessible directory.
3.  **Place Custom Script:** Ensure `comp_reliability.R` is in the same directory as `DASS_analyses.Rmd`.
4.  **Update File Paths:** Open `DASS_analyses.Rmd` in RStudio or a text editor and modify the file path within the `read_excel()` function to match the location of your data file.
5.  **Run Analysis:** Open `DASS_analyses.Rmd` in RStudio. You can:
    *   Run individual code chunks sequentially.
    *   Use the "Run" -> "Run All" command.
    *   Knit the document (e.g., to HTML or PDF using the "Knit" button) which will execute all code chunks and generate a full report.

## Analysis Sections in `DASS_analyses.Rmd`

The script is organized into the following main sections:

1.  **Setup and Package Loading:** Loads all necessary R packages.
2.  **Data Import and Cleaning:**
    *   Reads the data from an Excel file.
    *   Removes specified irrelevant columns.
    *   Calculates and displays the proportions of different academic community members participating.
    *   Subsets the data to include only undergraduate students for subsequent analyses.
3.  **Data Imputation:**
    *   Identifies a single missing value in the DASS-21 items for the undergraduate subgroup.
    *   Performs multiple imputation using Predictive Mean Matching (PMM) via the `mice` package.
    *   Uses the first imputed dataset for subsequent analyses.
4.  **Sociodemographic Data:**
    *   Presents descriptive statistics (mean, SD, median, range for age; frequencies and percentages for categorical variables) for the undergraduate student sample.
    *   Includes age, ethnicity/race, biological sex, gender identity, religion, yoga/meditation practice, physical exercise, sleep satisfaction, and self-reported mental illness diagnosis.
5.  **Evidence of Internal Structure Validity and Reliability of the DASS-21:**
    *   **Confirmatory Factor Analysis (CFA):**
        *   Specifies and fits the theoretical three-factor model (Anxiety, Stress, Depression) of the DASS-21 using `lavaan`.
        *   Employs the `WLSMV` estimator, appropriate for ordinal item data.
        *   Evaluates model fit using standard indices (χ², CFI, TLI, RMSEA).
        *   Visualizes the factor model using `semPlot`.
    *   **Reliability Testing:**
        *   Calculates various reliability coefficients (Cronbach's Alpha, Ordinal Alpha, McDonald's Omega via `semTools`).
        *   Calculates Composite Reliability using the custom `comp_reliability.R` script.
    *   **Factor Score Estimation:** Estimates latent factor scores for Anxiety, Stress, and Depression using `lavaan::lavPredict`.
6.  **Testing Which Sociodemographical Factors Predict Anxiety:**
    *   Conducts a multiple linear regression with DASS-21 Anxiety factor scores as the outcome variable.
    *   Uses stepwise forward selection (based on AIC) via `olsrr` to identify significant predictors among sociodemographic and lifestyle variables.
    *   Presents the final model summary, ANOVA table, and parameter estimates (including standardized beta coefficients).
    *   **Checking Model Assumptions:** Verifies residuals normality (Shapiro-Wilk test), homoscedasticity (Breusch-Pagan test), and residual autocorrelation (Durbin-Watson test).
7.  **Network Psychometric Analysis (DASS-21 Anxiety Items):**
    *   Estimates a network structure of the DASS-21 anxiety items using `bootnet` with the `EBICglasso` method.
    *   Visualizes the network.
    *   Calculates and plots centrality indices (Strength, Closeness, Betweenness, Expected Influence).
    *   Assesses the stability of centrality indices using case-dropping bootstrapping and `corStability`.
8.  **Session Information:** Includes output from `sessionInfo()` to document R and package versions.

## Outputs

The `DASS_analyses.Rmd` script, when run or knitted, will produce:

*   Descriptive statistics tables for sociodemographic data.
*   CFA model fit statistics and parameter estimates.
*   Reliability coefficient estimates.
*   Linear regression model summaries and diagnostic test results.
*   Network plots and centrality index tables/plots.
*   Correlation stability analysis results for network centrality.
*   All outputs are embedded within the rendered document if knitted (e.g., to HTML or PDF).

## Important Notes

*   **File Paths:** The most critical step for reproducibility is updating the path to your data file within `DASS_analyses.Rmd`.
*   **WLSMV Estimator:** The CFA uses the WLSMV estimator, appropriate for ordinal DASS-21 data. Fit indices reported by `lavaan` for WLSMV are the correctly adjusted ones.
*   **`comp_reliability.R`:** This custom script is essential for calculating composite reliability as presented in the report.

## How to Cite

If you use or adapt this code or analysis structure, please cite this repository.

### Citing this Repository/Code:
Pedrosa, F. (2025). *DASS-21 Analysis: Psychometric Properties and Anxiety Predictors*. GitHub Repository. https://github.com/FredPedrosa/DASS_21/

## Author

*   **Frederico Pedrosa**
*   fredericopedrosa@ufmg.br

## License

This project is licensed under a modified version of the GNU General Public License v3.0. 
Commercial use is not permitted without explicit written permission from the author.
