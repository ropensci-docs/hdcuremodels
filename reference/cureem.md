# Fit penalized mixture cure model using the E-M algorithm

Fits penalized parametric and semi-parametric mixture cure models (MCM)
using the E-M algorithm with user-specified penalty parameters. The
lasso (L1), MCP, and SCAD penalty are supported for the Cox MCM while
only lasso is currently supported for parametric MCMs.

## Usage

``` r
cureem(
  formula,
  data,
  subset,
  x_latency = NULL,
  model = c("cox", "weibull", "exponential"),
  penalty = c("lasso", "MCP", "SCAD"),
  penalty_factor_inc = NULL,
  penalty_factor_lat = NULL,
  thresh = 0.001,
  scale = TRUE,
  maxit = NULL,
  inits = NULL,
  lambda_inc = 0.1,
  lambda_lat = 0.1,
  gamma_inc = 3,
  gamma_lat = 3,
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

- thresh:

  small numeric value. The iterative process stops when the differences
  between successive expected penalized complete-data log-likelihoods
  for both incidence and latency components are less than this specified
  level of tolerance (default is 10^-3).

- scale:

  logical, if TRUE the predictors are centered and scaled.

- maxit:

  integer specifying the maximum number of passes over the data for each
  lambda. If not specified, 100 is applied when `penalty = "lasso"` and
  1000 is applied when `penalty = "MCP"` or `penalty = "SCAD"`.

- inits:

  an optional list specifiying the initial values. This includes:

  - `itct` the incidence intercept.

  - `b_u` a numeric vector for the unpenalized incidence coefficients
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

- lambda_inc:

  numeric value for the penalization parameter \\\lambda\\ for variables
  in the incidence portion of the model.

- lambda_lat:

  numeric value for the penalization parameter \\\lambda\\ for variables
  in the latency portion of the model.

- gamma_inc:

  numeric value for the penalization parameter \\\gamma\\ for variables
  in the incidence portion of the model when `penalty = "MCP"` or
  `penalty = "SCAD"` (default is 3).

- gamma_lat:

  numeric value for the penalization parameter \\\gamma\\ for variables
  in the latency portion of the model when `penalty = "MCP"` or
  `penalty = "SCAD"` (default is 3).

- na.action:

  this function requires complete data so `"na.omit"` is invoked. Users
  can impute missing data as an alternative prior to model fitting.

- ...:

  additional arguments.

## Value

- b_path:

  Matrix representing the solution path of the coefficients in the
  incidence portion of the model. Row is step and column is variable.

- beta_path:

  Matrix representing the solution path of the coefficients in the
  latency portion of the model. Row is step and column is variable.

- b0_path :

  Vector representing the solution path of the intercept in the
  incidence portion of the model.

- logLik_inc :

  Vector representing the expected penalized complete-data
  log-likelihood for the incidence portion of the model for each step in
  the solution path.

- logLik_lat :

  Vector representing the expected penalized complete-data
  log-likelihood for the latency portion of the model for each step in
  the solution path.

- x_incidence:

  Matrix representing the design matrix of the incidence predictors.

- x_latency:

  Matrix representing the design matrix of the latency predictors.

- y:

  Vector representing the survival object response as returned by the
  `Surv` function

- model:

  Character string indicating the type of regression model used for the
  latency portion of mixture cure model ("weibull" or "exponential").

- scale:

  Logical value indicating whether the predictors were centered and
  scaled.

- method:

  Character string indicating the EM algorithm was used in fitting the
  mixture cure model.

- rate_path:

  Vector representing the solution path of the rate parameter for the
  Weibull or exponential density in the latency portion of the model.

- alpha_path:

  Vector representing the solution path of the shape parameter for the
  Weibull density in the latency portion of the model.

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

[`cv_cureem`](https://docs.ropensci.org/hdcuremodels/reference/cv_cureem.md)

## Examples

``` r
library(survival)
withr::local_seed(1234)
temp <- generate_cure_data(n = 80, j = 100, n_true = 10, a = 1.8)
training <- temp$training
fit <- cureem(Surv(Time, Censor) ~ .,
  data = training, x_latency = training,
  model = "cox", penalty = "lasso", lambda_inc = 0.1,
  lambda_lat = 0.1, gamma_inc = 6, gamma_lat = 10
)
```
