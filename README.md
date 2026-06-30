# Stellar Accretion Rate Analysis
**Author:**  
Tanay Ayiliath  
University of Oklahoma  
B.S. Astronomy, M.S. Data Science and Analytics  

**Period:** Initial Project: January 2025 - December 2025  
Cleanup and upload: June 2026  
**Status:** Active  
**Last updated:** 30 Jun 2026  

Code Repository Link: https://github.com/TanayAyiliath/stellar-accretion-analysis

## Background and Aim
The evolution of mass accretion rates in young stellar objects is a fundamental build
ing block towards understanding the growth and development of low-mass stars. This research investigates the empirical relationship between 
mass accretion rates ($\dot{M}$), stellar mass ($M_{*}$) and stellar age ($t$) in T Tauri stars using
an empirical Triple Power Law (ABC) model. 

$log(\dot{M}) = (−0.411 \pm 0.047) log(t) + (1.137 \pm 0.111) log(M_{*}) + (−5.623 \pm 0.311)$

or equivalently:

$\dot{M} = 2.383 \times 10^{−6} \times t^{−0.411} \times M_{*}^{1.137}$

Using units of $\dot{M}$ - Solar Masses per year, $t$ - Years, $M_{*}$ - Solar Masses

## Methods
Through multivariable analysis on stars with $M_{*} \leq 2 M_{\odot}$ using LMFIT, we extract a power-law regression models
that derive a quantitative scaling relationship between mass accretion rates, stellar ages and stellar mass.
Bootstrapping and residual analysis was done on our coefficients using LMFIT to constrain uncertainty tracks.
This data was then used to generate a synthetic dataset based on our model.

## Data Sources
This analysis is of a compiled dataset of 1217 low-mass young stellar objects from the L. Venuti et al. (2024) and J. Serna
et al. (2021) catalogues. Links to papers below in citations.

Dataset is included in repository as 'compilation_Venuti_2024_Serna_2021.csv'.

## Results
Our results aligned with expected trends: accretion rates strongly decline with
stellar age (exponent $\alpha_{t} \approx −0.411$), and increase with stellar mass (exponent 
$\alpha_{M_{*}} \approx 1.137$). The residual scatter identified is well characterized by a Gaussian dis
tribution having standard deviation $\sigma_{c}$ = 0.541 dex. This intrinsic scatter quantifies
the stochastic nature of accretion processes that are not captured by the triple power law.

For more information on the astrophysical factors that causes scatter, L. Hartmann et al. 2016 and L. Venuti et al. 2024
detail information regarding episodic accretion bursts, variable disk conditions, binary conditions, and turbulent viscosity.

## How to Run
Run Order Visualization:
1. data_validation.py for data checking
2. correlation_analysis.py for initial linear model
3. main_power_law_fit.py for triple power law output and cleaned data output
4. uncertainty_ab_bootstrap.py for bootstrapping of mass and age variables to create uncertainty tracks
5. (Optional) uncertainty_abc_boostrap.py for comparison of c as residual scatter vs c as bootstrapping
6. synthetic_validation.py for creation of synthetic data set and comparison to original for verification of model.

In detail:
- A csv dataset is first loaded into the data_validation.py file, which will parse the dataset for missing parameters.
If the dataset has sufficient counts of Age, Mass, and Macc (mass accretion rates), we can move forward.
- The dataset is then loaded into correlation_analysis.py for an initial linear regression analysis of Macc
against mass and age.
- We then load data into main_power_law_fit.py. This program extracts all targets with valid 'logAge', 'Mstar', 'logMacc' 
columns and runs the triple_power_law model on it, returning a numerical model, along with a log(Macc) vs log(Age) graph 
coloured by stellar mass. The output further returns a decomposition of residuals.
- The output csv and npy files are then fed into the bootstrap files. uncertainty_ab_boostrap.py returns a bootstrap
analysis of the mass and age exponents (called a and b) while using residual method for the residual scatter (denoted as variable c).
uncertainty_abc_bootstrap.py returns a boostrap analysis of all three variables. User may choose as per requirements.
- ab_bootstrap is more physically motivated and uses c to represent intrinsic scatter from individual stars.
abc_bootstrap is more statistically consistent. User may try both to see differences in scatter behaviour.
- Output of main_power_law_fit.py is then loaded into synthetic_validation.py, which
generates a synthetic sample of stars and visually compares them to the original.


Given 578 data points, 500 bootstraps:
| Program | Estimated run time |
|---|---|
| uncertainty_ab_bootstrap.py | 6-8 seconds |
uncertainty_abc_bootstrap.py | 9-11 seconds |

## Requirements
- Python 3.8+
- pip install numpy scipy matplotlib pandas lmfit  

## Troubleshooting

### Common Errors and Solutions

| Error | Likely Cause | Solution |
|-------|--------------|----------|
| `ModuleNotFoundError: No module named 'lmfit'` | Missing dependency | Run `pip install lmfit` |
| `FileNotFoundError: compilation_Venuti_2024_Serna_2021.csv` | Dataset not in working directory | Ensure CSV is in the same folder as scripts |
| `KeyError: 'logMacc'` or `KeyError: logAge` | Column name mismatch | Verify dataset has required columns: `logAge`, `Mstar`, `logMacc` |
| Bootstrap warnings (e.g., failed fits) | Random resampling issues | Normal behavior; 1-3% failures are acceptable |
| `MemoryError` | Insufficient RAM | Close other applications; make sure `n_bootstraps` is not too high, reduce if needed |
| Plots not displaying | Missing matplotlib backend | Add `%matplotlib inline` (Jupyter) or ensure GUI backend is installed |
| PDF generation fails | Permission issues | Check file write permissions; try saving to different location |

### Verifying Installation

```bash
# Check Python version
python --version

# Verify all dependencies are installed
python -c "import numpy, scipy, matplotlib, pandas, lmfit; print('All modules found')"
```

## Contributions and Acknowledgements
Project conducted under the supervision of Dr. Sean Matt and Dr. Javier Serna Qui&ntilde;ones at the Astrophysics and Cosmology department
in the Dodge College of Arts and Sciences at OU. I would like to thank them for all the support and guidance in the completion
of this project, which would not have been possible otherwise.

## Citations:
Hartmann, L., Herczeg, G., & Calvet, N. 2016, Annual Review of Astronomy and Astrophysics, 54, 135, doi: 10.1146/annurev-astro-081915023347  
Serna, J., Hernandez, J., Kounkel, M., et al. 2021, The Astrophysical Journal, 923, 177, doi: 10.3847/1538-4357/ac300a  
Venuti, L., Cody, A. M., Beccari, G., et al. 2024, The Astronomical Journal, 167, 120, doi: 10.3847/1538-4381/ad1f65  