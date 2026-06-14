# Multi-Asset Basket Options Pricing & Delta-Hedging

A high-performance quantitative finance engine developed in Python to price multi-asset arithmetic basket options. The framework establishes a comparative pipeline between analytical moment-matching approximations and Monte Carlo simulation models enhanced by advanced variance reduction techniques.

---

## Model Parameters

The pricing engine operates on a two-asset basket model governed by multi-dimensional Brownian Motion with the following reference parameters, unless stated otherwise (e.g., when graphing sensitivities):


| Parameter | Symbol | Baseline Value |
| :--- | :---: | :---: |
| **Weights of Asset  1 and Asset 2** | $\alpha, \beta$ | $1.0 , 1.0$ |
| **Initial Spot Prices** | $S_1(0), S_2(0)$ | $1.0 , 1.0$ |
| **Volatilities** | $\sigma_1, \sigma_2$ | $0.35 , 0.4$ |
| **Correlation Coefficient** | $\rho$ | $0.3$ |
| **Risk-Free Interest Rate** | $r$ | $0.01$ |
| **Time to Maturity** | $T$ | $2.0$ (years) |
| **Strike Price** | $K$ | $2.0$ |
| **Number of MC Paths** | $N$ | $100,000$ |
---

## Pricing Methodologies

### 1. Analytical Moment-Matching Approximation
* **Log-Normal Fitting:** Fits the arithmetic basket distribution to a single log-normal asset model by matching the first two statistical moments.
* **Closed-Form Pricing:** Evaluates a modified Black-Scholes formula using an adjusted annualized basket volatility ($\sigma_B$).

### 2. Custom Monte Carlo Simulation Engine
* **Standard Monte Carlo:** Simulates joint asset trajectories using correlated Gaussian random variables.
* **Conditional expectation:** Implements basic variance reduction by simulating only a 1D Brownian Motion and evaluate the following conditional expectation.
* **Control Variate (Put Asset):** Leverages a standard Put option response as a linear control variate to stabilize path variance.
* **Control Variate (Geometric Call):** Uses a geometric average basket call option as a control covariate. The geometric call is priced exactly using a closed-form Black-Scholes equation, acting as a highly correlated variance anchor.

---

## Performance & Variance Reduction Comparison

The engine benchmarks the standard deviation (STD) of simulated payoffs to quantify convergence efficiency across different moneyness levels:

* **In-The-Money (ITM) Options:** The **Put control variate** yields the lowest standard deviation and highest efficiency, effectively absorbing intrinsic value fluctuations.
* **Out-Of-The-Money (OTM) Options:** The **Geometric Call control variate** outperforms all other methods due to its extreme correlation with the arithmetic payoff near zero.
* **Standard Error Tracking:** Calculates explicit confidence intervals ($1.645 \cdot \text{STD} / \sqrt{N}$) to prove error reduction magnitudes in real-time.

---

## Risk Management & Delta Hedging

The script computes the option's sensitivity profile with respect to the initial asset price:
* **Delta Evaluation ($\Delta_1$):** Compares pathwise Finite Difference Monte Carlo approximations against the analytical gradient of the moment-matching closed-form solution.

---

## Repository Structure

* `basket_options_final.py`: Core execution file housing the path generators, closed-form approximations, variance reduction functions, and performance graphing code.
* `project_slides_final.pdf`: Academic presentation slides outlining the underlying stochastic calculus proofs, covariance matrices, and empirical error convergence charts (in French).

---

## Requirements

Ensure you have Python installed alongside the following scientific libraries:

```bash
pip install numpy matplotlib scipy
