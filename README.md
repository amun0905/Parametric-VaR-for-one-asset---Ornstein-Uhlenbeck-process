# VaR for One Asset – Ornstein–Uhlenbeck (OU) Process

This notebook explores **Value-at-Risk (VaR)** estimation for a single asset whose **price dynamics** are modeled by an **Ornstein–Uhlenbeck (OU) process)** in log-space.  
The OU model is a **mean-reverting Gaussian process**, offering a more realistic alternative to Geometric Brownian Motion (GBM) for assets that exhibit strong reversion or high volatility without drifting to zero.

---

##  Model Overview

The **Ornstein–Uhlenbeck process** for the log-price \( X_t = \log P_t \) is given by:

$$
dX_t = \theta(\mu - X_t)\,dt + \sigma\,dW_t
$$

where  
- \( \theta \) — speed of mean reversion  
- \( \mu \) — long-run mean  
- \( \sigma \) — volatility  
- \( W_t \) — standard Wiener process  

The solution is:

$$
X_t = X_0 e^{-\theta t} + \mu(1 - e^{-\theta t}) + \sigma \int_0^t e^{-\theta (t-s)} dW_s
$$

From simulated or empirical prices, we compute **simple returns** and **log returns**:

$$
r_t = \frac{P_t - P_{t-1}}{P_{t-1}}, \quad \ell_t = \log\left(\frac{P_t}{P_{t-1}}\right)
$$

---

##  Value-at-Risk (VaR) Estimation

The notebook compares **three parametric VaR approaches**:

1. **Normal assumption on simple returns**
   $$
   VaR_{\alpha} = \mu_r + z_{\alpha}\sigma_r
   $$

2. **Normal assumption on log returns**
   $$
   VaR_{\alpha} = e^{\mu_{\ell} + z_{\alpha}\sigma_{\ell}} - 1
   $$

3. **Empirical VaR (historical)**
   $$
   VaR_{\alpha}^{emp} = -\text{Quantile}_{\alpha}(r_t)
   $$

where \( z_{\alpha} \) is the \( \alpha \)-quantile of the standard normal distribution.

---

##  Holding Period Scaling

To extend 1-day VaR to an \( h \)-day horizon:

- **Linear scaling (normal returns):**
  $$
  VaR_h = VaR_1 \sqrt{h}
  $$

- **Lognormal scaling (exponential compounding):**
  $$
  VaR_h = e^{\sqrt{h}\,VaR_1} - 1
  $$

The notebook shows that **lognormal scaling** yields estimates closer to empirical quantiles, especially over longer holding periods (10–130 days).

---

##  Key Insights

- The OU process provides **stable, mean-reverting log-prices** suitable for volatile assets.  
- **Lognormal (exponential) scaling** captures compounding effects better than linear scaling.  
- For short horizons, all VaR methods are similar; for long horizons, **lognormal VaR** is most accurate.
