[Think Stats Chapter 6 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2007.html#toc60) (household income)

The distribution of income is famously skewed to the right. In this exercise, we’ll measure how strong that skew is. The Current Population Survey (CPS) is a joint effort of the Bureau of Labor Statistics and the Census Bureau to study income and related variables. Data collected in 2013 is available from http://www.census.gov/hhes/www/cpstables/032013/hhinc/toc.htm. I downloaded hinc06.xls, which is an Excel spreadsheet with information about household income, and converted it to hinc06.csv, a CSV file you will find in the repository for this book. You will also find hinc2.py, which reads this file and transforms the data.

The dataset is in the form of a series of income ranges and the number of respondents who fell in each range. The lowest range includes respondents who reported annual household income “Under  5000.”𝑇ℎ𝑒ℎ𝑖𝑔ℎ𝑒𝑠𝑡𝑟𝑎𝑛𝑔𝑒𝑖𝑛𝑐𝑙𝑢𝑑𝑒𝑠𝑟𝑒𝑠𝑝𝑜𝑛𝑑𝑒𝑛𝑡𝑠𝑤ℎ𝑜𝑚𝑎𝑑𝑒“ 250,000 or more.”

To estimate mean and other statistics from these data, we have to make some assumptions about the lower and upper bounds, and how the values are distributed in each range. hinc2.py provides InterpolateSample, which shows one way to model this data. It takes a DataFrame with a column, income, that contains the upper bound of each range, and freq, which contains the number of respondents in each frame.

It also takes log_upper, which is an assumed upper bound on the highest range, expressed in log10 dollars. The default value, log_upper=6.0 represents the assumption that the largest income among the respondents is  106 , or one million dollars.

InterpolateSample generates a pseudo-sample; that is, a sample of household incomes that yields the same number of respondents in each range as the actual data. It assumes that incomes in each range are equally spaced on a log10 scale.
```python
def InterpolateSample(df, log_upper=6.0):
    Makes a sample of log10 household income.

    Assumes that log10 income is uniform in each range.

    df: DataFrame with columns income and freq
    log_upper: log10 of the assumed upper bound for the highest range

    returns: NumPy array of log10 household income
    
    # compute the log10 of the upper bound for each range
    df['log_upper'] = np.log10(df.income)

    # get the lower bounds by shifting the upper bound and filling in
    # the first element
    df['log_lower'] = df.log_upper.shift(1)
    df.loc[0, 'log_lower'] = 3.0

    # plug in a value for the unknown upper bound of the highest range
    df.loc[41, 'log_upper'] = log_upper
    
    # use the freq column to generate the right number of values in
    # each range
    arrays = []
    for _, row in df.iterrows():
        print(row.log_lower, row.log_upper, row.freq)
        vals = np.linspace(start=row.log_lower, stop=row.log_upper, num=int(row.freq))
        arrays.append(vals)

    # collect the arrays into a single sample
    log_sample = np.concatenate(arrays)
    return log_sample
```
```python
import hinc
income_df = hinc.ReadData()
```
```python
log_sample = InterpolateSample(income_df, log_upper=6)
```
```
sample = np.power(10, log_sample)
```

Jacky's code:
```python
Mean(sample), Median(sample), Skewness(sample), PearsonMedianSkewness(sample)
```
Ouput:
(74278.70753118733, 51226.45447894046, 4.949920244429583, 0.7361258019141782)

```python
cdf.Prob(Mean(sample))
```
Output:
0.660005879566872

Interpretation: around 66% of households have a taxable income below the mean.

To answer how the results depend on an assumed upper bound, I increased the upper bound to 7.

```Python
log_sample = InterpolateSample(income_df, log_upper=7)
sample = np.power(10, log_sample)
Mean(sample), Median(sample), Skewness(sample), PearsonMedianSkewness(sample)
```

Ouput:
(124267.39722164685,
 51226.45447894046,
 11.603690267537793,
 0.39156450927742087)
 
When the upper bound increases, it looks like the mean increased, the median stayed similar. The skewness to the right increased, but for some reason the PearsonMedianSkewness decreased.

The formula for PearsonMedianSkewness is based on 3 * (mean - median) / std. As the upper bound increases, the mean and standard deviation both increase while the median stays the same. However, it seems like the standard deviation increases more and has a stronger effect on the PearsonMedianSkewness. This is probably because it is directly in the denominator and has a stronger effect since the mean is dampened by the median.
