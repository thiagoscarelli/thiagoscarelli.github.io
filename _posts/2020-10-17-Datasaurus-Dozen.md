---
title: Summary Statistics and the Datasaurus Dozen
layout: post
author: Thiago Scarelli
lang: en_US
tags:
  - Statistics
  - R
---

Summary statistics are great tools: they reduce a potentially large number of observations down to a single number that is easy to interpret and communicate. This feature justifies the widespread popularity of averages and correlations, from introductory statistics courses and newspaper articles to scientific papers. The caveat is that they are usually insufficient to tell the whole history, as it is nicely illustrated by a group of datasets known as the "datasaurus dozen".

<!--more-->
<br>

##### Overview of the datasets

The original datasaurus was designed by Alberto Cairo as a toy example to emphasize the importance of plotting the data (see the source on [thefunctionalart.com](http://www.thefunctionalart.com/2016/08/download-datasaurus-never-trust-summary.html)). The dataset has only two variables (x and y), and their summary statistics are not particularly interesting.

In fact, if you were to imagine that those 142 observations come from a bivariate normal distribution, their summary statistics could well suggest something like this:

And yet this is the unexpected scatter plot of the datasaurus:

Inspired by this example, Justin Matejka and George Fitzmaurice (see [this page](https://www.autodesk.com/research/publications/same-stats-different-graphs)) extended the idea and built another 12 datasets (a.k.a. "the datasaurus dozen"), all of them sharing nearly the same basic summary statistics but representing a wide variety of (x, y) patterns:

In real-world applications, one should not expect to plot the data hoping that some extinct reptile will pop up. The takeaway from those examples is that the usual summary statistics can describe a given aspect of the variable of interest but may fail to disentangle more complex patterns.

If you want to explore this data further, it is available in the standalone datasauRus package (datasauRus::datasaurus_dozen, no OpenIntro affiliation).

##### Extension: How to find different patterns of data that have common statistical properties

The experienced reader will notice that these sets are similar in spirit to the classic Anscombe's Quartet. This "modern" version is more appealing in the sense that it has more points per set (142 in the datasaurus' case versus 11 in Anscombe's), making the statistical coincidence more striking to the student. The datasaurus dozen also makes it more salient that the limitation of the summary measures is not simply due to an insufficiently small number of points.

While it is unknown how Anscombe came up with his sets, the authors' of the datasaurus dozen explain that their strategy was based on bounded randomization combined with simulated annealing optimization (see [details here](https://damassets.autodesk.net/content/dam/autodesk/research/publications-assets/pdf/same-stats-different-graphs.pdf)). In a nutshell: start from a given dataset (i.e., the dino), subject it to some random variation, and then check that (1) the statistical properties of interest remain sufficiently similar, and (2) the set of new points are closer to a prespecified target pattern (e.g., a circle or a star). If both criteria are satisfied, the new set is accepted, and the process is repeated (for these datasets, the authors report some 200,000 iterations).

The "simulated annealing" part means that the process is calibrated in a way that the early interactions have some chance of being accepted even if the criterion (2) is not satisfied, and the protocol becomes gradually more stringent as it converges to a minimization result. The analogy with the annealing process in metallurgy comes from this controlled "cooling" embedded in the optimization rule, which helps the iteration process avoid being stuck in a local minimum early on (see the [Wikipedia page on simulated annealing](https://en.wikipedia.org/wiki/Simulated_annealing)).

<br>
<hr>
