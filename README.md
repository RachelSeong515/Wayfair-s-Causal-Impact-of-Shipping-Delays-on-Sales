# Wayfair-s-Causal-Impact-of-Shipping-Delays-on-Sales

# Project Description
Wayfair is evaluating a $335M logistics hub intended to reduce average days-to-ship nationwide. Our job: estimate how changes in average delivery time affect product-level annual sales and quantify the likely revenue impact of a one-day reduction in shipping time.

# Data & Causal Framework

We use a cross-sectional, product-level dataset (simulated) containing product attributes and annual 2025 sales for thousands of SKUs. Key variables:

- Outcome (Y): annual sales (USD)
- Treatment (X): avg_days_to_ship (1–14 days)
- Confounders (A): category, weight, fulfillment_type (Wayfair vs third-party), price, popularity
- Instrument (IV): weather_exposure (days of severe weather affecting shipping)

A DAG shows A → X and A → Y (backdoor), so naive OLS risks omitted-variable bias. Weather_exposure provides exogenous variation in X (IV → X → Y) and supports identification under relevance and exclusion assumptions.

# Model 
We estimate the causal effect using a three-step progression to increase rigor:

1. Baseline OLS (short): log(sales) ~ avg_days_to_ship
2. Multivariate OLS (long): add controls (popularity, price, weight, fulfillment_type, category) to reduce OVB
3. IV (2SLS): instrument avg_days_to_ship with weather_exposure, controlling for the same covariates

Modeling details: log-transform of sales (to address skew), robust SEs, K-fold cross-validation and sensitivity analysis (sensemakr) to probe omitted-variable robustness.

# Diagnostics & Robustness

- Instrument relevance: weather_exposure strongly predicts avg_days_to_ship (corr ≈ 0.6); first-stage F-statistics are large (no weak-instrument concern).
- OVB sensitivity: an omitted confounder would need to explain ~25–27% of residual variation in both X and Y to nullify estimates—an implausibly strong hidden confounder.
- Endogeneity test (Wu–Hausman): indicates shipping time is endogenous, supporting the IV strategy.


# Main Results (elasticities of log sales)

Below are the estimates for the difference in log sales/day.

1. OLS (short) : −0.135 (−13.5% per day)
2. OLS (long, +controls): −0.144 (−14.4% per day)
3. IV (2SLS, weather) : −0.133 (−13.3% per day)

All estimates are highly statistically significant. The IV result is slightly smaller than the controlled OLS, consistent with IV identifying a LATE driven by weather-induced delays.

# Business Quantification & ROI Example

Assume the hub reduces average delivery time by 1 day across products:

- Per-product impact: ≈ +13.3% annual sales
- Example lift (client numbers from brief): ≈ $36.3M additional annual revenue (per the team’s aggregation), $309.3M across 10,000 customers/products.
- Investment comparison: At a $335M project cost, estimated annual incremental revenue ≈ $36M → implied annual ROI ≈ 10.8% and a payback period ≈ 9 years (illustrative; excludes operating costs and cross-effects).

# Business Implications

- Delivery speed materially affects revenue. Even one additional day of delay meaningfully reduces product-level sales.
- Targeted interventions may yield outsized returns. Heavier items, third-party fulfilled SKUs, and products with high weather exposure are prime candidates for prioritized logistics or local inventory buffering.
- Predictive gains ≠ causal gains. Controlling for product heterogeneity and addressing endogeneity is essential—naive correlations over- or under-state true causal effects.
- Investment decision: The hub could be economically justified if the forecasted revenue uplift and operational savings exceed capital and running costs; sensitivity analyses around uptake, cost run-rate, and cannibalization are recommended.
