[Think Stats Chapter 8 Exercise 3](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77)

In games like hockey and soccer, the time between goals is roughly exponential. So you could estimate a team’s goal-scoring rate by observing the number of goals they score in a game. This estimation process is a little different from sampling the time between goals, so let’s see how it works.

Write a function that takes a goal-scoring rate, lam, in goals per game, and simulates a game by generating the time between goals until the total time exceeds 1 game, then returns the number of goals scored.

Write another function that simulates many games, stores the estimates of lam, then computes their mean error and RMSE.

Is this way of making an estimate biased?

Given by the question:
```python
def SimulateGame(lam):
    """Simulates a game and returns the estimated goal-scoring rate.

    lam: actual goal scoring rate in goals per game
    """
    goals = 0
    t = 0
    while True:
        time_between_goals = random.expovariate(lam)
        t += time_between_goals
        if t > 1:
            break
        goals += 1

    # estimated goal-scoring rate is the actual number of goals scored
    L = goals
    return L
```
Other Given Functions RMSE and MeanError:

```python
def RMSE(estimates, actual):
    """Computes the root mean squared error of a sequence of estimates.

    estimate: sequence of numbers
    actual: actual value

    returns: float RMSE
    """
    e2 = [(estimate-actual)**2 for estimate in estimates]
    mse = np.mean(e2)
    return np.sqrt(mse)
```

```python
def MeanError(estimates, actual):
    """Computes the mean error of a sequence of estimates.

    estimate: sequence of numbers
    actual: actual value

    returns: float mean error
    """
    errors = [estimate-actual for estimate in estimates]
    return np.mean(errors)
```


Jacky's Answer:
```python
def SimulateManyGames(n=1000, lam=2):
    goals_scored = []
    
    for _ in range(n):
        goals = SimulateGame(lam)
        goals_scored.append(goals)
    
    standard_error = RMSE(goals_scored, lam)
    mean_error = MeanError(goals_scored, lam)
    print("Standard Error ", standard_error)
    print("Mean Error ", mean_error)
    
    pmf = thinkstats2.Pmf(goals_scored)
    thinkplot.Hist(pmf)
    thinkplot.Config(xlabel='Goals score', ylabel='pmf')
    
SimulateManyGames()
```
![image1](https://github.com/jackywlu/dsp/blob/master/lessons/statistics/Exercise%208-3%20Image%201.png)

```python
SimulateManyGames(n=100000)
```
![image2](https://github.com/jackywlu/dsp/blob/master/lessons/statistics/Exercise%208-3%20Image%202.png)


The mean error with 1000 games is -0.066, which is close to zero. The mean error with 100000 games is -0.01414, which is even closer to zero. Since this mean error decreases as n increases, this way of making an estimate appears unbiased.
