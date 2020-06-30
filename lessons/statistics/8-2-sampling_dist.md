[Think Stats Chapter 8 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77) (scoring)

`Estimate3(n=10, iters=1000)`

Output: 

`rmse L 0.8572895216737223`

`rmse Lm 1.6533906658188489`

`mean error L 0.2360903568646397`

`mean error Lm 0.699641263495729`

Input:

`xbars1 = SimulateSample(mu=24, sigma=1.4, n=10, iters=1000)`

`cdf1 = thinkstats2.Cdf(xbars1)`

`thinkplot.Cdf(cdf1)`

`thinkplot.Config(xlabel='Sample mean', ylabel='CDF')`

`stderr1 = RMSE(xbars1, 90)`

`ci_sample1 = cdf1.Percentile(5), cdf1.Percentile(95)`

`print(stderr1)`

`print(ci_sample1)`

Output:

`66.01529356383227`

`(86.04130631564871, 24.67699067285868)`

Input:

`xbars2 = SimulateSample(mu=24, sigma=1.4, n=100, iters=1000)`

`cdf2 = thinkstats2.Cdf(xbars2)`

`thinkplot.Cdf(cdf2)`

`thinkplot.Config(xlabel='Sample mean', ylabel='CDF')`

`stderr2 = RMSE(xbars2, 90)`

`ci_sample2 = cdf2.Percentile(5), cdf2.Percentile(95)`

`print(stderr2)`

`print(ci_sample2)`

Output:

`66.00221699808903`

Input:

`xbars3 = SimulateSample(mu=24, sigma=1.4, n=500, iters=1000)`

`cdf3 = thinkstats2.Cdf(xbars3)`

`thinkplot.Cdf(cdf3)`

`thinkplot.Config(xlabel='Sample mean', ylabel='CDF')`

`stderr3 = RMSE(xbars3, 90)`

`ci_sample3 = cdf3.Percentile(5), cdf3.Percentile(95)`

`print(stderr3)`

`print(ci_sample3)`

Output:

`66.00044212559132`

Input:

`# standard error vs. n`

`import matplotlib.pyplot as plt`

`x_values = [stderr1, stderr2, stderr3]`

`y_values = [10, 100, 500]`

`plt.plot(x_values, y_values)`
