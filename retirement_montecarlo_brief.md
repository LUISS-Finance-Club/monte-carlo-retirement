# LFC MarketView: Retirement Monte Carlo Planner

## Project Type
Solo Jupyter Notebook

## Overview
Build a Monte Carlo simulator that answers a simple question: if someone invests €300 every month from age 25 to 65, how much will they have at retirement? Markets are uncertain, so the honest answer is not a single number but a range of possible outcomes. You will simulate 10,000 possible futures and measure how likely different results are.

This project teaches you the foundations used by every quant and wealth manager: compounding, modelling uncertainty with random numbers, and turning thousands of simulations into clear probabilities. It is designed as a first project, so no prior finance or advanced Python is required.

## What You'll Build
A complete Jupyter notebook implementing:
1. A deterministic savings calculator (fixed return, no randomness)
2. A single random path of portfolio growth
3. 10,000 simulated paths using NumPy
4. A fan chart and distribution of final wealth
5. A comparison of conservative, balanced and aggressive portfolios

## Important
The use of Generative AI (Claude) is encouraged, however it remains essential that the student understands and is able to explain without the use of the technology any design choices/implementations in the code.

## Learning Objectives
- Write loops, functions and f-strings in Python
- Understand compounding and why time in the market matters
- Generate random numbers with NumPy and understand the normal distribution
- Vectorize a simulation instead of using slow for-loops
- Interpret percentiles and probabilities
- Distinguish nominal returns from real (inflation-adjusted) returns

## PART 1: Compounding & Deterministic Growth

**Goal**: Build a savings calculator that assumes a fixed yearly return

**What is compounding?**
Each month your money earns a return, and next month you earn a return on that return as well. Over 40 years this effect becomes larger than the money you actually put in.

**Core Formula**:
```python
# Each month:
balance = balance * (1 + monthly_return) + monthly_contribution
```

**Tasks**:
- Set parameters: starting age, retirement age, monthly contribution, expected return
- Write a loop that grows the balance month by month
- Plot the balance over time
- Split the final amount into "money contributed" vs "growth from returns"
- Test how the result changes when you start at 25 vs 35

**Deliverables**:
- [ ] Deterministic growth loop works and is plotted
- [ ] Table: total contributed vs total growth
- [ ] Comparison: starting at 25 vs 35
- [ ] README: Explain compounding in your own words

## PART 2: Monte Carlo Simulation

**Goal**: Replace the fixed return with random returns and simulate thousands of futures

**What is Monte Carlo?**
Instead of assuming the market returns exactly 7% every year, we draw a random return each month from a distribution with a given average and volatility. Repeating this 10,000 times shows the full range of what could happen.

**Portfolio Assumptions** (yearly):
```
Stocks:  expected return 7%   volatility 15%
Bonds:   expected return 3%   volatility 5%
Correlation between stocks and bonds: 0.2

Monthly return  = yearly return / 12
Monthly vol     = yearly vol / sqrt(12)
```

**Portfolio Return & Volatility** (60% stocks / 40% bonds):
```python
mu_p    = w * mu_stocks + (1 - w) * mu_bonds
sigma_p = sqrt(w**2 * s_s**2 + (1-w)**2 * s_b**2 + 2*w*(1-w)*rho*s_s*s_b)
```

**Tasks**:
- Simulate one random path with a for-loop and plot it
- Simulate 10,000 paths at once with NumPy (vectorized)
- Plot a fan chart: all paths in light grey, median and 10th/90th percentiles highlighted
- Plot a histogram of final balances
- Calculate the 10th, 50th and 90th percentile outcomes

**Deliverables**:
- [ ] One random path plotted
- [ ] 10,000 simulated paths
- [ ] Fan chart with median and percentile bands
- [ ] Histogram of final wealth
- [ ] README: Explain why the median is more useful than the average here

## PART 3: Analysis & Portfolio Comparison

**Goal**: Turn simulations into decisions

**Metrics to Calculate**:
```python
Median Final    = 50th percentile of final balances
Bad Case        = 10th percentile of final balances
Good Case       = 90th percentile of final balances
P(Target)       = % of paths that reach the target (e.g. €500,000)
Real Median     = median adjusted for 2% yearly inflation
```

**Portfolios to Compare**:
```
Conservative:  30% stocks / 70% bonds
Balanced:      60% stocks / 40% bonds
Aggressive:    100% stocks / 0% bonds
```

**Visualizations**:
1. Fan chart for each portfolio
2. Overlaid histograms of final wealth (3 portfolios)
3. Bar chart: probability of reaching €500k per portfolio
4. Nominal vs real (inflation-adjusted) median growth

**Tasks**:
- Calculate the probability of reaching a target amount
- Run the simulation for all 3 portfolios
- Build a comparison table
- Adjust results for inflation
- Analyse: which portfolio would you choose at 25? At 55? Why?

**Deliverables**:
- [ ] Probability of reaching €500k calculated
- [ ] 3 portfolios simulated and compared
- [ ] Comparison table with all metrics
- [ ] Inflation-adjusted results
- [ ] README: Explain the trade-off between risk and expected wealth


**Performance Table Example**:
| Metric | Conservative | Balanced | Aggressive |
|--------|--------------|----------|------------|
| Median Final | €359k | €461k | €607k |
| Bad Case (P10) | €256k | €270k | €260k |
| Good Case (P90) | €514k | €817k | €1.54M |
| P(€500k) | 12% | 43% | 61% |
| Real Median | €163k | €209k | €275k |

**Optional Slides** (3 slides):
1. Method: What is Monte Carlo, how the simulation works
2. Key results: Fan chart, comparison table
3. Insights: What this means for a 25-year-old saver, limitations of the model

## Success Metrics

**Minimum Viable Product (MVP)**:
- [ ] Deterministic calculator working
- [ ] 10,000 Monte Carlo paths simulated
- [ ] Fan chart and histogram
- [ ] Probability of reaching target
- [ ] Comparison of 3 portfolios
- [ ] Clean, documented code

**Stretch Goals (Optional)**:
- [ ] Use real historical returns (yfinance: e.g. VWCE.DE or ^GSPC) instead of assumed ones
- [ ] Bootstrap: resample real historical monthly returns instead of a normal distribution
- [ ] Add a withdrawal phase after 65 and calculate the probability of running out of money
- [ ] Increase contributions by 2% each year (salary growth)
- [ ] Interactive sliders with ipywidgets or a small Streamlit app

## Resources

**Learning Python**:
- Kaggle Python course: https://www.kaggle.com/learn/python
- Kaggle Pandas course: https://www.kaggle.com/learn/pandas

**Key Concepts**:
- Random returns: numpy.random.default_rng().normal()
- Cumulative growth: loops or numpy.cumprod()
- Percentiles: numpy.percentile()
- Probability of target: (final_balances >= target).mean()
