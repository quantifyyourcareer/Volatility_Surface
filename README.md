# Generate a Volatility Surface (from synthetic option prices)

This notebook builds an **implied volatility surface** end-to-end using **synthetic market option prices**. The point is to learn the pipeline clearly, without relying on math formatting.

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

In academic Black–Scholes, volatility is treated as one constant number.

In practice, the market implies **different volatility** depending on:
- **Strike** (smile or skew)
- **Maturity** (term structure)

This project starts from an option price grid and backs out the implied volatility at every strike–maturity point, then smooths that grid into a clean surface you can visualize and query.

---

## Outputs you get

### Python
- **Market price grid**: a table of call prices indexed by strike and maturity
- **Implied volatility grid**: a table of implied volatilities indexed by strike and maturity
- **Smoothed surface** on a dense grid (clean 3D plot, less bumpiness)
- Plots:
  - 3D surface
  - smile curves
  - term-structure curves

### Excel
- `MarketPrices` (synthetic prices generated from the true volatility surface)
- `ImpliedVol` (recovered implied volatility grid)

---

## Methodology

### Step 1: Define inputs and grids
You set:
- Spot price (current underlying price)
- Risk-free rate
- Dividend yield (optionally zero)
- A strike grid (a list of strikes)
- A maturity grid (a list of times to expiry)

### Step 2: Create synthetic “market” option prices
You define a “true” volatility surface as a function of strike and maturity. It should deliberately create:
- curvature across strike (smile or skew)
- structure across time (term structure)

Then, for every strike and maturity pair, you compute the call option price using Black–Scholes with that true volatility.  
Optional: add small noise to mimic bid-ask spreads or quote errors.

### Step 3: Black–Scholes call pricing (forward direction)
You implement Black–Scholes from scratch as a pure function:
- inputs: spot, strike, maturity, risk-free rate, dividend yield, volatility
- output: call price

This function is called repeatedly by the implied-volatility solver.

### Step 4: Implied volatility via bisection (inverse direction)
For each synthetic market call price, implied volatility is the volatility that makes Black–Scholes reproduce that price.

The notebook uses **bisection**:
- pick a low volatility and a high volatility
- confirm the correct volatility is bracketed inside that range
- repeatedly cut the interval in half until the pricing error is tiny

Why this method: it is slower than Newton’s method but very stable and does not require derivatives (no vega).

### Step 5: Smooth the implied vol grid using a 2D smoothing spline
Raw implied-vol grids can be bumpy, especially if you added noise.

So the notebook fits a smooth surface over:
- moneyness (strike divided by spot)
- maturity

Then it evaluates that smooth surface on a dense grid to get a clean surface for plotting and querying.

### Step 6: Visualize
You generate three standard views:
- **3D surface**: implied volatility as a function of strike and maturity
- **Smile**: for each maturity, implied volatility versus strike
- **Term structure**: for strikes near spot, implied volatility versus maturity

### Step 7: Export to Excel
The notebook writes an Excel workbook containing:
- the synthetic market price grid
- the implied volatility grid

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
