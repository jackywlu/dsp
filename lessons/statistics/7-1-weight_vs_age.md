[Think Stats Chapter 7 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2008.html#toc70) (weight vs. age)

Using data from the NSFG, make a scatter plot of birth weight versus mother’s age. Plot percentiles of birth weight versus mother’s age. Compute Pearson’s and Spearman’s correlations. How would you characterize the relationship between these variables?

```python
import first

live, firsts, others = first.MakeFrames()
live = live.dropna(subset=['agepreg', 'totalwgt_lb'])
```

Jacky's answer:
```python
mother_age, birth_weight = live.agepreg, live.totalwgt_lb
thinkplot.Scatter(mother_age, birth_weight, alpha=0.1, s=10)
thinkplot.Config(xlabel='Mother\'s Age',
                 ylabel='Birth Weight (lb)',
                 legend=False)
```
![image1](https://github.com/jackywlu/dsp/blob/master/lessons/statistics/Exercise%207-1%20Image%201.png)

```python
bins = np.arange(10, 42, 3)
indices = np.digitize(live.agepreg, bins)
groups = live.groupby(indices)

mother_ages = [group.agepreg.mean() for i, group in groups]
cdfs = [thinkstats2.Cdf(group.totalwgt_lb) for i,group in groups]
```

```python
for percent in [75, 50, 25]:
    birth_weights = [cdf.Percentile(percent) for cdf in cdfs]
    label = '%dth' % percent
    thinkplot.Plot(mother_ages, birth_weights, label=label)

thinkplot.Show(xlabel='Mother\'s Age', ylabel='Birth Weights (lb)',legend=True)
```

![image2](https://github.com/jackywlu/dsp/blob/master/lessons/statistics/Exercise%207-1%20Image%202.png)

```python
thinkstats2.Corr(mother_age, birth_weight)
```
Output:
0.0688339703541091

```python
thinkstats2.SpearmanCorr(mother_age, birth_weight)
```

Output:
0.09461004109658226

The scatterplot doesn't show much of a relationship between the variables. There may be a slight increase in birth weights as the mother's age increases, but it's incredibly weak. 

This is backed up by the low Pearson and Sperman correlation numbers. Considering the maximum value of these correlations is 1, a value of 0.06 or 0.09 is pretty low. The correlation is positive, which means that there's a slight increase in birth weights as the mother's age increases.

Going by the percentile plot, it looks like there may be a linear relationship between age and weight when the mother is younger (between 15 and 20). There isn't much of a relationship between ages 25 and 35, and there may be a slight decrease between age and weight when the mother is older (between 35 and 40).

