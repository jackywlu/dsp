[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

Exercise 4   Using the variable totalwgt_lb, investigate whether first babies are lighter or heavier than others. Compute Cohen’s d to quantify the difference between the groups. How does it compare to the difference in pregnancy length?

```
From the pregnancy file.
preg = nsf.ReadFemPreg()
live = preg[preg.outcome == 1]
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]

Input: firsts.totalwgt_lb.mean(), others.totalwgt_lb.mean()

Output: (7.201094430437772, 7.325855614973262)

Function from ThinkStats2
def CohenEffectSize(group1, group2):
    """Computes Cohen's effect size for two groups.
    
    group1: Series or DataFrame
    group2: Series or DataFrame
    
    returns: float if the arguments are Series;
             Series if the arguments are DataFrames
    """
    diff = group1.mean() - group2.mean()

    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)

    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    d = diff / np.sqrt(pooled_var)
    return d

Input: CohenEffectSize(firsts.totalwgt_lb, others.totalwgt_lb)

Output: -0.088672927072602

```
Firstborne babies tend to be slightly lighter (7.2 lb) than babies born afterwards (7.3 lb). The difference between the two groups is 0.088 pooled standard deviations.

The mean for firstborne babies pregnancy length is 38.6 weeks while babies born afterwards have mean pregnancy length 38.5 weeks. The Cohen effect size is 0.078 pooled standard deviations.

The Cohen effect size for weight for firstborns vs. others is slightly higher than the effect size for pregnancy length.
