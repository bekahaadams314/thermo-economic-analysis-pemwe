# Techno-Economic & Risk Analysis of PEM Water Electrolyzer Systems

Quantitative techno-economic and risk-analysis framework for green hydrogen production via proton-exchange-membrane (PEM) water electrolysis. The project models the full economics of a hydrogen production asset — cost, degradation, and capital allocation — the way an investor or technical diligence team would need to evaluate a hydrogen or energy-hardware venture.

## Why This Matters

Levelized Cost of Hydrogen (LCOH) is the metric that determines whether a green hydrogen project is investable, but LCOH on its own hides two things investors care about most: how fast the hardware degrades, and how that degradation risk should shape capital deployment across a portfolio of energy assets. This project treats those three questions — cost, technical risk, and allocation — as one connected problem rather than three separate spreadsheets.

## Repository Structure

| Notebook | Question it answers |
|---|---|
| `lcoh_prediction.ipynb` | What does it cost to produce a kilogram of hydrogen over the system's lifetime? |
| `stochastic_degradation_and_VaR_PEMWE.ipynb` | How much financial risk does stack degradation/failure introduce, and what's the Value-at-Risk? |
| `portfolio_allocation_energy_hydrogen.ipynb` | Given that risk, how should capital be allocated across a hybrid energy-to-hydrogen system? |

## Methodology & Key Findings

### 1. LCOH Prediction (`lcoh_prediction.ipynb`)
Builds a discounted cash flow model for a 1 MW PEM electrolyzer over a 10-year project life.

- **CAPEX:** $1.5M initial build + $400K stack replacement in Year 6
- **OPEX:** $45K/year (insurance, maintenance, land)
- **Degradation:** stack efficiency declines 1.5%/year from an initial 55 kWh/kg H₂
- **Electricity price:** modeled as a lognormal distribution (mean 3.5, std 0.5), clipped to $10–$100/MWh, against hourly power draw sampled uniformly between 0.2–1.0 MW across 8,760 hours/year
- **Discount rate:** 8%

**Result:** baseline annual electricity cost of ~$197,200 against ~5.29M kWh consumed annually, yielding a discounted **LCOH of $5.54/kg H₂**.

### 2. Stochastic Degradation & Value-at-Risk (`stochastic_degradation_and_VaR_PEMWE.ipynb`)
Models stack failure as a **Weibull survival process** and runs a Monte Carlo simulation across 1,000 simulated stack lifetimes to characterize the distribution of replacement timing and cost. This converts "the stack might fail early" from a qualitative engineering concern into a quantified **Value-at-Risk** figure — the same risk language used in financial underwriting — so degradation risk can be priced into the investment case rather than treated as a footnote.

### 3. Portfolio Allocation for Hybrid Energy-to-Hydrogen Systems (`portfolio_allocation_energy_hydrogen.ipynb`)
Runs a 500-iteration Monte Carlo simulation to evaluate how capital and capacity should be split across a hybrid energy system (e.g., grid, renewables, and electrolyzer capacity) under uncertainty, identifying allocations that are robust to the cost and degradation risk quantified in the first two notebooks.

## Languages Used in this Project:
Python · NumPy · Pandas · SciPy · Matplotlib/Seaborn · Jupyter

## How to Run
```bash
git clone https://github.com/bekahaadams314/thermo-economic-analysis-pemwe.git
cd thermo-economic-analysis-pemwe
pip install -r requirements.txt   # numpy, pandas, scipy, matplotlib, seaborn
jupyter notebook
```
Run `lcoh_prediction.ipynb` first, then `stochastic_degradation_and_VaR_PEMWE.ipynb`, then `portfolio_allocation_energy_hydrogen.ipynb` — each builds on assumptions established in the prior notebook.

## Limitations & Future Work
- Electricity price and degradation distributions are parameterized from literature-informed assumptions rather than a specific operating site; swapping in real meter/utility data would sharpen the LCOH estimate.
- The portfolio allocation model currently treats the hybrid system at a single representative scale; extending it to multiple electrolyzer/storage configurations would make the allocation analysis more directly comparable across deal sizes.
- Next step: connect the VaR output from the degradation model directly into the LCOH discount-rate assumption, so technical risk feeds back into the cost-of-capital used for valuation.

## Author
Rebekah Adams — PhD Candidate, Mechanical Engineering, Carnegie Mellon University
[linkedin.com/in/rebekah-ann-adams](https://linkedin.com/in/rebekah-ann-adams/) · [github.com/bekahaadams314](https://github.com/bekahaadams314)
