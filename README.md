# DSCP-Dependent-Sparse-Change-Point-Model
Simultaneous Covariate Selection and Change-point Detection in Dependent Binary Time Series: A Sparse Bayesian Approach

# DSCP: Dependent Sparse Change-Point Model

This repository provides the official R implementation of the **Dependent Sparse Change-Point (DSCP)** model. 

The DSCP model is designed for Bayesian structural break detection in high-dimensional binary time series. It seamlessly integrates autoregressive dynamics, covariate selection, and change-point detection using Reversible Jump Markov Chain Monte Carlo (RJMCMC) and Pólya-Gamma data augmentation.

## ✨ Key Features
* **Reversible Jump MCMC (RJMCMC):** Dynamically estimates the unknown number of change-points ($K$) and their locations ($\tau$) using Split/Merge (Birth/Death) and Shift moves.
* **Pólya-Gamma Data Augmentation:** Enables exact and efficient Gibbs sampling for the logistic-autoregressive likelihood without relying on Metropolis-Hastings tuning for the coefficients.
* **Stochastic Search Variable Selection (SSVS):** Performs sparse covariate selection within each detected regime, using spike-and-slab priors to identify active predictors.
* **Modular Codebase:** Clean, robust, and highly modular R functions (`move_birth`, `move_death`, `update_ssvs_pg`) designed for numerical stability and high-dimensional scalability.

## 📦 Dependencies
The code is written in base `R`, but requires the following packages for data augmentation and visualization:
```R
install.packages(c("pgdraw", "MASS", "ggplot2"))

## 🚀 Quick Start & Example

The main script includes a complete simulation study. You can run the code directly to simulate data, run the MCMC chain, and plot the results.

### 1. Run the Main MCMC
The main function `DSCP_mcmc_complete` takes your binary time series `y` and covariate matrix `X`:

R
source("DSCP_model.R")

# Run the MCMC sampler
res <- DSCP_mcmc_complete(y = y, 
X = X, 
n_iter = 3000, 
burn_in = 1000, 
n_min = 20,       # Minimum segment length
prop_var = 0.1)   # Proposal variance for RJMCMC

### 2. Visualize Posterior Distributions
The script automatically generates plots for the posterior distribution of the number of regimes ($K$) and the locations of the change-points ($\tau$):

R
# View the estimated number of regimes
K_table <- table(res$K)
barplot(K_table, main="Posterior Distribution of K", col="steelblue")

# View the change-point locations
# (assuming K > 1 is the most frequent state)
idx_mode <- which(res$K == as.numeric(names(which.max(K_table))))
tau_samples <- sapply(res$tau[idx_mode], function(x) x[2:(length(x)-1)])
hist(unlist(tau_samples), breaks=30, col="lightgreen", main="Change-Points")

## ⚙️ Core Functions Overview
* `segment_loglik()`: Computes the logistic-AR log-likelihood for a given time segment.
* `update_ssvs_pg()`: Implements Algorithm 2, executing Pólya-Gamma draws and SSVS with exact marginal likelihood calculations.
* `move_birth() / move_death() / move_shift()`: Implements Algorithm 1, managing the trans-dimensional RJMCMC steps using auxiliary variables to maintain high acceptance rates.
* `DSCP_mcmc_complete()`: The main wrapper orchestrating the Gibbs and RJMCMC updates.

\el.R` ذخیره کرده و همراه با این `README.md` در گیت‌هاب آپلود کنید.
۲. بخش **Citation** در انتهای فایل را می‌توانید بعد از چاپ شدن مقاله، با اطلاعات دقیق ژورنال و نام نویسندگان همکار به‌روزرسانی کنید.
