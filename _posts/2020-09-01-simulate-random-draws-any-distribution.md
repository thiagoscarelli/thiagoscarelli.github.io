---
layout: post
title: "How to simulate random draws from any distribution"
date: 2020-09-01
mathjax: true
tags:
  - statistics
  - R code
  - stata code
---

Simulating synthetic data is an easy way to check whether an estimator will work as intended before you have access to real-world data. In fact, experienced researchers have told me on repeated occasions that this step should be part of any research project. The key advantages are that you know exactly how the variables were generated, and your sample is as large as you need it to be. But how do you actually get random draws from a given distribution?

**Short answer:** for the most common cases, your statistical software has a function that does that. Here is a quick reference:

### 1000 random draws from a given distribution (R code)

``` r
# Uniform distribution
draws_uniform <- runif(n = 1000, min = 0, max = 1)
hist(draws_uniform)

# Normal distribution
draws_normal <- rnorm(n = 1000, mean = 0, sd = 1)
hist(draws_normal)

# Binomial distribution
draws_binomial <- rbinom(n = 1000, size = 100, prob = .1)
hist(draws_binomial)

# Exponential distribution
draws_exponential <- rexp(n = 1000, rate = 1)
hist(draws_exponential)

# Weibull distribution
draws_weibull <- rweibull(n = 1000, shape = 2, scale = 2)
hist(draws_weibull)

# Gamma distribution
draws_gamma <- rgamma(n = 1000, shape = 1, scale = 2)
hist(draws_gamma)
```

### 1000 random draws from a given distribution (Stata code)

```
// Set the number of observations
set obs 1000

// Uniform distribution with parameters a = 0 and b = 1
gen uniform = runiform(0, 1)

// Normal distribution with parameters m = 0 and s = 1
gen normal = rnormal(0, 1)

// Binomial distribution with parameters n = 100 and p = 0.1
gen binomial = rbinomial(100, 0.1)

// Exponential distribution with parameters b = 1
gen exponential = rexponential(1)

// Weibull distribution with paramters a = 1 and b = 2
gen weibull = rweibullph(1, 2)

// Gamma distribution with paramters a = 1 and b = 2
gen gamma = rgamma(1, 2)
```

**A more general answer:** If you need more flexibility, you can simulate random draws for *any arbitrary distribution*, provided that you can write down an analytical form for the inverse of its cumulative distribution function $F(\cdot)$.

You just need to take this new function $F^{-1}(\cdot)$ and evaluate it using draws from the standard uniform. This works because $x_i = F^{-1}(u_i)$ will be distributed according to the density $f(\cdot)$ for $U \sim \text{Uniform} \, \mathrm{(0, 1)}$.

### Example: Draws from an exponential distribution

Suppose you need to simulate draws from an [exponential distribution](https://en.wikipedia.org/wiki/Exponential_distribution). This distribution is a nice one because it's fully characterized by a single parameter $\lambda$ (often called the rate parameter) and it's useful in a number of applications, particularly those related with the [modeling of duration](https://en.wikipedia.org/wiki/Survival_analysis). We know that for non-negative values of $x$ its CDF is given by:

$$F(x; \lambda) = 1 - \exp(-\lambda x)$$

And the inverse of it could be written as:

$$F^{-1}(x; \lambda) = - \frac{\log(1-x)}{\lambda}$$

All you need to do is generate random draws $u_i$ from the standard [0, 1] uniform and input them in the above function to get $x_i = F^{-1}(u_i; \lambda)$, which will follow an exponential distribution of rate $\lambda$.

### Code example using R

``` r
# How many draws?
N <- 100000

# A vector of n draws from an uniform distribution
u_i <- runif(n = N, min = 0, max = 1)

# Parameter that defines the distribution of interest
lambda <- 2

# Draws from an exponential distribution using the inverse of its CDF
exp_i <- -log(1 - u_i) / lambda

# A density plot with the simulated values
library(ggplot2)
ggplot(as.data.frame(exp_i)) + geom_density(aes(x = exp_i))
```

<img class = "center" src = "/assets/images/simulation_exponential_R.png">

If you check the mean and the standard deviation of <kbd>exp_i</kbd>, you should find values close to 0.5 (or, in general, $1/\lambda$). Note too that the distribution created this way converges to what you would obtain using R's function <kbd>rexp(n = 100000, rate = 2)</kbd>. After all, what the software is doing under the hood is not much different from what we are doing explicitly.

### Code example using Stata

```
* How many draws?
set obs 100000

* Draws from an uniform distribution
gen u_i = runiform()

* Parameter that defines the distribution of interest
scalar lambda = 2

* Draws from an exponential distribution using the inverse of its CDF
gen exp_i = -log(1 - u_i) / lambda

* A density plot with the simulated values
kdensity exp_i
```
<img class = "center" src = "/assets/images/simulation_exponential_stata.png">

Again, the results are similar to what one would obtain with Stata's own <kbd>rexponential(2)</kbd>.

More importantly, however, is that the same strategy goes beyond the simple exponential case and can be easily generalized for more exotic distributions that do not have a preexisting random function available or for cases in which the default parametrization is different from the one you would need.

### Why does it work?

The CDF, by definition, maps every possible value a function can take into the interval between 0 and 1. Intuitively, the strategy above makes use of this property and revert the mapping, from the interval [0, 1] back to the support of the function. The shape of $F^{-1}(\cdot)$ alone is enough to make more frequent values of $x_i$ appear more often, even if every $u_i$ has the same probability of being used in the process.

<img class = "center" src = "/assets/images/simulation_why.png">
