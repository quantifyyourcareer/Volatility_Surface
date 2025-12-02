## Notes
This notebook is educational. It does not enforce no-arbitrage constraints.
Production volatility surfaces often impose constraints to avoid calendar-spread or butterfly arbitrage.
Real market data requires extra care: forwards, discount curves, dividends, day count, quote conventions, and filtering stale quotes.

## References 

### 1) Black–Scholes option pricing (volatility to price)

1. **Black, F. and Scholes, M. (1973).** “The Pricing of Options and Corporate Liabilities.” *Journal of Political Economy*, 81(3), 637–654.
    
    European option pricing formula, d1d1, d2d2.
    
2. **Merton, R. C. (1973).** “Theory of Rational Option Pricing.” *Bell Journal of Economics and Management Science*, 4(1), 141–183.
    
    Continuous-time option pricing framework and extensions.
    
3. **Hull, J. C.** *Options, Futures, and Other Derivatives* (any recent edition).
    
    Practical explanation of implied volatility, discounting, dividends, and smiles.
    

### 2) Implied volatility (price to volatility)

1. **Hull, J. C.** *Options, Futures, and Other Derivatives* (chapters on volatility and implied volatility).
    
    Why implied volatility exists and how smiles and term structure show up in markets.
    
2. **Jäckel, P. (2015).** *Let’s Be Rational* (book / notes).
    
    Implied volatility inversion as a numerical problem and practical edge cases.
    
3. **Brenner, M. and Subrahmanyam, M. G. (1988).** “A Simple Formula to Compute the Implied Standard Deviation.” *Financial Analysts Journal*.
    
    Intuition and approximations around implied volatility.
    

### 3) Root-finding used here (bisection, bracketing, convergence)

1. **Press, W. H., Teukolsky, S. A., Vetterling, W. T., Flannery, B. P.** *Numerical Recipes* (any edition).
    
    Bracketing methods, bisection, and convergence behavior.
    
2. **Burden, R. L. and Faires, J. D.** *Numerical Analysis* (any edition).
    
    Clean, formal treatment of bisection and root-finding guarantees under bracketing.
    

### 4) Volatility surfaces (smile, skew, term structure)

1. **Gatheral, J. (2006).** *The Volatility Surface: A Practitioner’s Guide*.
    
    Practitioner view of surface shape and common quoting behavior.
    
2. **Fengler, M. R. (2005).** *Semiparametric Modeling of Implied Volatility*.
    
    Building smooth surfaces from discrete implied volatility quotes.
    
3. **Dumas, B., Fleming, J., Whaley, R. E. (1998).** “Implied Volatility Functions: Empirical Tests.” *Journal of Finance*.
    
    Empirical behavior of implied volatility across strike and maturity.
    

### 5) Smoothing spline methodology used here (two-dimensional spline surface)

1. **Wahba, G. (1990).** *Spline Models for Observational Data*.
    
    Smoothing splines, regularization, and controlling overfitting.
    
2. **de Boor, C.** *A Practical Guide to Splines*.
    
    Practical spline construction and interpretation.
    
3. **Dierckx, P.** (FITPACK literature on smoothing splines).
    
    Standard basis for many scientific spline implementations.
    

### 6) Synthetic market price generation (assume a surface, price with Black–Scholes)

1. **Hull, J. C.** *Options, Futures, and Other Derivatives*.
    
    Using Black–Scholes as a mapping from volatility to price and interpreting implied volatility as a quote.
    
2. **Gatheral, J. (2006).** *The Volatility Surface: A Practitioner’s Guide*.
    
    Realistic smile and term structure shapes that motivate a “true surface” for simulated data.
    
