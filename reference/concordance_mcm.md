# C-statistic for mixture cure models

This function calculates the C-statistic using the cure status weighting
(CSW) method proposed by Asano and Hirakawa (2017).

## Usage

``` r
concordance_mcm(object, newdata, cure_cutoff = 5, model_select = "AIC")
```

## Arguments

- object:

  a `mixturecure` object resulting from `curegmifs`, `cureem`,
  `cv_curegmifs`, or `cv_cureem`.

- newdata:

  an optional data.frame that minimally includes the incidence and/or
  latency variables to use for predicting the response. If omitted, the
  training data are used.

- cure_cutoff:

  cutoff value for cure, used to produce a proxy for the unobserved cure
  status (default is 5 representing 5 years). Users should be careful to
  note the time scale of their data and adjust this according to the
  time scale and clinical application.

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

  - `model_select = n` where n is any numeric value from the solution
    path.

  This option has no effect for objects fit using `cv_curegmifs` or
  `cv_cureem`.

## Value

value of C-statistic for the cure models.

## References

Asano, J. and Hirakawa, H. (2017) Assessing the prediction accuracy of a
cure model for censored survival data with long-term survivors:
Application to breast cancer data. *Journal of Biopharmaceutical
Statistics*, **27**:6, 918–932.

## See also

[`auc_mcm`](https://docs.ropensci.org/hdcuremodels/reference/auc_mcm.md)

## Examples

``` r
library(survival)
withr::local_seed(1234)
temp <- generate_cure_data(n = 100, j = 10, n_true = 10, a = 1.8)
training <- temp$training
testing <- temp$testing
fit <- curegmifs(Surv(Time, Censor) ~ .,
  data = training, x_latency = training,
  model = "weibull", thresh = 1e-4, maxit = 2000,
  epsilon = 0.01, verbose = FALSE
)
#> Warning: Warning: Maximum step of iterations achieved.
#>                            Algorithm may not converge.
concordance_mcm(fit, model_select = "cAIC")
#> [1] 0.813186
concordance_mcm(fit, newdata = testing, model_select = "cAIC")
#> [1] 0.7101386
```
