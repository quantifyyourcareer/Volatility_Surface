# Generate a Volatility Surface
Generate an Options Volatility Surface from synthetic market prices
# Volatility Surface Project (Python + Excel)

Build an implied-volatility surface from a grid of option prices using:
1) Black–Scholes call pricing (implemented from scratch)
2) An implied-volatility solver (root-finding)
3) Surface smoothing / interpolation
4) Visualizations: 3D surface, smile, term structure
5) A companion Excel workbook that mirrors the logic and lets you run Goal Seek

---

## What this project does (in one minute)
In real markets, implied volatility is not one constant number. It changes across:
- **Strike** (smile / skew)
- **Maturity** (term structure)

This project takes option prices across strikes and maturities, backs out the implied volatility for each (K, T) point, and then builds a smooth surface you can visualize and use.

---

## Outputs you get
### Python
- A **strike-by-maturity implied volatility grid** (a table)
- A **smoothed surface** on a dense grid (for clean plots)
- Interactive plots:
  - **3D surface**
  - **Smile** (implied volatility versus strike for fixed maturities)
  - **Term structure** (implied volatility versus time for fixed strikes)

### Excel
- An **Inputs** sheet (spot, rates, dividend, grids)
- A **TrueVol** sheet (if you’re generating synthetic prices)
- A **MarketPrices** sheet (Black–Scholes call prices)
- An **ImpliedVol** sheet (implied vol grid values)
- A **OneOption** sheet: **Goal Seek** implied volatility for ONE option (K, T)
- Charts sheet: surface + smile + term structure

---

## Methodology (what the code is doing, step-by-step)

### Step 1: Define inputs and grids
- Spot price: \(S_0\)
- Strike grid: \(K\_1, \dots, K_n\)
- Maturity grid: \(T\_1, \dots, T_m\)
- Risk-free rate: \(r\)
- Dividend yield (optional): \(q\)

### Step 2: Black–Scholes call pricing (forward direction)
Given volatility \(\sigma\), compute the call price:
\[
C = S_0 e^{-qT} N(d_1) - K e^{-rT} N(d_2)
\]
\[
d_1 = \frac{\ln(S_0/K) + (r-q + \tfrac{1}{2}\sigma^2)T}{\sigma\sqrt{T}}, \quad
d_2 = d_1 - \sigma\sqrt{T}
\]
This is the “pricing engine” the solver calls repeatedly.

### Step 3: Implied volatility (inverse direction)
For each observed market price \(C_{\text{mkt}}(K,T)\), solve:
\[
f(\sigma) = C_{\text{BS}}(\sigma; S_0,K,T,r,q) - C_{\text{mkt}} = 0
\]
The solver is a numerical root-finder:
- **Bisection** (slow but reliable) OR
- **Newton–Raphson** (fast but needs vega and can fail without safeguards)

### Step 4: Build the implied-volatility grid
Store \(\sigma_{\text{imp}}(K_i,T_j)\) in a table:
- rows = strikes
- columns = maturities

This grid is the raw surface before smoothing.

### Step 5: Smooth / interpolate the surface (to avoid bumps)
Real data is discrete and noisy, so we fit an interpolator over \((K,T)\) (or moneyness \(K/S_0\)) to get a smooth surface for plotting and for “looking up” vol at off-grid points.

### Step 6: Visualize
- **3D surface**: implied vol as a function of strike and time
- **Smile**: fix T, plot vol versus strike
- **Term structure**: fix K, plot vol versus maturity

---

## Synthetic market prices (if included)
If you don’t have real option prices, the project can generate “market” prices by:
1) Defining a “true” volatility function \(\sigma_{\text{true}}(K,T)\) that has smile + term structure
2) Pricing each option with Black–Scholes using \(\sigma_{\text{true}}\)
3) Optionally adding tiny noise to mimic bid-ask / quote errors

This is used only to create a clean learning dataset.

---

## Repo / notebook structure (typical)
- `black_scholes_call(...)`  
  Forward pricing: volatility → price
- `implied_volatility_solver(...)`  
  Inversion: price → volatility (root-finding)
- `build_implied_surface(...)`  
  Loop through (K, T) grid
- `smooth_surface(...)`  
  Interpolation / smoothing on a dense grid
- `plot_surface(...)`, `plot_smile(...)`, `plot_term_structure(...)`  
  Charts and diagnostics

---

## Requirements (Python)
Typical dependencies:
- `numpy`
- `pandas`
- `scipy` (CDF or interpolation, depending on your implementation)
- `plotly` (for interactive 3D plots) or `matplotlib` (static)
- `openpyxl` (for Excel generation)

If your notebook uses Plotly:
```bash
pip install numpy pandas scipy plotly openpyxl
