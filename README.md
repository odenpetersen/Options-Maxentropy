# Options-Maxentropy
Given price of calls and underlying, find the probability distribution with maximal (differential) entropy such that the expected net profit of any transaction at midpoint price is zero (i.e. satisfying martingality).

## How?
Let $k_i$ be the $n$ different strike prices in increasing order. The Lagrangian of the problem is:
$$\int_{\mathbb{R}^+} \left(-p(x)\ln(p(x)) + \lambda_N p(x) + \lambda_0 xp(x) + \sum_{i=1}^n\lambda_i\max(x-k_i,0)p(x) \right)dx$$

Gradient of Lagrangian wrt $p$ is zero at the maxima:
$$-(\ln(p(x)) + 1) + \lambda_N + \lambda_0 x + \sum_{i=1}^n\lambda_i\max(x-k_i,0) = 0$$

So the probability distribution (whose support is $\mathbb{R}^+$) is of the form
$$p(x) = \exp\left(N + \lambda_0 x + \sum_{i=1}^n\lambda_i\max(x-k_i,0)\right)$$
where $\exp(N)$ is the normalisation constant.

In other words, the surprisal is some piecewise linear function satisfying the martingality constraints. The slope discontinuities occur at the strike prices.

Note that for the RHS to integrate to a finite value we require $$\sum_{i=0}^n\lambda_i < 0.$$

Letting $k_0=0$ and $k_{n+1}=\infty$, and denoting the prices of the call options by $C_i$ (with stock price being $C_0$), we can derive the particular constraints:

$$\sum_{i=0}^n \int_{k_i}^{k_{i+1}} \exp\left(N + \sum_{j\leq i} \lambda_j(x-k_j)\right)dx = 1$$

$$\sum_{i=m}^n \int_{k_i}^{k_{i+1}} (x-k_m)\exp\left(N + \sum_{j\leq i} \lambda_j(x-k_j)\right)dx = C_m$$

Evaluating the first constraint yields

$$\sum_{i=0}^n \left(\sum_{j\leq i} \lambda_j\right)\left[\exp\left(N + \sum_{j\leq i} \lambda_j(x-k_j)\right)\right]_{k_i}^{k_{i+1}} = 1$$

$$\sum_{i=0}^n \left(\sum_{j\leq i} \lambda_j\right)\left(\exp\left(N + \sum_{j\leq i} \lambda_j(k_{i+1}-k_j)\right) - \exp\left(N + \sum_{j \leq i} \lambda_j(k_i - k_j)\right)\right) = 1$$

Evaluating the second category of constraints yields

$$\sum_{i=m}^n\left(\sum_{j\leq i}\lambda_j\right)^{-2}\left[\left(\left(\sum_{j\leq i}\lambda_j\right)(x-k_m)-1\right)\exp\left(N + \sum_{j\leq i} \lambda_j(x-k_j)\right)\right]_{k_i}^{k_{i+1}} = C_m$$

$$\sum_{i=m}^n\left(\sum_{j\leq i}\lambda_j\right)^{-2}\left(\left(\left(\sum_{j\leq i}\lambda_j\right)(k_{i+1}-k_m)-1\right)\exp\left(N + \sum_{j\leq i} \lambda_j(k_{i+1}-k_j)\right)-\left(\left(\sum_{j\leq i}\lambda_j\right)(k_i-k_m)-1\right)\exp\left(N + \sum_{j\leq i} \lambda_j(k_i-k_j)\right)\right)= C_m$$

We can solve this system of equations for the $\lambda$ vector (and for the value of $N$) using Newton's method (note: *actual* Newton's method, not Newton's method applied to the gradient). This gives the parameters of the probability distribution.

From here we can:
- Look for historical patterns of reversion, (e.g. Wasserstein, KLD)
- Use this as a prior to be updated using other features
- Compare the distributions for different expiries (e.g. Wasserstein, KLD)
- Compute local volatility surface (or other moments)
- Identify contracts that ought to have a tighter spread
- Compute the returns distribution of a particular position

The above reasoning can clearly be extended to incorporate put contract prices also. However, these shouldn't give any information not contained in the call prices, so long as put-call parity holds.

Separate distributions can be obtained using bid and ask prices at various depths.

One might alternatively consider computing the entropy with respect to a logarithmically transformed measure. My conjecture would be that this results in vanishing probability density at zero.
