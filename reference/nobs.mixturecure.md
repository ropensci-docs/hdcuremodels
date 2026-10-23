# Number of observations in mixturecure object

Number of observations in fitted \`mixturecure\` object.

## Usage

``` r
# S3 method for class 'mixturecure'
nobs(object, ...)
```

## Arguments

- object:

  An object of class \`mixturecure\`.

- ...:

  other arguments.

## Value

number of subjects in the dataset.

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
#> Warning: Warning: Maximum step of iterations achieved.
#>                            Algorithm may not converge.
nobs(fit)
#> [1] 75
```
