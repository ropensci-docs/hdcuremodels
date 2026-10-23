# Fit penalized parametric mixture cure model using the GMIFS algorithm

Fits a penalized Weibull or exponential mixture cure model using the
generalized monotone incremental forward stagewise (GMIFS) algorithm
(Hastie et al 2007) and yields solution paths for parameters in the
incidence and latency portions of the model.

## Usage

``` r
curegmifs(
  formula,
  data,
  subset,
  x_latency = NULL,
  model = c("weibull", "exponential"),
  penalty_factor_inc = NULL,
  penalty_factor_lat = NULL,
  epsilon = 0.001,
  thresh = 1e-05,
  scale = TRUE,
  maxit = 10000,
  inits = NULL,
  verbose = TRUE,
  suppress_warning = FALSE,
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

- epsilon:

  small numeric value reflecting the incremental value used to update a
  coefficient at a given step (default is 0.001).

- thresh:

  small numeric value. The iterative process stops when the differences
  between successive expected penalized log-likelihoods for both
  incidence and latency components are less than this specified level of
  tolerance (default is 10^-5).

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

  If not supplied or improperly supplied, initialization is
  automatically provided by the function.

- verbose:

  logical, if TRUE running information is printed to the console
  (default is FALSE).

- suppress_warning:

  logical, if TRUE, suppresses echoing the warning that the maximum
  number of iterations was reached so that the algorithm may not have
  converged. Instead, warning is returned as part of the output with
  this message.

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

- b0_path:

  Vector representing the solution path of the intercept in the
  incidence portion of the model.

- rate_path:

  Vector representing the solution path of the rate parameter for the
  Weibull or exponential density in the latency portion of the model.

- logLik:

  Vector representing the log-likelihood for each step in the solution
  path.

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

- alpha_path:

  Vector representing the solution path of the shape parameter for the
  Weibull density in the latency portion of the model.

- call:

  the matched call.

- warning:

  message indicating whether the maximum number of iterations was
  achieved which may indicate the model did not converge.

## References

Fu, H., Nicolet, D., Mrozek, K., Stone, R. M., Eisfeld, A. K., Byrd, J.
C., Archer, K. J. (2022) Controlled variable selection in Weibull
mixture cure models for high-dimensional data. *Statistics in Medicine*,
**41**(22), 4340–4366.

Hastie, T., Taylor J., Tibshirani R., Walther G. (2007) Forward
stagewise regression and the monotone lasso. *Electron J Stat*,
**1**:1–29.

## See also

[`cv_curegmifs`](https://docs.ropensci.org/hdcuremodels/reference/cv_curegmifs.md)

## Examples

``` r
library(survival)
withr::local_seed(1234)
temp <- generate_cure_data(n = 100, j = 10, n_true = 10, a = 1.8)
training <- temp$training

fit <- curegmifs(Surv(Time, Censor) ~ .,
  data = training, x_latency = training,
  model = "weibull", thresh = 1e-4, maxit = 2000, epsilon = 0.01,
  verbose = FALSE
)
```
