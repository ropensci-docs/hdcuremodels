# Print the contents of a mixture cure fitted object

This function prints the first several incidence and latency
coefficients, the rate (when fitting an exponential or Weibull mixture
cure model), and alpha (when fitting a Weibull mixture cure model). This
function returns the fitted object invisible to the user.

## Usage

``` r
# S3 method for class 'mixturecure'
print(x, max = 6, ...)
```

## Arguments

- x:

  a `mixturecure` object resulting from `curegmifs`, `cureem`,
  `cv_cureem`, or `cv_curegmifs`.

- max:

  maximum number of rows in a matrix or elements in a vector to display

- ...:

  other arguments.

## Value

prints coefficient estimates for the incidence portion of the model and
if included, prints the coefficient estimates for the latency portion of
the model. Also prints rate for exponential and Weibull models and scale
(alpha) for the Weibull mixture cure model. Returns all objects fit
using `cureem`, `curegmifs`, `cv_cureem`, or `cv_curegmifs`.

## Note

The contents of a `mixturecure` fitted object differ depending upon
whether the EM (`cureem`) or GMIFS (`curegmifs`) algorithm is used for
model fitting or if cross-validation is used. Also, the output differs
depending upon whether `x_latency` is specified in the model (i.e.,
variables are included in the latency portion of the model fit) or only
`terms` on the right hand side of the equation are included (i.e.,
variables are included in the incidence portion of the model).

## See also

[`curegmifs`](https://docs.ropensci.org/hdcuremodels/reference/curegmifs.md),
[`cureem`](https://docs.ropensci.org/hdcuremodels/reference/cureem.md),
[`coef.mixturecure`](https://docs.ropensci.org/hdcuremodels/reference/coef.mixturecure.md),
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
print(fit)
#> mixturecure object fit using Weibull GMIFS algorithm
#> 
#> $b_path
#>      U1 U2 X1 X2 X3 X4
#> [1,]  0  0  0  0  0  0
#> [2,]  0  0  0  0  0  0
#> [3,]  0  0  0  0  0  0
#> [4,]  0  0  0  0  0  0
#> [5,]  0  0  0  0  0  0
#> [6,]  0  0  0  0  0  0
#> 1060 more rows
#> 6 more columns
#> 
#> $beta_path
#>      U1 U2 X1    X2 X3 X4
#> [1,]  0  0  0 -0.01  0  0
#> [2,]  0  0  0 -0.02  0  0
#> [3,]  0  0  0 -0.03  0  0
#> [4,]  0  0  0 -0.04  0  0
#> [5,]  0  0  0 -0.05  0  0
#> [6,]  0  0  0 -0.05  0  0
#> 1060 more rows
#> 6 more columns
#> 
#> $rate
#>  1.652884 1.655865 1.658297 1.660449 1.662323 1.675661 
#> 1060 more elements
#> 
#> $alpha
#>  0.6271478 0.6282657 0.6295001 0.6307358 0.6319733 0.6341303 
#> 1060 more elements
#> 
```
