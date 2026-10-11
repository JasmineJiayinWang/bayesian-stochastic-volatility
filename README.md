# Bayesian Stochastic Volatility

**Author: Jiayin Wang**

Two research notebooks studying latent volatility in equity returns, progressing from a univariate S&P 500 model to a bivariate IBM/S&P 500 model implemented in JAX. The notebooks compare fitted paths and parameter summaries with textbook reference values recorded in the original work.

## Contents

| Notebook | Scope | Implementation |
| --- | --- | --- |
| [Univariate SV](notebooks/01_univariate_sv.ipynb) | S&P 500 returns, Gaussian GARCH initialization, Bayesian stochastic volatility, posterior parameter summaries and variance paths | NumPy, SciPy, `arch`; Gibbs and Griddy Gibbs updates |
| [Bivariate SV](notebooks/02_bivariate_sv_jax.ipynb) | Aligned IBM/S&P 500 returns, Cholesky-GARCH and BEKK initialization, latent variance and correlation paths | JAX with 64-bit arithmetic; Gibbs and Griddy Gibbs updates |

## Models

The univariate specification combines Gaussian returns with an autoregressive log-variance process:

$$r_t=\mu+\sqrt{h_t}\epsilon_t,\qquad \log h_t=\alpha_0+\alpha_1\log h_{t-1}+v_t.$$

For the bivariate specification, demeaned returns satisfy

$$b_{1t}=a_{1t},\qquad b_{2t}=a_{2t}-q_ta_{1t},\qquad b_t\mid G_t\sim N(0,G_t),$$

where $G_t$ is diagonal, each log-variance follows an AR(1) process, and $q_t=\gamma_0+u_t$. This decomposition implies a time-varying covariance matrix. The first log-variance values are held fixed in the bivariate implementation.

## Run locally

Use a Python environment supported by the installed JAX release. Dependency versions are currently unpinned; a reproducible environment lock has not yet been established.

```bash
python -m venv .venv
# Activate the environment using the command appropriate for your shell.
python -m pip install -r requirements.txt
python -m jupyterlab
```

The supplied `sp500_raw.xlsx` and `ibm.csv` files are included in `data/raw/`; see [the data guide](data/raw/README.md) for their schemas. Open either notebook and run its cells in order from a fresh kernel. The notebooks can be launched from the repository root or the notebooks directory. Generated plots are written to `figures/` and, for some univariate plots, `notebooks/figures/`.

## Current status and interpretation

- The supplied original data files are included. Both data-processing pipelines were executed successfully and produce 575 finite monthly return observations, spanning February 1962 to December 2009. Existing model tables and plots are retained from the supplied notebooks; the full estimation and MCMC workflows have not been independently rerun for this package.
- Both main samplers are configured for 2,500 iterations with 500 discarded as burn-in. The bivariate notebook also includes a shorter-chain comparison.
- This is exploratory research code. The finite Griddy Gibbs intervals require sensitivity analysis; the univariate implementation clamps an inverted interval to a narrow positive-width interval, whose statistical effect needs investigation.
- Data construction can affect comparison with reference values. In particular, the IBM series uses the first available daily adjusted close in each month; the exact S&P 500 price convention should be documented.
- Saved summaries and chain comparisons do not establish convergence by themselves. Multiple-chain diagnostics, effective sample sizes, and interval sensitivity remain useful extensions.
- The textbook title, edition, and complete bibliographic citation need to be added by the author. No source attribution has been guessed.

## Packaging notes

The research computations are preserved. Packaging adds this README, dependency and data guidance, descriptive filenames, and a working-directory/output-directory setup cell. Environment installation logs and machine-specific environment display cells were removed. Python syntax and both data-processing pipelines were checked. The full analysis was not executed because the verification environment lacks `arch`, `statsmodels`, and `jax`. The source data are copied byte-for-byte; `ibm(1).csv` is named `ibm.csv` to match the notebook path.
