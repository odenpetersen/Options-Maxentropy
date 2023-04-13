# Options-Maxentropy
Given price of calls and underlying, find the probability distribution with maximal (differential) entropy such that the expected net profit of any transaction at midpoint price is zero (i.e. satisfying martingality).

## How?
Lagrangian of the problem is:
$$\int_{\mathbb{R}^+} \left(-p(x)\ln(p(x)) + \lambda_c p(x) + \lambda_s xp(x) + \sum_{i}\lambda_i\max(x-k_i,0)p(x) \right)$$

Gradient of Lagrangian wrt $p$ is zero at the maxima:
$$-(\ln(p(x)) + 1) + \lambda_c + \lambda_s x + \sum_{i}\lambda_i\max(x-k_i,0) = 0$$

So the probability distribution (whose support is $\mathbb{R}^+$) is of the form
$$p(x) = exp(C + \lambda_s x + \sum_{i}\lambda_i\max(x-k_i,0))$$
where $exp(C)$ is the normalisation constant.

In other words, the surprisal is some piecewise linear function satisfying the martingality constraints. The slope discontinuities occur at the strike prices.

Note that for the RHS to integrate to one we require $\lambda_s + \sum_{i}\lambda_i < 0$.


