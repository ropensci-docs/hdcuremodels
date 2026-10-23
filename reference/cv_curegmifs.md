# Fit a penalized parametric mixture cure model using the generalized monotone incremental forward stagewise (GMIFS) algorithm (Hastie et al 2007) with cross-validation for model selection

Fits a penalized Weibull or exponential mixture cure model using the
generalized monotone incremental forward stagewise (GMIFS) algorithm
with k-fold cross-validation to select the optimal iteration step along
the solution path. When FDR controlled variable selection is used, the
model-X knockoffs method is applied and indices of selected variables
are returned.

## Usage

``` r
cv_curegmifs(
  formula,
  data,
  subset,
  x_latency = NULL,
  model = c("weibull", "exponential"),
  penalty_factor_inc = NULL,
  penalty_factor_lat = NULL,
  fdr_control = FALSE,
  fdr = 0.2,
  epsilon = 0.001,
  thresh = 1e-05,
  scale = TRUE,
  maxit = 10000,
  inits = NULL,
  n_folds = 5,
  measure_inc = c("c", "auc"),
  one_se = FALSE,
  cure_cutoff = 5,
  parallel = FALSE,
  seed = NULL,
  verbose = TRUE,
  na.action = na.omit,
  ...
)
```

## Arguments

- formula:

  an object of class "`formula`" (or one that can be coerced to that
  class): a symbolic description of the model to be fitted. The response
  must be a survival object as returned by the `Surv` function while the
  variables on the right side of the formula are the covariates that are
  included in the incidence portion of the model.

- data:

  a data.frame in which to interpret the variables named in the
  `formula` or in the `subset` argument. Rows with missing data are
  omitted (only `na.action = na.omit` is operational) therefore users
  may want to impute missing data prior to calling this function.

- subset:

  an optional expression indicating which subset of observations to be
  used in the fitting process, either a numeric or factor variable
  should be used in subset, not a character variable. All observations
  are included by default.

- x_latency:

  specifies the variables to be included in the latency portion of the
  model and can be either a matrix of predictors, a model formula with
  the right hand side specifying the latency variables, or the same
  data.frame passed to the `data` parameter. Note that when using the
  model formula syntax for `x_latency` it cannot handle
  `x_latency = ~ .`.

- model:

  type of regression model to use for the latency portion of mixture
  cure model. Can be "weibull" or "exponential"; default is "weibull".

- penalty_factor_inc:

  vector of binary indicators representing the penalty to apply to each
  incidence coefficient: 0 implies no shrinkage and 1 implies shrinkage.
  If not supplied, 1 is applied to all incidence variables.

- penalty_factor_lat:

  vector of binary indicators representing the penalty to apply to each
  latency coefficient: 0 implies no shrinkage and 1 implies shrinkage.
  If not supplied, 1 is applied to all latency variables.

- fdr_control:

  logical, if TRUE, model-X knockoffs are used for FDR-controlled
  variable selection and indices of selected variables are returned
  (default is FALSE).

- fdr:

  numeric value in (0, 1) range specifying the target FDR level to use
  for variable selection when `fdr_control = TRUE` (default is 0.2).

- epsilon:

  small numeric value reflecting incremental value used to update a
  coefficient at a given step (default is 0.001).

- thresh:

  small numeric value. The iterative process stops when the differences
  between successive expected penalized complete-data log-likelihoods
  for both incidence and latency components are less than this specified
  level of tolerance (default is 10^-5).

- scale:

  logical, if TRUE the predictors are centered and scaled.

- maxit:

  integer specifying the maximum number of steps to run in the iterative
  algorithm (default is 10^4).

- inits:

  an optional list specifying the initial values as follows:

  - `itct` a numeric value for the unpenalized incidence intercept.

  - `b_u` a numeric vector for the unpenalized incidence coefficients.

  - `beta_u` a numeric vector for unpenalized latency coefficients.

  - `lambda` a numeric value for the rate parameter.

  - `alpha` a numeric value for the shape parameter when
    `model = "weibull"`.

  If `inits` is not specified or improperly supplied, initialization is
  automatically provided by the function.

