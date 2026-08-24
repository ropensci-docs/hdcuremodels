# AUC for cure prediction using mean score imputation

This function calculates the AUC for cure prediction using the mean
score imputation (MSI) method proposed by Asano et al (2014).

## Usage

``` r
auc_mcm(object, newdata, cure_cutoff = 5, model_select = "AIC")
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

  - `n` where n is any numeric value from the solution path.

  This option has no effect for objects fit using `cv_curegmifs` or
  `cv_cureem`.

## Value

Returns the AUC value for cure prediction using the mean score
imputation (MSI) method.

## References

Asano, J., Hirakawa, H., Hamada, C. (2014) Assessing the prediction
accuracy of cure in the Cox proportional hazards cure model: an
application to breast cancer data. *Pharmaceutical Statistics*,
**13**:357–363.

## See also

[`concordance_mcm`](https://docs.ropensci.org/hdcuremodels/reference/concordance_mcm.md)

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
auc_mcm(fit, model_select = "cAIC")
#> [1] 0.9045888
auc_mcm(fit, newdata = testing)
#> [1] 0.7866268
```
