# Test for sufficient follow-up

Tests for sufficient follow-up using a Kaplan-Meier fitted object.

## Usage

``` r
sufficient_fu_test(object)
```

## Arguments

- object:

  a `survfit` object.

## Value

- p_value:

  p-value from testing the null hypothesis that there was not sufficient
  follow-up against the alternative that there was sufficient follow-up

- n_n:

  total number of events that occurred at time \> pmax(0, 2\*(last
  observed event time)-(last observed time)) and \< the last observed
  event time

- N:

  number of observations in the dataset

## References

Maller, R. A. and Zhou, X. (1996) *Survival Analysis with Long-Term
Survivors*. John Wiley & Sons.

## See also

[`survfit`](https://rdrr.io/pkg/survival/man/survfit.html),
[`cure_estimate`](https://docs.ropensci.org/hdcuremodels/reference/cure_estimate.md),
[`nonzerocure_test`](https://docs.ropensci.org/hdcuremodels/reference/nonzerocure_test.md)

## Examples

``` r
library(survival)
withr::local_seed(1234)
temp <- generate_cure_data(n = 100, j = 10, n_true = 10, a = 1.8)
training <- temp$training
km_fit <- survfit(Surv(Time, Censor) ~ 1, data = training)
sufficient_fu_test(km_fit)
#>        p_value n_n  N
#> 1 6.858046e-05   9 75
```
