# Predicted probabilities for susceptibles, linear predictor for latency, and risk class for latency for mixture cure fit

This function returns a list that includes the predicted probabilities
for susceptibles as well as the linear predictor for the latency
distribution and a dichotomous risk for latency for a `curegmifs`,
`cureem`, `cv_curegmifs` or `cv_cureem` fitted object.

## Usage

``` r
# S3 method for class 'mixturecure'
predict(object, newdata, model_select = "AIC", ...)
```

## Arguments

- object:

  a `mixturecure` object resulting from `curegmifs`, `cureem`,
  `cv_curegmifs`, or `cv_cureem`.

- newdata:

  an optional data.frame that minimally includes the incidence and/or
  latency variables to use for predicting the response. If omitted, the
  training data are used.

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

  other arguments

## Value

- p_uncured:

  a vector of probabilities from the incidence portion of the fitted
  model representing the P(uncured).

- linear_latency:

  a vector for the linear predictor from the latency portion of the
  model.

- latency_risk:

  a dichotomous class representing low (below the median) versus high
  risk for the latency portion of the model.

## See also

[`curegmifs`](https://docs.ropensci.org/hdcuremodels/reference/curegmifs.md),
[`cureem`](https://docs.ropensci.org/hdcuremodels/reference/cureem.md),
[`coef.mixturecure`](https://docs.ropensci.org/hdcuremodels/reference/coef.mixturecure.md),
[`summary.mixturecure`](https://docs.ropensci.org/hdcuremodels/reference/summary.mixturecure.md),
[`plot.mixturecure`](https://docs.ropensci.org/hdcuremodels/reference/plot.mixturecure.md)

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
predict_train <- predict(fit)
names(predict_train)
#> [1] "p_uncured"      "linear_latency" "latency_risk"  
testing <- temp$testing
predict_test <- predict(fit, newdata = testing)
```
