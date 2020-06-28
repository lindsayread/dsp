[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

>> REPLACE THIS TEXT WITH YOUR RESPONSE

resp = nsfg.ReadFemResp()

num_of_kids = thinkstats2.Pmf(resp.numkdhh, label='actual')

thinkplot.Hist(num_of_kids)
thinkplot.Config(xlabel='Number of Children under 18 in respondents household', ylabel='Pmf')

biased_kids = BiasPmf(num_of_kids, label='observed')
thinkplot.PrePlot(2)
thinkplot.Pmfs([num_of_kids, biased_kids])
thinkplot.Config(xlabel='Number of Kids in Household', ylabel='PMF')

print(num_of_kids.Mean())

print(biased_kids.Mean())
