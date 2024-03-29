---
title: "The transformation issue: predicting in level after fitting in log"
layout: post
author: Thiago Scarelli
---

This one of those things everybody seems to know but you. For those who are senior statisticians, this would be so trivial they wouldn't even care to mention it, so its unlikely you'll see this in a class. For the rest of us, there's a good chance we'll commit this mistake at some point. Long story short: if you have estimated a model where the independent variable is specified in log, just exponentiating the fitted values is usually not enough to recover an unbiased retransformation.

<!--more-->

##### A quick background on the use of logs

Here's how the typical setting goes: you want to model wage as a linear function of individual observable attributes. This regression, which labor economists often call a [Mincer model](https://en.wikipedia.org/wiki/Mincer_earnings_function), would be more or less like this:

$$ log\:wage = x_1 \beta_1 + \cdots + x_k \beta_k + \epsilon $$

The first thing you should note is that people often use the *natural log of earnings* instead of the *level of earnings* in such estimations. But why would one make this weird transformation in the first place? Well, there are a few good reasons to do so:

(1) The distribution of wages is known to be skewed, with a lot of people making a bit more than the minimum wage and a few people making a lot of money. Such distribution gets much closer to a normal one when we use the log (yes, you could say the distribution of wages is close to a [lognormal distribution](https://en.wikipedia.org/wiki/Log-normal_distribution) if you want) and the estimation will be more efficient.

(2) It may prevent [heteroskedasticity](https://en.wikipedia.org/wiki/Heteroscedasticity), since it compresses the higher variance usually observed for higher wages.

(3) The partial effect of the coefficients are read as <em>relative change</em> (some beta percent difference, on average) instead of <em>jumps in level</em> (some beta dollars difference, on average). This interpretation is often more interesting and more plausible.

(4) Yet another reason is that you might be interested in predicting wages using your model. Using wages in level, one often gets both negative and positive coefficients in the estimation and, as a consequence, some predicted values could be negative for some combinations of attributes. We all agree it doesn't make much sense to predict negative earnings. Thanks to the properties of the log, this model specification avoids such inconveniences.

And the log does all that without changing the relative rank of the values: if your wage is higher than mine in level, it will certainly remain higher in log, no problem there. Thank you, [monotonic transformation](https://en.wikipedia.org/wiki/Monotonic_function).

##### Transforming and back-transforming

Now that we're convinced log is a friend, we can go on and fit the model. If you just want to interpret the marginal effects, that's probably all you need. However, if you want to use those coefficients to fit expected wages in dollars, we need to look at it a little closer.

The naïve approach is to calculate the linear combination of the individual attributes and the estimated coefficients, and exponentiate the result to transform the fitted log wage back to wage in monetary terms. Since exponentiation and log are inverse operations, that should do the trick, right? However intuitive, this procedure will lead an underestimation.

We can make use of some general notation here. Let $\widetilde{y}$ represent a non-linear transformation $f()$ of the original variable $y$:

$$ \widetilde{y} \equiv f(y) $$

A linear regression of $\widetilde{y}$ on a set of covariates $\mathbf{X}$ (with bold minuscule for vectors and bold majuscules for matrices) has that:

$$ \widetilde{\mathbf{y}} = \mathbf{X}'\boldsymbol{\beta} + \boldsymbol{\epsilon} $$

$$ \hat{\boldsymbol{\beta}} = (\mathbf{X}'\mathbf{X})^{-1} \mathbf{X}'\widetilde{\mathbf{y}} $$

We focus now on the expected value of $y_j$ for a given individual $j$. The whole issue is that, for a non-linear function $f()$:

$$ \mathbb{E}[\widetilde{y}_{j} | \mathbf{X_j}] = \mathbb{E}[(\mathbf{X_j}'\hat{\boldsymbol{\beta}} + \epsilon_j)] = \mathbb{E}[(\mathbf{X_j}'\hat{\boldsymbol{\beta}})] $$

but

$$ \mathbb{E}[y_{j} | \mathbf{X_j}] = \mathbb{E}[f^{-1}(\mathbf{X_j}'\hat{\boldsymbol{\beta}} + \epsilon_j)] \neq \mathbb{E}[f^{-1}(\mathbf{X_j}'\hat{\boldsymbol{\beta}})] $$

even if

$$ \mathbb{E}[\epsilon_j | \mathbf{X_j}] = 0 $$

Back to our particular case:

$$ \mathbb{E}[log\:wage_{j} | \mathbf{X_j}] = \mathbb{E}[(\mathbf{X_j}'\hat{\boldsymbol{\beta}} + \epsilon_j)] = \mathbb{E}[(\mathbf{X_j}'\hat{\boldsymbol{\beta}})] $$

but

$$ \mathbb{E}[wage_{j} | \mathbf{X_j}] = \mathbb{E}[\text{exp}(\mathbf{X_j}'\hat{\boldsymbol{\beta}} + \epsilon_j)] \neq \mathbb{E}[\text{exp}(\mathbf{X_j}'\hat{\boldsymbol{\beta}})] $$

Right, so what do we do then?

##### Two corrections for the log transformation issue

The good news is that it's possible to correct for the bias induced by the transformation. Following the presentation of Duan (1983), there are at least two easy strategies:

###### (a) Normal theory estimates

If you are willing to assume the error component $\epsilon_i$ is normally distributed, the untransformed scale expectation is

$$ \exp \left( xb + \frac{\sigma^2}{2} \right) $$

where $\sigma^2$ is the variance of $\epsilon_i$ and, as such, can be estimated using the empirical variance of the residuals $u_i = y - xb$

###### (b) The smearing estima

...

##### Further readings

A little note of caution, before we conclude: the discussion presented above assumes away the issue of selection that is usually present at this type of question. Wages are not something observed for a random sample of individuals, but only for those individuals who have decided to look for a job, who have indeed found a job and who are currently employed -- which is quite a particular group of people. If their earnings are correlated with the likelihood of them being employees, the coefficients would confound those things. This is a classic example of selection bias, but that is a topic for another post.

Additional aspects of estimations with transformed independent variables are discussed at [David Gile's blog](https://davegiles.blogspot.com/2014/12/s.html) and at [Stata's blog](https://blog.stata.com/2011/08/22/use-poisson-rather-than-regress-tell-a-friend/).

 and [Stata's levpredict help](https://fmwww.bc.edu/repec/bocode/l/levpredict.html).
