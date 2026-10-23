# Summarize a fitted mixture cure object

`summary` method for a mixturecure object fit using `curegmifs`,
`cureem`, `cv_curegmifs`, or `cv_cureem`.

## Usage

``` r
# S3 method for class 'mixturecure'
summary(object, ...)
```

## Arguments

- object:

  a `mixturecure` object resulting from `curegmifs`, `cureem`,
  `cv_curegmifs`, or `cv_cureem`.

- ...:

  other arguments.

## Value

prints the number of non-zero coefficients from the incidence and
latency portions of the fitted mixture cure model when using the minimum
AIC to select the final model. When fitting a model using `curegmifs` or
`cureem` the summary function additionally prints results associated
with the following model selection methods: the step and value that
maximizes the log-likelihood; the step and value that minimizes the AIC,
modified AIC (mAIC), corrected AIC (cAIC), BIC, modified BIC (mBIC), and
extended BIC (EBIC). This information can be used to guide the user in
the selection of a final model from the solution path.

## See also

[`curegmifs`](https://docs.ropensci.org/hdcuremodels/reference/curegmifs.md),
[`cureem`](https://docs.ropensci.org/hdcuremodels/reference/cureem.md),
[`coef.mixturecure`](https://docs.ropensci.org/hdcuremodels/reference/coef.mixturecure.md),
[`plot.mixturecure`](https://docs.ropensci.org/hdcuremodels/reference/plot.mixturecure.md),
[`predict.mixturecure`](https://docs.ropensci.org/hdcuremodels/reference/predict.mixturecure.md)

## Examples

``` r
library(survival)
withr::local_seed(1234)
temp <- generate_cure_data(n = 100, j = 10, n_true = 10, a = 1.8)
training <- temp$training
fit <- curegmifs(Surv(Time, Censor) ~ .,
  data = training, x_latency = training,
  model = "weibull", thresh = 1e-4, maxit = 2000,
  epsilon = 0.01, verbose = FALSE
)
summary(fit)
#> Mixture cure model fit using the GMIFS algorithm 
#> Number of non-zero incidence covariates at minimum AIC: 11
#> Number of non-zero latency covariates at minimum AIC: 12
#> Optimal step for selected information criterion: GMIFS algorithm 
#>   at step    = 870 logLik     = -11.3809284169563
#>   at step    = 788 AIC        = 75.0902882298322
#>   at step    = 1 mAIC        = 159.323704538379
#>   at step    = 788 cAIC        = 104.340288229832
#>   at step    = 151 BIC        = 130.08672768038
#>   at step    = 1 mBIC        = 150.116729689263
#>   at step    = 448 EBIC        = -Inf
```
