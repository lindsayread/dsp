[Think Stats Chapter 6 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2007.html#toc60) (household income)

Compute the median, mean, skewness and Pearson’s skewness of the resulting sample. What fraction of households report a taxable income below the mean? How do the results depend on the assumed upper bound?

`print(Median(sample))`

`print(Mean(sample))`

Output: 
`74278.70753118739`, `51226.45447894046`

`print(Skewness(sample), PearsonMedianSkewness(sample))`

Output: `(4.949920244429579, 0.7361258019141795)`

`cdf.Prob(Mean(sample))`

Output: `0.660005879566872`

Approximately 2/3 of households report a taxable income below the mean, assuming the highest income is 10^6 (one million dollars). There are definitely people that make more than one million dollars, so if the upper bound increased, the mean would also increase, and so even a larger portion of households would report a taxable income below the mean. If the upper bound decreased, the mean would also decrease, and there would likely be a lower fraction of households reporting a taxable income below the mean.
