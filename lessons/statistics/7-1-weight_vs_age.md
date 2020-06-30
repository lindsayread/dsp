[Think Stats Chapter 7 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2008.html#toc70) (weight vs. age)

`import first`

`live, firsts, others = first.MakeFrames()`
`live = live.dropna(subset=['agepreg', 'totalwgt_lb'])`

`mothers_age = live.agepreg`
`birth_weight = live.totalwgt_lb`

`thinkplot.Scatter(birth_weight, mothers_age)`
`thinkplot.Config(xlabel='Weight (lb)',
                 ylabel='Mothers Age',
                 legend=False)`
                 
`thinkplot.HexBin(birth_weight, mothers_age)`
`thinkplot.Config(xlabel='Height (cm)',
                 ylabel='Weight (kg)',
                 legend=False)`
                 
## Pearson's Correlation:
`Corr(birth_weight, mothers_age)`

## Spearman's Correlation:
`SpearmanCorr(birth_weight, mothers_age)`

## Pearson's correlation with log-weight and height:
`Corr(mothers_age, np.log(birth_weight))`

## Conclusions:
Upon analysis of Pearson's and Spearman's Correlations, we can conclude that Spearman's Correlation more closely represents the relationship of birth weight vs. mother's age. This implies that the relationship is NOT linear.  Although Spearman's Correlation is a better representation, the relationship does not yield a strong correlation (Spearman's Correlation for these variables is 0.946 while a strong correlation is considered to be >=0.7). Since the correlation is positive, it indicates (ever so slightly) that the higher the birthweight, the higher the mother's age.
