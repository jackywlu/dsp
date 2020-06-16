[Think Stats Chapter 8 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77) (scoring)

Suppose you draw a sample with size n=10 from an exponential distribution with λ=2. Simulate this experiment 1000 times and plot the sampling distribution of the estimate L. Compute the standard error of the estimate and the 90% confidence interval.

Repeat the experiment with a few different values of n and make a plot of standard error versus n.

Jacky's Answer:

```python
estimates = []
for _ in range(1000):
    xs = np.random.exponential(scale=1/2, size=10)
    L = 1 / np.mean(xs)
    estimates.append(L)
    
cdf = thinkstats2.Cdf(estimates)
thinkplot.Cdf(cdf)
thinkplot.Config(xlabel='Standard Error',ylabel='CDF')
```
![image1](https://github.com/jackywlu/dsp/blob/master/lessons/statistics/Exercise%208-2%20Image%201.png)


```python
standard_error = RMSE(estimates, lam)
standard_error
```
Output: 0.8876438495251074

```python
confidence_interval = cdf.Percentile(5), cdf.Percentile(95)
confidence_interval
```
Output: (1.2775027964824457, 3.7275077164323904)

```python
def MakeSample(lam=2, n=10, m=1000):

    estimates = []
    for _ in range(m):
        xs = np.random.exponential(1/lam, n)
        L = 1 / np.mean(xs)
        estimates.append(L)
    
    cdf = thinkstats2.Cdf(estimates)
    thinkplot.Cdf(cdf)
    thinkplot.Config(xlabel='Standard Error',ylabel='CDF')
    
    standard_error = RMSE(estimates, lam)
    print("Standard Error ", standard_error)
    
    confidence_interval = cdf.Percentile(5), cdf.Percentile(95)
    print("Confidence Interval ", confidence_interval)
```

```python
MakeSample(n=100)
```
Output:
Standard Error  0.2084427726257259
Confidence Interval  (1.7147591832596503, 2.385213465010694)

![image2](https://github.com/jackywlu/dsp/blob/master/lessons/statistics/Exercise%208-2%20Image%202.png)

```python
MakeSample(n=500)
```
Output:
Standard Error  0.08943840657937228
Confidence Interval  (1.8650145035721752, 2.161561023935328)

![image3](https://github.com/jackywlu/dsp/blob/master/lessons/statistics/Exercise%208-2%20Image%203.png)

```python
MakeSample(n=1000)
```
Output:
Standard Error  0.06476597270051543
Confidence Interval  (1.8958874185173813, 2.1067136974217537)

![image4](https://github.com/jackywlu/dsp/blob/master/lessons/statistics/Exercise%208-2%20Image%204.png)

```python
thinkplot.Scatter([10, 100, 500, 1000],[0.8876438495251074,0.2084427726257259,0.08943840657937228,0.06476597270051543])
thinkplot.Config(xlabel='Number of samples',ylabel='Standard Error')
```

![image5](https://github.com/jackywlu/dsp/blob/master/lessons/statistics/Exercise%208-2%20Image%205.png)

|Sample Size | Standard Error | 90% Confidence Interval|
|---|---|---|
|10 | 0.8876438495251074 | (1.2775027964824457, 3.7275077164323904) |
|100 | 0.2084427726257259 | (1.7147591832596503, 2.385213465010694) |
|500 | 0.08943840657937228 | (1.8650145035721752, 2.161561023935328) |
|1000 | 0.06476597270051543 | (1.8958874185173813, 2.1067136974217537) |

As the number of samples increases, standard error decreases, but at an exponential rate. The confidence interval also shrinks in size.
