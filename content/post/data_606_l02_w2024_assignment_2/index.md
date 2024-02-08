---
title: '# Assignment 2'
date: '2024-02-07'
---


1. Produce a [stem plot](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.stem.html) of the probability mass function of the negative binomial distribution $\operatorname{NB}(r, p)$ (see [scipy.stats.nbinom](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.nbinom.html)) for $r=2$, $p=0.4$. Use `np.arange(30)` for the horizontal axis.

2. The negative binomial distribution $\operatorname{NB}(r, p)$ is a discrete probability distribution that models the number of failures in a sequence of independent and identically distributed Bernoulli trials with parameter $p$ before $r$ successes occur.

  Write a function `sample_from_nb` (see below) that returns a sample of size $n$ drawn from $\operatorname{NB}(r, p)$ using the above generative process. Half points for correct solutions using a `for` loop. (Obviously, you may not use `scipy.stats.nbinom.rvs` for this problem.)

  ```python
  def sample_from_nb(r, p, n, random_state=None):
    if random_state == None:
      random_state = np.random.default_rng()
    ...
  ```

3. Make a stem plot of the empirical probabilities of drawing $0, 1, \ldots, 30$
   from $\operatorname{NB}(r, p)$ for $r=5$, $p=0.4$ computed from a sample of size $n=10,000$ generated using your function `sample_from_nb`. Compare with the plot from (1). (If it doesn't match fairly closely, there's a problem with your `sample_from_nb` function.)

4. (Agnesti, Problem 1.9) In an experiment on chlorophyll inheritance in maize, for 1103 seedlings of self-fertilized, heterozygous plants, 854 seedlings were green and 249 were yellow. Theory predicts a ratio of $3:1$. test the hypothesis that $3:1$ is the correct ratio. Report the $P$-value and interpret.

5. (Agnesti, Problem 1.10) The table below contains Ladislaus von Bortiewicz's data on deaths of soldiers in the Prussian army from kicks by army mules (Fisher 1934, Quine and Seneta 1987). The data refer to 10 army corps, each observed for 20 years. in 109 corps-years of exposure, there were no deaths, in 65 corps-years, there was one death, and so on. Estimate the mean and test whether probabilities of occurrences in these five categories follow a Poisson distribution (truncated for $5$ and above).

| Number of Deaths | Number of Corps-Years |
| ---------------- | --------------------- |
| $0$              | $109$                 |
| $1$              | $65$                  |
| $2$              | $22$                  |
| $3$              | $3$                   |
| $4$              | $1$                   |
| $\geq 5$         | $0$                   |
