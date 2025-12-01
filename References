
## 1) Black–Scholes option pricing (the forward map: volatility → price)

1. **Black, F. and Scholes, M. (1973).** “The Pricing of Options and Corporate Liabilities.” *Journal of Political Economy*, 81(3), 637–654.
   Used for the core European call pricing formula and the (d_1, d_2) structure.

2. **Merton, R. C. (1973).** “Theory of Rational Option Pricing.” *Bell Journal of Economics and Management Science*, 4(1), 141–183.
   Same framework, more general continuous-time derivation.

3. **Hull, J. C.** *Options, Futures, and Other Derivatives* (any recent edition).
   Practical treatment of Black–Scholes inputs, discounting, dividends, and how volatility is quoted and used by practitioners.

## 2) Implied volatility (the inverse map: price → volatility)

4. **Hull, J. C.** *Options, Futures, and Other Derivatives* (chapter(s) on implied volatility and volatility smiles/surfaces).
   Explains why implied volatility exists, how it is backed out, and why smiles and term structure show up in real markets.

5. **Jäckel, P. (2015).** *Let’s Be Rational* (book / notes).
   Focused on implied volatility inversion as a numerical problem, including stability and practical edge cases.

6. **Brenner, M. and Subrahmanyam, M. G. (1988).** “A Simple Formula to Compute the Implied Standard Deviation.” *Financial Analysts Journal*.
   Practical approximations and intuition for implied volatility (often cited as the trader-intuition side).

## 3) Root-finding / numerical methods (how the code solves for implied volatility)

The notebook’s implied volatility solver is a classic “solve (f(\sigma)=0)” problem where
(f(\sigma) = C_{\text{Black–Scholes}}(\sigma) - C_{\text{market}}).

7. **Press, W. H., Teukolsky, S. A., Vetterling, W. T., Flannery, B. P.** *Numerical Recipes* (various editions).
   Standard reference for bisection, Newton’s method, convergence, bracketing, and when Newton fails.

8. **Burden, R. L. and Faires, J. D.** *Numerical Analysis* (various editions).
   Clear, formal treatment of the bisection method (guaranteed convergence if you bracket) and Newton’s method.

9. **Nocedal, J. and Wright, S. J.** *Numerical Optimization*.
   Useful if you later extend to more robust solvers, line search, and stability logic.

### Vega and Newton (important nuance)

* If you use **Newton–Raphson**, it uses **vega** as (f'(\sigma)). Vega itself is covered in:

10. **Hull, J. C.** (the “Greeks” section).
11. **Castagna, A.** *FX Options and Smile Risk*.
    Very practical discussion of implied volatility, smile, and sensitivities in a trading context.

(And yes: if that UCLA note is doing Newton updates, then it is using vega implicitly as the derivative term. If it only brackets and bisects, then no vega.)

## 4) Volatility surfaces: smile, skew, and term structure (why the surface exists)

12. **Gatheral, J. (2006).** *The Volatility Surface: A Practitioner’s Guide*.
    The go-to reference for surface behavior, how practitioners think about it, and common parameterizations.

13. **Fengler, M. R. (2005).** *Semiparametric Modeling of Implied Volatility*.
    Directly about building surfaces from discrete option quotes and smoothing them in a principled way.

14. **Dumas, B., Fleming, J., Whaley, R. E. (1998).** “Implied Volatility Functions: Empirical Tests.” *Journal of Finance*.
    Empirical behavior of implied volatility across strikes/maturities and why constant volatility is wrong.

## 5) Interpolation / smoothing (how you turn a grid into a smooth surface)



15. **Wahba, G. (1990).** *Spline Models for Observational Data*.
    The classic reference on smoothing splines and avoiding overfitting bumps.

16. **de Boor, C.** *A Practical Guide to Splines*.
    Practical spline construction and evaluation details.

17. **Dierckx, P.** (FITPACK / smoothing splines literature).
    Underlies many spline implementations used in scientific computing.

## 6) Optional but highly relevant if you want a “good” surface (no-arbitrage constraints)

The current “build a smooth surface” approach is fine for learning, but production surfaces usually enforce no-arbitrage shape constraints.

18. **Carr, P. and Madan, D. (1999).** “Option Valuation Using the Fast Fourier Transform.” *Journal of Computational Finance*.
    Often cited around surface construction, pricing consistency, and transforms.

19. **Breeden, D. T. and Litzenberger, R. H. (1978).** “Prices of State-Contingent Claims…” *Journal of Business*.
    Connects option prices to implied risk-neutral distributions; motivates why arbitrage-free surfaces matter.

20. **Roper, M. (2010)** and related papers on arbitrage-free interpolation of implied volatility.
    If you later want interpolation that avoids calendar and butterfly arbitrage.


