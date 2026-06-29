# Techno-Economic & Risk Analysis of PEM Water Electrolyzer Systems

Quantitative techno-economic and risk-analysis framework for green hydrogen production via polymer electrolyte membrane (PEM) water electrolysis. The project models the full economics of a hydrogen production asset, which includes the cost, degradation, and capital allocation involved in a stack.

## Motivation:

Levelized Cost of Hydrogen (LCOH) is the metric that determines whether a green hydrogen project is investable, but LCOH on its own hides two things investors care about most: how fast the hardware degrades, and how that degradation risk should shape capital deployment across a portfolio of energy assets. This project views the full economics of hydrogen production as connected rather than separate.

## Repository Structure:

| Notebook | Question |
|---|---|
| `lcoh_prediction.ipynb` | What does it cost to produce a kilogram of hydrogen over the system's lifetime? |
| `stochastic_degradation_and_VaR_PEMWE.ipynb` | How much financial risk does stack degradation/failure introduce, and what's the Value-at-Risk? |
| `portfolio_allocation_energy_hydrogen.ipynb` | Given that risk, how should capital be allocated across a hybrid energy-to-hydrogen system? |

## Methodology & Key Findings:

### LCOH Prediction (`lcoh_prediction.ipynb`)
Builds a discounted cash flow model for a 1 MW PEM electrolyzer over a 10-year project life.

- **CAPEX:** $1.5M initial build + $400K stack replacement in Year 6
- **OPEX:** $45K/year (insurance, maintenance, land)
- **Degradation:** stack efficiency declines 1.5%/year from an initial 55 kWh/kg H₂
- **Electricity price:** modeled as a lognormal distribution (mean 3.5, std 0.5), clipped to $10–$100/MWh, against hourly power draw sampled uniformly between 0.2–1.0 MW across 8,760 hours/year
- **Discount rate:** 8%

**Result:** baseline annual electricity cost of ~$197,200 against ~5.29M kWh consumed annually, yielding a discounted LCOH of $5.54/kg H₂.

### Stochastic Degradation & Value-at-Risk (`stochastic_degradation_and_VaR_PEMWE.ipynb`)
- Models stack failure as a Weibull survival process and runs a Monte Carlo simulation across 1E3 simulated stack lifetimes to characterize the distribution of replacement timing and cost.
- This converts the engineering concern into a quantified Value at Risk figure so degradation risk can be priced into the investment case rather than treated as a footnote.

### Portfolio Allocation for Hybrid Energy-to-Hydrogen Systems (`portfolio_allocation_energy_hydrogen.ipynb`)
Runs a 500-iteration Monte Carlo simulation to evaluate how capital and capacity should be split across a hybrid energy system (e.g., grid, renewables, and electrolyzer capacity) under uncertainty, identifying allocations that are robust to the cost and degradation risk quantified in the first two notebooks.

## Languages and Packages Used in this Project:
- Python (NumPy, Pandas, SciPy, Matplotlib, Seaborn)
- Jupyter

## Limitations & Future Work
- Electricity price and degradation distributions are parameterized from literature-informed assumptions rather than a specific operating site so the next aim to use real utility data to improve the LCOH estimate.
- The portfolio allocation model currently treats the hybrid system at a single representative scale. Extending it to multiple electrolyzer/storage configurations would make the allocation analysis more directly comparable.
- Connect the VaR output from the degradation model directly into the LCOH discount-rate assumption, so technical risk feeds back into the cost-of-capital used for valuation.
