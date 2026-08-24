# Estimate cured fraction

Estimates the cured fraction using a Kaplan-Meier fitted object.

## Usage

``` r
cure_estimate(object)
```

## Arguments

- object:

  a `survfit` object.

## Value

estimated proportion of cured observations

## See also

[`survfit`](https://rdrr.io/pkg/survival/man/survfit.html),
[`sufficient_fu_test`](https://docs.ropensci.org/hdcuremodels/reference/sufficient_fu_test.md),
[`nonzerocure_test`](https://docs.ropensci.org/hdcuremodels/reference/nonzerocure_test.md)

## Examples

``` r
library(survival)
withr::local_seed(1234)
temp <- generate_cure_data(n = 100, j = 10, n_true = 10, a = 1.8)
training <- temp$training
km_fit <- survfit(Surv(Time, Censor) ~ 1, data = training)
cure_estimate(km_fit)
#> [1] 0.535734
```
