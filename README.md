# Options-Maxentropy
Given price of calls and underlying, find the probability distribution with maximal (differential) entropy such that the expected net profit of any transaction at midpoint price is zero (i.e. satisfying martingality).

## How?
Let $k_i$ be the $n$ different strike prices in increasing order. The Lagrangian of the problem is:
$$\int_{\mathbb{R}^+} \left(-p(x)\ln(p(x)) + \lambda_c p(x) + \lambda_s xp(x) + \sum_{i}\lambda_i\max(x-k_i,0)p(x) \right)dx$$

Gradient of Lagrangian wrt $p$ is zero at the maxima:
$$-(\ln(p(x)) + 1) + \lambda_c + \lambda_0 x + \sum_{i=1}^n\lambda_i\max(x-k_i,0) = 0$$

So the probability distribution (whose support is $\mathbb{R}^+$) is of the form
$$p(x) = \exp(C + \lambda_0 x + \sum_{i=1}^n\lambda_i\max(x-k_i,0))$$
where $\exp(C)$ is the normalisation constant.

In other words, the surprisal is some piecewise linear function satisfying the martingality constraints. The slope discontinuities occur at the strike prices.

Note that for the RHS to integrate to a finite value we require $\sum_{i=0}^n\lambda_i < 0$.

Letting $k_0=0$ and $k_{n+1}=\infty$, and denoting the prices of the call options by $C_i$ (with stock price being $C_0$), we can derive the particular constraints:

$$\sum_{i=0}^n \int_{k_i}^{k_{i+1}} \exp(C + \sum_{j\leq i} \lambda_j(x-k_j))dx = 1$$
$$\sum_{i=m}^n \int_{k_i}^{k_{i+1}} x\exp(C + \sum_{j\leq i} \lambda_j(x-k_j))dx = C_m$$

Evaluating,

$$\sum_{i=0}^n \left(\sum_{j\leq i} \lambda_j\right)\left[\exp(C + \sum_{j\leq i} \lambda_j(x-k_j))\right]_{k_i}^{k_{i+1}} = 1$$
$$\sum_{i=0}^n \left(\sum_{j\leq i} \lambda_j\right)\left(\exp(C + \sum_{j\leq i} \lambda_j(k_{i+1}-k_j)) - \exp(C + \sum_{j \leq i} \lambda_j(k_i - k_j))\right) = 1$$

This telescopes.
