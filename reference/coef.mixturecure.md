# Extract model coefficients from a fitted mixturecure object

`coef.mixturecure` is a generic function which extracts the model
coefficients from a fitted `mixturecure` model object fit using
`curegmifs`, `cureem`, `cv_curegmifs`, or `cv_cureem`.

## Usage

``` r
# S3 method for class 'mixturecure'
coef(object, model_select = "AIC", ...)
```

## Arguments

- object:

  a `mixturecure` object resulting from `curegmifs`, `cureem`,
  `cv_curegmifs`, or `cv_cureem`.

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

- rate:

  estimated rate parameter when fitting a Weibull or exponential mixture
  cure model.

- shape:

  estimated shape parameter when fitting a Weibull mixture cure model.

- b0:

  estimated intercept for the incidence portion of the mixture cure
  model.

- beta_inc:

  the vector of coefficient estimates for the incidence portion of the
  mixture cure model.

- beta_lat:

  the vector of coefficient estimates for the latency portion of the
  mixture cure model.

- p_uncured:

  a vector of probabilities from the incidence portion of the fitted
  model representing the P(uncured).

## See also

[`curegmifs`](https://docs.ropensci.org/hdcuremodels/reference/curegmifs.md),
[`cureem`](https://docs.ropensci.org/hdcuremodels/reference/cureem.md),
[`summary.mixturecure`](https://docs.ropensci.org/hdcuremodels/reference/summary.mixturecure.md),
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
coef(fit)
#> $rate
#> [1] 4.546212
#> 
#> $shape
#> [1] 1.149704
#> 
#> $b0
#> [1] 1.040589
#> 
#> $beta_inc
#>    U1    U2    X1    X2    X3    X4    X5    X6    X7    X8    X9   X10 
#> -0.68 -0.74 -0.97 -0.46 -2.64 -1.58 -0.48  1.29  1.53 -0.18 -1.08  1.22 
#> 
#> $beta_lat
#>    U1    U2    X1    X2    X3    X4    X5    X6    X7    X8    X9   X10 
#> -0.44  0.05  0.77 -1.02 -1.20 -0.15  0.72 -0.05  0.01 -0.90  0.64 -1.32 
#> 
```
