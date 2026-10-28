# Coffee Derivatives Pricing | Black-Scholes & Monte Carlo in R


## 1. Overview
Pricing a coffee futures contract and a European call option in R, using the cost of carry model, Black-Scholes and Monte Carlo simulation, with a sensitivity analysis on spot price and volatility.

This project was completed as part of the Citi Markets Quantitative Analysis virtual job simulation (Forage). The analysis illustrates how financial models can be used to estimate derivative prices and assess exposure to commodity market risk. This projet represents an educational simulation and independent work, it is not affiliated with or endorsed by Citi.


## 2. Project Objectives
1. Price a six-month coffee futures contract with the cost of carry model.
2. Price a European call option on coffee using the Black-Scholes model.
3. Implement a Monte Carlo simulation to estimate the same option's value.
5. Compare the analytical and simulated prices.
6. Analyse the sensitivity of option prices to changes in the underlying spot price and volatility.
7. Discuss how supply, demand, weather and geopolitical factors affect pricing.


 ## 3. Assumptions
 | Parameter | Value |
 |---|---|
 | Coffee Spot price ($S_0$)| \$1.20/lb |
 | Risk-free rate ($r$)| 2% |
 | Annual storage cost ($d$)| 1% |
 | Time to maturity ($T$)| 0.5 years |
 | Strike price ($X$)| \$1.25/lb |
 | Annual volatility ($\sigma$)| 25% |
 | Monte Carlo simulation paths | 10,000 |


## 4. Methods
 
### 4.1 Step 1: Cost of carry model
The theoretical futures price is estimated using:

$$
F = S_0 \cdot e^{(r + d)T}
$$

where $S_0$ is the current spot price, $r$ the risk-free rate, $d$ the storage cost and $T$ the time to maturity.

### 4.2 Step 2: Black-Scholes model
The Black-Scholes framework is used to value a European call option on the underlying spot asset.
The model assumes constant volatility and interest rates, lognormally distributed underlying prices and frictionless markets.

$$
C = S_0 \cdot N(d_1) - X e^{-rT} N(d_2)
$$

where $d_1 = \dfrac{\ln(S_0/X) + (r + \sigma^2/2)T}{\sigma\sqrt{T}}$ and $d_2 = d_1 - \sigma\sqrt{T}$.

### 4.3 Step 3: Monte Carlo simulation
Terminal prices are simulated under geometric Brownian motion in the risk-neutral measure:

$$
S_T = S_0 \cdot \exp\left[\left(r-\frac{\sigma^2}{2}\right)T + \sigma\sqrt{T}Z\right]
$$

where $Z \sim \mathcal{N}(0,1)$.

A total of 10,000 simulated outcomes are generated with a fixed random seed for reproducibility.
For each simulated terminal price, the European call payoff is:

$$
\text{Payoff}_i = \max(S_T^{(i)} - X, 0)
$$

The call price is the average payoff, discounted at the risk-free rate:

$$
C_{MC} = e^{-rT} \cdot \frac{1}{n}\sum_{i=1}^{n} \text{Payoff}_i
$$

### 4.4 Step 4: Sensitivity
The Black-Scholes call price is recomputed for spot prices from $1.00 to $1.40 per pound and for volatilities from 15% to 40%.
- Spot price sensitivity: Examines how changes in the coffee spot price affect the call premium.
- Volatility sensitivity: Examines how changes in annualized volatility affect the call premium.


## 5. Results
| Output | Value (\$/lb) |
|---|---|
| Futures price (cost of carry) | \$1.2181/lb |
| Call price, Black-Scholes | \$0.0684/lb |
| Call price, Monte Carlo | \$0.0677/lb |

### 5.1 Sensitivity findings
- Monte Carlo converges to the Black-Scholes price (difference under \$0.001), which provides a numerical consistency check.
- The call price is convex in the spot price (positive gamma) and increases with volatility (positive vega): from about \$0.035 at 15% volatility to \$0.119 at 40% volatility.

#### Spot Price Sensitivity
![Call Option Price vs Spot Price](figures/spot_sensitivity.png)

#### Volatility Sensitivity
![Call Option Price vs Volatility](figures/volatility_sensitivity.png)

### 5.2 Commodity Market risk
- Supply and demand: Production shortages or stronger demand may push the call price up through the spot price.
- Weather: Droughts, frost and excessive rainfall may disrupt production and push the call price up through volatility.
- Geopolitical risks: Trade restrictions, transportation disruptions and political instability may affect supply chains and storage costs. The risk the call price up through volatility, and higher storage costs raise the futures prices.

  
## 6. Limitations and possible extensions
- The option is priced on the spot price, so the storage cost affects the futures price but not the option price. The Black-76 model, which prices options on futures price, would provide a more appropriate modeling approach under its assumptions.
- The convenience yield is not modeled, so the model cannot produce backwardation.
- Constant volatility ignores the seasonal and upward-skewed volatility of coffee.
- Geometric Brownian motion ignores mean reversion and price jumps from weather events.


## 7. Tools and Technologuies
- R
- RStudio
- R Markdown
- Black-Scholes option pricing
- Geometric Brownian Motion
- Monte Carlo simulation
- Sensitivity analysis
- Knitr for reporting


## 8. How to run
Requirements: R and the `rmarkdown` and `knitr` packages

```r
install.packages(c("rmarkdown", "knitr"))
rmarkdown::render("coffee_pricing.Rmd", output_format = "github_document")
```
The rendered report is also available as Pricing_commoditities_-_Coffee.pdf


## 9. Project Files
![R Markdown Source Code](Coffee_pricing.Rmd)
![Full Analysis Report (PDF)](Coffee_pricing.pdf)


## 10. Repository Structure
```
coffee-price-modeling-R/
├── README.md
├── Coffee_pricing.Rmd
├── Coffee_pricing.pdf
└── figures/
    ├── spot_sensitivity.png
    └── volatility_sensitivity.png

```

## Author
Emmanuelle K.
