# Generate a Volatility Surface (from synthetic option prices)

This notebook builds an **implied volatility surface** end-to-end using **synthetic market option prices**. The point is to learn the pipeline clearly.

What you build:
1. **Generate synthetic call option prices** on a strike × maturity grid using a “true” volatility surface.
2. Implement **Black–Scholes call pricing** from scratch.
3. Recover **implied volatility** at each grid point using a **bracketing root-finder (bisection)**.
4. Fit a **smooth volatility surface** with a **two-dimensional smoothing spline** (to remove bumps).
5. Plot:
   - **3D volatility surface**
   - **volatility smiles** (implied volatility versus strike, per maturity)
   - **term structure** (implied volatility versus maturity, for strikes near spot)
6. Export the grids to **Excel**.

---

## What this project does

In academic Black–Scholes, volatility is one constant number.

In practice, the market implies **different volatility** depending on:
- **Strike** (smile or skew)
- **Maturity** (term structure)

This project starts from an option price grid and backs out the implied volatility at every strike–maturity point, then smooths that grid into a clean surface you can visualize and query.

---

## Outputs you get

### Python
- **Market price grid**: call prices $C_{\mathrm{mkt}}(K,T)$
- **Implied volatility grid**: $\sigma_{\mathrm{imp}}(K,T)$
- **Smoothed surface** on a dense grid (clean 3D plot, less bumpiness)
- Plots:
  - 3D surface
  - smile curves
  - term-structure curves

### Excel
- `MarketPrices` (Synthetic prices from `TrueVol`)
- `ImpliedVol` (Recovered implied volatility grid)

---

## Methodology

### Step 1: Define inputs and grids
- Spot price: $S_0$
- Risk-free rate: $r$
- Dividend yield: $q$ (can be zero)
- Strike grid: $\{K_1,\dots,K_n\}$
- Maturity grid: $\{T_1,\dots,T_m\}$

### Step 2: Create synthetic “market” option prices
You define a “true” volatility function $\sigma_{\mathrm{true}}(K,T)$ that has:
- curvature across strike (smile/skew)
- structure across time (term structure)

Then you generate synthetic market call prices:
$$
C_{\mathrm{mkt}}(K,T) \;=\; C_{\mathrm{BS}}\!\left(S_0, K, T, r, q, \sigma_{\mathrm{true}}(K,T)\right).
$$

Optional: add small noise to mimic bid-ask / quote error (if enabled in the notebook).

### Step 3: Black–Scholes call pricing (forward direction)
Black–Scholes is the engine that maps volatility to price:
$$
C \;=\; S_0 e^{-qT} N(d_1) \;-\; K e^{-rT} N(d_2),
$$
$$
d_1 \;=\; \frac{\ln(S_0/K) + \left(r-q + \frac{1}{2}\sigma^2\right)T}{\sigma\sqrt{T}},
\qquad
d_2 \;=\; d_1 - \sigma\sqrt{T}.
$$

This function is called repeatedly by the implied-vol solver.

### Step 4: Implied volatility via bisection (inverse direction)
For each market price $C_{\mathrm{mkt}}(K,T)$, implied volatility solves:
$$
f(\sigma) \;=\; C_{\mathrm{BS}}(\sigma; S_0, K, T, r, q) \;-\; C_{\mathrm{mkt}} \;=\; 0.
$$

The notebook uses **bisection**:
- pick a low $\sigma_{\mathrm{low}}$ and high $\sigma_{\mathrm{high}}$
- ensure the root is bracketed (sign change)
- repeatedly halve the interval until the pricing error is near zero

This is why the solution is stable: bisection is slow but dependable.

Important: this approach **does not use vega** because it does not use Newton–Raphson.

### Step 5: Smooth the implied vol grid using a 2D smoothing spline
Raw implied-vol grids can have small bumps (even with synthetic data, and especially if noise is added).

So the notebook fits a smooth function over:
- **moneyness** $M = K / S_0$
- **maturity** $T$

That gives a surface:
$$
\sigma_{\mathrm{smooth}}(M,T).
$$

Then it evaluates that spline on a dense $(K,T)$ grid for a clean plot.

### Step 6: Visualize
You generate:
- **3D surface**: $(K, T) \mapsto \sigma$
- **Smile**: for each maturity $T$, plot $\sigma$ versus strike $K$
- **Term structure**: for a few strikes near spot, plot $\sigma$ versus maturity $T$

### Step 7: Export to Excel
The notebook writes an `.xlsx` workbook containing:
- the synthetic market prices
- the implied vol grid

---

## Requirements
- `numpy`
- `pandas`
- `scipy`
- `plotly`
- `openpyxl`

Install:
```bash
pip install numpy pandas scipy plotly openpyxl
