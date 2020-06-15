[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

Exercise: In the BRFSS (see Section 5.4), the distribution of heights is roughly normal with parameters µ = 178 cm and σ = 7.7 cm for men, and µ = 163 cm and σ = 7.3 cm for women.

In order to join Blue Man Group, you have to be male between 5’10” and 6’1” (see http://bluemancasting.com). What percentage of the U.S. male population is in this range? Hint: use scipy.stats.norm.cdf.

From the book:

`scipy.stats` contains objects that represent analytic distributions

```Python
import scipy.stats
```

For example `scipy.stats.norm` represents a normal distributioni.

```Python
mu = 178
sigma = 7.7
dist = scipy.stats.norm(loc=mu, scale=sigma)
type(dist)
```
output is `scipy.stats._distn_infrastructure.rv_frozen`

A "frozen random variable" can compute its mean and standard deviations.

```Python
dist.mean(), dist.std()
```
output is (178.0, 7.7)

It can also evaluate its CSF> How many people are more than one standard deviations below the mean? About 16%
```
dist.cdf(mu-sigma)
```
output is 0.1586552539314574

How many people are between 5'10" and 6'1"?

Jacky's Answer:
5'10" corresponds to 177.8 cm. 6'1" corresponds to 185.42cm.

```Python
dist.cdf(185.42) - dist.cdf(177.8)
```
output: 0.3427468376314737

Around 34.3% of men meet the height requirement.

