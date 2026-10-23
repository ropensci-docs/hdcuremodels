# Non-parametric test for a non-zero cured fraction

Tests the null hypothesis that the proportion of observations
susceptible to the event = 1 against the alternative that the proportion
of observations susceptible to the event is \< 1. If the null hypothesis
is rejected, there is a significant cured fraction.

## Usage

``` r
nonzerocure_test(object, reps = 1000, seed = NULL, plot = FALSE, b = NULL)
```

## Arguments

- object:

  a `survfit` object.

- reps:

  number of simulations on which to base the p-value (default = 1000).

- seed:

  optional random seed.

- plot:

  logical. If TRUE a histogram of the estimated susceptible proportions
  over all simulations is produced.

- b:

  optional. If specified the maximum observed time for the uniform
  distribution for generating the censoring times. If not specified, an
  exponential model is used for generating the censoring times
  (default).

## Value

- proportion_susceptible:

  estimated proportion of susceptibles

- proportion_cured:

  estimated proportion of those cured

- p_value:

  p-value testing the null hypothesis that the proportion of
  susceptibles = 1 (cured fraction = 0) against the alternative that the
  proportion of susceptibles \< 1 (non-zero cured fraction)

- time_95_percent_of_events:

  estimated time at which 95 events should have occurred

## References

Maller, R. A. and Zhou, X. (1996) *Survival Analysis with Long-Term
Survivors*. John Wiley & Sons.

## See also

[`survfit`](https://rdrr.io/pkg/survival/man/survfit.html),
[`cure_estimate`](https://docs.ropensci.org/hdcuremodels/reference/cure_estimate.md),
[`sufficient_fu_test`](https://docs.ropensci.org/hdcuremodels/reference/sufficient_fu_test.md)

## Examples

``` r
library(survival)
withr::local_seed(1234)
temp <- generate_cure_data(n = 100, j = 10, n_true = 10, a = 1.8)
training <- temp$training
km_fit <- survfit(Surv(Time, Censor) ~ 1, data = training)
nonzerocure_test(km_fit)
#> $proportion_susceptible
#> [1] 0.4668152
#> 
#> $proportion_cured
#> [1] 0.5331848
#> 
#> $p_value
#> [1] 0.139
#> 
#> $time_95_percent_of_events
#> [1] 4.146257
#> 
```
