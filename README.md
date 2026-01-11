# LD Decay Analysis in R

[![R](https://img.shields.io/badge/R-%3E%3D3.6.0-blue.svg)](https://www.r-project.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

A comprehensive R-based tool for analyzing and visualizing linkage disequilibrium (LD) decay patterns from TASSEL LD analysis output. This tool implements the Hill and Weir (1988) expectation model to estimate recombination parameters and calculate LD decay half-distance.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
  - [Step 1: Generate LD Data in TASSEL](#step-1-generate-ld-data-in-tassel)
  - [Step 2: Run LD Decay Analysis](#step-2-run-ld-decay-analysis)
- [Output](#output)
- [Methodology](#methodology)
- [Example Data](#example-data)
- [Citation](#citation)
- [Contributing](#contributing)
- [License](#license)

## Overview

Linkage disequilibrium (LD) decay analysis is crucial for understanding the extent of non-random association of alleles at different loci in a population. This information is essential for:

- **Association mapping studies**: Determining marker density requirements
- **Population genetics**: Understanding recombination patterns and demographic history
- **Breeding programs**: Designing genomic selection strategies
- **Evolutionary studies**: Inferring population structure and admixture

This R script processes LD output from TASSEL software, fits a non-linear regression model based on the Hill and Weir expectation, and generates publication-quality LD decay plots with half-decay distance annotations.

## Features

- ✅ **Automated data processing**: Handles TASSEL LD output files with built-in data cleaning
- ✅ **Non-linear regression modeling**: Implements Hill and Weir (1988) expectation model
- ✅ **Recombination parameter estimation**: Calculates population recombination rate (ρ = 4Nₑr)
- ✅ **LD decay metrics**: Computes half-decay distance for LD assessment
- ✅ **Publication-quality plots**: Generates customizable PDF visualizations
- ✅ **Flexible input**: Interactive file selection for ease of use

## Prerequisites

### Software Requirements

- **R** (version ≥ 3.6.0)
- **TASSEL** (version 5.0 or higher) - for generating LD analysis output

### R Packages

No external R packages are required. The script uses base R functions only.

## Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/SudhakarBasu/LD-Decay-Plot-in-R.git
   cd LD-Decay-Plot-in-R
   ```

2. **Verify R installation**:
   ```bash
   R --version
   ```

3. **Download TASSEL** (if not already installed):
   - Visit [TASSEL website](https://www.maizegenetics.net/tassel)
   - Download and install the appropriate version for your operating system

## Usage

### Step 1: Generate LD Data in TASSEL

Before running the R script, you need to generate LD analysis output from TASSEL:

#### 1. Data Preparation

- Ensure your genotype data is in a compatible format (HapMap, VCF, or Plink)
- Clean and filter data for quality and missing data if necessary
- Example formats:
  - **HapMap**: `.hmp.txt` format (as provided in `mdp_genotype.hmp.txt`)
  - **VCF**: Variant Call Format
  - **Plink**: `.bed/.bim/.fam` files

#### 2. Open TASSEL

Launch TASSEL software on your computer.

#### 3. Import Genotype Data

- Navigate to the **Data** tab
- Use import tools to load your genotype data file
- Verify data import success in the data tree

#### 4. Perform LD Analysis

- Select your imported genotype data
- Navigate to **Analysis** → **Linkage Disequilibrium**
- Configure LD parameters:
  - **LD Type**: Full Matrix or Sliding Window
  - **Window Size**: Typically 50-100 SNPs (adjust based on genome size)
  - **MAF Threshold**: Minimum minor allele frequency (e.g., 0.05)
  - **LD Statistics**: Select r² (required for this script)

#### 5. Run Analysis

- Click **Run** to start the LD calculation
- TASSEL will compute pairwise LD statistics across your dataset

#### 6. Export LD Results

- Right-click on the LD analysis result in the data tree
- Select **Export** → **Save to File**
- Choose a location and save as a tab-delimited text file (e.g., `Tassel_LD_Result.txt`)

### Step 2: Run LD Decay Analysis

1. **Open R or RStudio**

2. **Set working directory** (optional):
   ```r
   setwd("path/to/LD-Decay-Plot-in-R")
   ```

3. **Run the script**:
   ```r
   source("LD decay Plot from TASSEL LDoutput.R")
   ```

4. **Select input file**:
   - A file dialog will appear
   - Navigate to and select your TASSEL LD output file (e.g., `Tassel_LD_Result.txt`)

5. **Wait for processing**:
   - The script will automatically:
     - Clean the data (remove NaN values)
     - Fit the non-linear regression model
     - Calculate LD decay parameters
     - Generate the plot

## Output

### Generated Files

- **`LD_decay.pdf`**: Publication-quality plot showing:
  - Raw LD (r²) values vs. physical distance (grey points)
  - Fitted LD decay curve (red line)
  - Baseline threshold at r² = 0.1 (blue horizontal line)
  - Half-decay distance (green vertical line with annotation)

### Interpretation

- **Red curve**: Expected LD decay based on the Hill and Weir model
- **Half-decay distance**: Physical distance (bp) at which LD drops to 50% of its maximum value
  - Shorter distances indicate faster LD decay (higher recombination)
  - Longer distances suggest slower LD decay (lower recombination or recent bottleneck)
- **r² = 0.1 threshold**: Commonly used cutoff for significant LD in association studies

### Example Output

The plot will display:
- X-axis: Physical distance between markers (bp)
- Y-axis: LD measured as r²
- Green annotation: Half-decay distance value in base pairs

## Methodology

### Statistical Model

The script implements the **Hill and Weir (1988) expectation** for LD decay:

```
E[r²] = [(10 + ρd) / ((2 + ρd)(11 + ρd))] × 
        [1 + ((3 + ρd)(12 + 12ρd + (ρd)²)) / (2N(2 + ρd)(11 + ρd))]
```

Where:
- **r²**: Squared correlation coefficient (LD measure)
- **ρ**: Population recombination parameter (4Nₑr)
- **d**: Physical distance between markers (bp)
- **N**: Sample size (number of individuals)
- **Nₑ**: Effective population size
- **r**: Recombination rate per base pair per generation

### Analysis Pipeline

1. **Data Import**: Read TASSEL LD output file
2. **Data Cleaning**: Remove entries with NaN values for distance or r²
3. **Model Fitting**: Non-linear least squares regression to estimate ρ
4. **Prediction**: Calculate expected r² values across distance range
5. **Half-Decay Calculation**: Identify distance where r² = 0.5 × max(r²)
6. **Visualization**: Generate annotated LD decay plot

### Key Parameters

- **Starting C value**: 0.1 (initial guess for ρ)
- **Maximum iterations**: 100 (for non-linear model convergence)
- **LD threshold**: r² = 0.1 (commonly used in GWAS)

## Example Data

The repository includes example data for testing:

- **`mdp_genotype.hmp.txt`**: Sample HapMap genotype file (maize diversity panel)
- **`Tassel_LD_Result.txt`**: Pre-computed TASSEL LD analysis output
- **`LD_decay.pdf`**: Example output plot

To test the script with example data:
```r
source("LD decay Plot from TASSEL LDoutput.R")
# When prompted, select: Tassel_LD_Result.txt
```

## Citation

If you use this tool in your research, please cite:

**Hill, W. G., & Weir, B. S. (1988).** Variances and covariances of squared linkage disequilibria in finite populations. *Theoretical Population Biology*, 33(1), 54-78.

For TASSEL software:
**Bradbury, P. J., Zhang, Z., Kroon, D. E., Casstevens, T. M., Ramdoss, Y., & Buckler, E. S. (2007).** TASSEL: software for association mapping of complex traits in diverse samples. *Bioinformatics*, 23(19), 2633-2635.

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

### How to Contribute

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/YourFeature`)
3. Commit your changes (`git commit -m 'Add YourFeature'`)
4. Push to the branch (`git push origin feature/YourFeature`)
5. Open a Pull Request

### Suggestions for Improvements

- Add support for D' LD statistic
- Implement chromosome-specific LD decay analysis
- Add command-line interface for batch processing
- Include additional LD decay models (e.g., exponential decay)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

**Sudhakar Basu**
- GitHub: [@SudhakarBasu](https://github.com/SudhakarBasu)

---

**Acknowledgments**: This tool was developed to facilitate LD decay analysis in population genetics and association mapping studies. Special thanks to the TASSEL development team for providing comprehensive LD analysis capabilities.
