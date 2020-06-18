[Think Stats Chapter 9 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2010.html#toc90) (resampling)

Exercise: In Section 9.3, we simulated the null hypothesis by permutation; that is, we treated the observed values as if they represented the entire population, and randomly assigned the members of the population to the two groups.

An alternative is to use the sample to estimate the distribution for the population, then draw a random sample from that distribution. This process is called resampling. There are several ways to implement resampling, but one of the simplest is to draw a sample with replacement from the observed values, as in Section 9.10.

Write a class named DiffMeansResample that inherits from DiffMeansPermute and overrides RunModel to implement resampling, rather than permutation.

Use this model to test the differences in pregnancy length and birth weight. How much does the model affect the results?

Given previously:

```python
class DiffMeansPermute(thinkstats2.HypothesisTest):
    
    def TestStatistic(self, data):
        group1, group2 = data
        test_stat = abs(group1.mean() - group2.mean())
        return test_stat
    
    def MakeModel(self):
        group1, group2 = self.data
        self.n, self.m = len(group1), len(group2)
        self.pool = np.hstack((group1, group2))
        
    def RunModel(self):
        np.random.shuffle(self.pool)
        data = self.pool[:self.n], self.pool[self.n:]
        return data
```

Jacky's Answer:

```python
class DiffMeansResample(DiffMeansPermute):
  
  def RunModel(self):
    sample1 = np.random.choice(self.pool, size=self.n, replace=True)
    sample2 = np.random.choice(self.pool, size=self.m, replace=True)
    return sample1, sample2

```

Difference in pregnancy lengths
```python
data = firsts.prglngth.values, others.prglngth.values
ht = DiffMeansResample(data)
pvalue = ht.PValue()
pvalue, ht.actual
```
Output: (0.173, 0.07803726677754952)
Note: ht.actual represents the absolute value of the difference in pregnancy length (weeks) for first-borne babies and others in the raw data.

Difference in birth weight
```python
data = firsts.totalwgt_lb.dropna().values, others.totalwgt_lb.dropna().values
ht = DiffMeansResample(data)
pvalue = ht.PValue()
pvalue, ht.actual
```
Output: (0.0, 0.12476118453549034)
Note: ht.actual represents the absolute value of the difference in first-born baby weights (lb) vs. the weights of babies born after the first-born in the raw data.

Previously, we were told the p-value for difference in pregnancy length for first babies and others is about 17%. Our resampling shows a p-value also around 17%.

It doesn't seem like sampling has much of an affect on the p-value.
