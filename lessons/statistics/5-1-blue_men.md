[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

In the BRFSS (see Section 5.4), the distribution of heights is roughly normal with parameters µ = 178 cm and σ = 7.7 cm for men, and µ = 163 cm and σ = 7.3 cm for women.
In order to join Blue Man Group, you have to be male between 5’10” and 6’1” (see http://bluemancasting.com). What percentage of the U.S. male population is in this range? Hint: use scipy.stats.norm.cdf.
scipy.stats contains objects that represent analytic distributions

`import scipy.stats`

`mu = 178`

`sigma = 7.7`

`dist = scipy.stats.norm(loc=mu, scale=sigma)`

`type(dist)`

`dist.cdf(mu-sigma)`

## How many people are between 5'10" and 6'1"?

`five_ten = scipy.stats.norm.cdf(177.8, loc=mu, scale=sigma)`

`six_one = scipy.stats.norm.cdf(185.4, loc=mu, scale=sigma)`

`six_one - five_ten`

Output: `0.3420946829459531`

Approximately 34% of the U.S. male population is between 5'10" and 6'1".
