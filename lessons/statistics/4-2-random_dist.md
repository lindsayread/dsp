[Think Stats Chapter 4 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2005.html#toc41) (a random distribution)

`random_sample = np.random.random(1000)`
`random_pmf = thinkstats2.Pmf(random_sample, label='random')`
`thinkplot.Pmf(random_pmf)`

`cdf_random = thinkstats2.Cdf(random_sample, label='Random Numbers')`
`thinkplot.Cdf(cdf_random)`

When plotting the PMF of a random sample of 1000 numbers, the graph is completely colored in. This is because, regardless of what the number's value is, the number itself represents 1/1000 of the whole data set of 1000 numbers (assuming no two random numbers are exactly the same). If we examine our plot further, we see that the height of the seemingly solid black has the value 0.0010, which is 1/1000. In other words, whatever the number is (represented on the x-axis as 1000 numbers between 0 and 1), each number represents one one-thousandth of the total numbers in the set.

On the contrary, the CDF plot is essentially a uniform line. Since the CDF represents the percentage at or below a given number, it actually evaluates that number in relation to other numbers (whether it is higher or lower than other numbers). So, in a random sample of 1000 numbers between 0 and 1, if the number is 0.8000, you would expect that approximately 80% of the values to be at or below that number. This trend is confirmed by this CDF.
