# Log-likelihood for fitted mixture cure model

This function returns the log-likelihood for a user-specified model
criterion or step for a `curegmifs`, `cureem`, `cv_curegmifs` or
`cv_cureem` fitted object.

## Usage

``` r
# S3 method for class 'mixturecure'
logLik(object, model_select = "AIC", ...)
```

## Arguments

- object:

  a `mixturecure` object resulting from `curegmifs`, `cureem`,
  `cv_curegmifs`, `cv_cureem`.

- model_select:

  either a case-sensitive parameter for models fit using `curegmifs` or
  `cureem` or any numeric step along the solution path can be selected.
  The default is `model_select = "AIC"` which calculates the predicted
  values using the coefficients from the model achieving the minimum
  AIC. The complete list of options are:

  - `"AIC"` for the minimum AIC (default).

  - `"mAIC"` for the minimum modified AIC.

  - `"cAIC"` for the minimum corrected AIC.

  - `"BIC"`, for the minimum BIC.

  - `"mBIC"` for the minimum modified BIC.

  - `"EBIC"` for the minimum extended BIC.

  - `"logLik"` for the step that maximizes the log-likelihood.

  - `n` where n is any numeric value from the solution path.

  This option has no effect for objects fit using `cv_curegmifs` or
  `cv_cureem`.

- ...:

  other arguments.

## Value

log-likelihood of the fitted mixture cure model using the specified
criteria.

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
logLik(fit, model_select = "AIC")
#> 'log Lik.' 7.274242 (df=25)
```
