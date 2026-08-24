# Fit penalized mixture cure model using the E-M algorithm with cross-validation for parameter tuning

Fits penalized parametric and semi-parametric mixture cure models (MCM)
using the E-M algorithm with with k-fold cross-validation for parameter
tuning. The lasso (L1), MCP and SCAD penalty are supported for the Cox
MCM while only lasso is currently supported for parametric MCMs. When
FDR controlled variable selection is used, the model-X knockoffs method
is applied and indices of selected variables are returned.

## Usage

``` r
cv_cureem(
  formula,
  data,
  subset,
  x_latency = NULL,
  model = c("cox", "weibull", "exponential"),
  penalty = c("lasso", "MCP", "SCAD"),
  penalty_factor_inc = NULL,
  penalty_factor_lat = NULL,
  fdr_control = FALSE,
  fdr = 0.2,
  grid_tuning = FALSE,
  thresh = 0.001,
  scale = TRUE,
  maxit = NULL,
  inits = NULL,
  lambda_inc_list = NULL,
  lambda_lat_list = NULL,
  nlambda_inc = NULL,
  nlambda_lat = NULL,
  gamma_inc = 3,
  gamma_lat = 3,
  lambda_min_ratio_inc = 0.1,
  lambda_min_ratio_lat = 0.1,
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
  cure model. Can be "cox", "weibull", or "exponential" (default is
  "cox").

- penalty:

  type of penalty function. Can be "lasso", "MCP", or "SCAD" (default is
  "lasso").

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

- grid_tuning:

  logical, if TRUE a 2-D grid tuning approach is used to select the
  optimal pair of \\\lambda_b\\ and \\\lambda\_{\beta}\\ penalty
  parameters for the incidence and latency portions of the model,
  respectively. Otherwise the \\\lambda_b\\ and \\\lambda\_{\beta}\\ are
  selected from a 1-D sequence and are equal to one another (default is
  FALSE).

- thresh:

  small numeric value. The iterative process stops when the differences
  between successive expected penalized complete-data log-likelihoods
  for both incidence and latency components are less than this specified
  level of tolerance (default is 10^-3).

- scale:

  logical, if TRUE the predictors are centered and scaled.

- maxit:

  maximum number of passes over the data for each lambda. If not
  specified, 100 is applied when `penalty = "lasso"` and 1000 is applied
  when `penalty = "MCP"` or `penalty = "SCAD"`.

- inits:

  an optional list specifiying the initial values to be used for model
  fitting as follows:

  - `itct` the incidence intercept.

  - `b_u` a numeric vector for the unpenalized. incidence coefficients
    for the incidence portion of the model.

  - `beta_u` a numeric vector for unpenalized latency coefficients in
    the incidence portion of the model.

  - `lambda` a numeric value for the rate parameter when fitting either
    a Weibull or exponential MCM using `model = "weibull"` or
    `model = "exponential"`.

  - `alpha` a numeric value for the shape parameter when fitting a
    Weibull MCM using `model = "weibull"`.

  - `survprob` a numeric vector for the latency survival probabilities
    \\S_u(t_i\|w_i)\\ for i=1,...,N when fitting a Cox MCM
    `model = "cox"`.

  Penalized coefficients are initialized to zero. If `inits` is not
  specified or improperly specified, initialization is automatically
  provided by the function.

- lambda_inc_list:

  a numeric vector used to search for the optimal \\\lambda_b\\ tuning
  parameter. If not supplied, the function computes a \\\lambda_b\\
  sequence based on `nlambda_inc` and `lambda_min_ratio_inc`. If
  `grid_tuning = FALSE`, the same sequence should be used for both
  \\\lambda_b\\ and \\\lambda\_{\beta}\\.

- lambda_lat_list:

  a numeric vector used to search for the optimal \\\lambda\_{\beta}\\
  tuning parameter. If not supplied, the function computes a
  \\\lambda\_{\beta}\\ sequence based on `nlambda_lat` and
  `lambda_min_ratio_lat`. If `grid_tuning = FALSE`, the same sequence
  should be used for both \\\lambda_b\\ and \\\lambda\_{\beta}\\.

- nlambda_inc:

  an integer specifying the number of values to search for the optimal
  \\\lambda_b\\ tuning parameter; default is 10 if `grid_tuning = TRUE`
  and 50 otherwise.

- nlambda_lat:

  an integer specifying the number of values to search for the optimal
  \\\lambda\_{\beta}\\ tuning parameter; default is 10 if
  `grid_tuning = TRUE` and 50 otherwise.

- gamma_inc:

  numeric value for the penalization parameter \\\gamma\\ for variables
  in the incidence portion of the model when `penalty = "MCP"` or
  `penalty = "SCAD"` (default is 3).

- gamma_lat:

  numeric value for the penalization parameter \\\gamma\\ for variables
  in the latency portion of the model when `penalty = "MCP"` or
  `penalty = "SCAD"` (default is 3).

- lambda_min_ratio_inc:

  numeric value in (0,1) representing the smallest value for
  \\\lambda_b\\ as a fraction of `lambda.max_inc`, the data-derived
  entry value at which essentially all penalized variables in the
  incidence portion of the model have a coefficient estimate of 0
  (default is 0.1).

- lambda_min_ratio_lat:

  numeric value in (0.1) representing the smallest value for
  \\\lambda\_{\beta}\\ as a fraction of `lambda.max_lat`, the
  data-derived entry value at essentially all penalized variables in the
  latency portion of the model have a coefficient estimate of 0 (default
  is 0.1).

- n_folds:

  an integer specifying the number of folds for the k-fold
  cross-valiation procedure (default is 5).

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

- b0:

  Estimated intercept for the incidence portion of the model.

- b:

  Estimated coefficients for the incidence portion of the model.

- beta:

  Estimated coefficients for the latency portion of the model.

- alpha:

  Estimated shape parameter if the Weibull model is fit.

- rate:

  Estimated rate parameter if the Weibull or exponential model is fit.

- logLik_inc:

  Expected penalized complete-data log-likelihood for the incidence
  portion of the model.

- logLik_lat:

  Expected penalized complete-data log-likelihood for the latency
  portion of the model.

- selected_lambda_inc:

  Value of \\\lambda_b\\ selected using cross-validation. NULL when
  fdr_control is TRUE.

- selected_lambda_lat:

  Value of \\\lambda\_{\beta}\\ selected using cross-validation. NULL
  when fdr_control is TRUE.

- max_c:

  Maximum C-statistic achieved.

- max_auc:

  Maximum AUC for cure prediction achieved; only output when
  `measure_inc="auc"`.

- selected_index_inc :

  Indices of selected variables for the incidence portion of the model
  when `fdr_control=TRUE`. If no variables are selected, `int(0)` will
  be returned.

- selected_index_lat :

  Indices of selected variables for the latency portion of the model
  when `fdr_control=TRUE`. If no variables are selected, `int(0)` will
  be returned.

- call:

  the matched call.

## References

Archer, K. J., Fu, H., Mrozek, K., Nicolet, D., Mims, A. S., Uy, G. L.,
Stock, W., Byrd, J. C., Hiddemann, W., Braess, J., Spiekermann, K.,
Metzeler, K. H., Herold, T., Eisfeld, A.-K. (2024) Identifying long-term
survivors and those at higher or lower risk of relapse among patients
with cytogenetically normal acute myeloid leukemia using a
high-dimensional mixture cure model. *Journal of Hematology & Oncology*,
**17**:28.

## See also

[`cureem`](https://docs.ropensci.org/hdcuremodels/reference/cureem.md)

## Examples

``` r
if (FALSE) { # \dontrun{
library(survival)
withr::local_seed(1234)
temp <- generate_cure_data(n = 200, j = 25, n_true = 5, a = 1.8, rho = 0.2)
training <- temp$training
# Fit a penalized Cox MCM selecting parameters using 2-fold CV
fit.cv <- cv_cureem(Surv(Time, Censor) ~ .,
  data = training,
  x_latency = training, fdr_control = FALSE,
  grid_tuning = FALSE, nlambda_inc = 10, nlambda_lat = 10,
  n_folds = 2, seed = 23, verbose = FALSE
)
fit.cv.fdr <- cv_cureem(Surv(Time, Censor) ~ .,
   data = training,
   x_latency = training, model = "weibull", penalty = "lasso",
   fdr_control = TRUE, grid_tuning = FALSE, nlambda_inc = 10,
   nlambda_lat = 10, n_folds = 2, seed = 23, verbose = TRUE
   )
} # }
```