- n_folds:

  an integer specifying the number of folds for the k-fold
  cross-validation procedure (default is 5).

- measure_inc:

  character string specifying the evaluation criterion used in selecting
  the optimal \\\lambda_b\\ which can be either

  - `"c"` specifying to use the C-statistic for cure status weighting
    (CSW) method proposed by Asano and Hirakawa (2017) to select both
    \\\lambda_b\\ and \\\lambda\_{\beta}\\

  - `"auc"` specifying to use the AUC for cure prediction using the mean
    score imputation (MSI) method proposed by Asano et al. (2014) to
    select \\\lambda_b\\ while the C-statistic with CSW is used for
    \\\lambda\_{\beta}\\.

- one_se:

  logical, if TRUE then the one standard error rule is applied for
  selecting the optimal parameters. The one standard error rule selects
  the most parsimonious model having evaluation criterion no more than
  one standard error worse than that of the best evaluation criterion
  (default is FALSE).

- cure_cutoff:

  numeric value representing the cutoff time value that represents
  subjects not experiencing the event by this time are cured. This value
  is used to produce a proxy for the unobserved cure status when
  calculating C-statistic and AUC (default is 5 representing 5 years).
  Users should be careful to note the time scale of their data and
  adjust this according to the time scale and clinical application.

- parallel:

  logical. If TRUE, parallel processing is performed for K-fold CV using
  `foreach` and the doParallel package is required.

- seed:

  optional integer representing the random seed. Setting the random seed
  fosters reproducibility of the results.

- verbose:

  logical, if TRUE running information is printed to the console
  (default is FALSE).

- na.action:

  this function requires complete data so `"na.omit"` is invoked. Users
  can impute missing data as an alternative prior to model fitting.

- ...:

  additional arguments.

## Value

- b0 :

  Estimated intercept for the incidence portion of the model.

- b :

  Estimated coefficients for the incidence portion of the model.

- beta :

  Estimated coefficients for the latency portion of the model.

- alpha :

  Estimated shape parameter if the Weibull model is fit.

- rate :

  Estimated rate parameter if the Weibull or exponential model is fit.

- logLik :

  Log-likelihood value.

- selected_step_inc :

  Iteration step selected for the incidence portion of the model using
  cross-validation. NULL when fdr_control is TRUE.

- selected_step_lat :

  Iteration step selected for the latency portion of the model using
  cross-validation. NULL when fdr_control is TRUE.

- max_c :

  Maximum C-statistic achieved

- max_auc :

  Maximum AUC for cure prediction achieved; only output when
  `measure_inc = "auc"`.

- selected_index_inc :

  Indices of selected variables for the incidence portion of the model
  when `fdr_control = TRUE`. If none selected, `int(0)` will be
  returned.

- selected_index_lat :

  Indices of selected variables for the latency portion of the model
  when `fdr_control = TRUE`. If none selected, `int(0)` will be
  returned.

- call:

  the matched call.

## References

Fu, H., Nicolet, D., Mrozek, K., Stone, R. M., Eisfeld, A. K., Byrd, J.
C., Archer, K. J. (2022) Controlled variable selection in Weibull
mixture cure models for high-dimensional data. *Statistics in Medicine*,
**41**(22), 4340–4366.

Hastie, T., Taylor J., Tibshirani R., Walther G. (2007) Forward
stagewise regression and the monotone lasso. *Electron J Stat*,
**1**:1–29.

## See also

[`curegmifs`](https://docs.ropensci.org/hdcuremodels/reference/curegmifs.md)

## Examples

``` r
if (FALSE) { # \dontrun{
library(survival)
withr::local_seed(123)
temp <- generate_cure_data(n = 100, j = 15, n_true = 3, a = 1.8, rho = 0.1)
training <- temp$training

fit.cv <- cv_curegmifs(Surv(Time, Censor) ~ .,
  data = training,
  x_latency = training, fdr_control = FALSE,
  maxit = 450, epsilon = 0.01, n_folds = 2, thresh = 1e-4,
  seed = 23, verbose = TRUE
)
} # }
```
