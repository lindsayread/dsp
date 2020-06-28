[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)


`firsts = live[live.birthord == 1]`

`others = live[live.birthord != 1]`

`print(firsts.totalwgt_lb.mean())`

`print(others.totalwgt_lb.mean())`

`CohenEffectSize(firsts.totalwgt_lb, others.totalwgt_lb)`

`print(firsts.prglngth.mean())`

`print(others.prglngth.mean())`

`CohenEffectSize(firsts.prglngth, others.prglngth)`

## Findings: There is a much stronger correlation between birth order and total weight than there is a correlation between birth order and pregnancy length. Based on this data, we can conclude that although there is not a strong correlation of first born babies being born later, there is a correlation of first born babies weighing less at birth than proceeding babies.
