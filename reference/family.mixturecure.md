# Return model family and fitting algorithm for mixturecure model fits

Return model family and fitting algorithm for`mixturecure` model fits.

## Usage

``` r
# S3 method for class 'mixturecure'
family(object, ...)
```

## Arguments

- object:

  an object of class `mixturecure`

- ...:

  other arguments.

## Value

the parametric or semi-parametric model fit and the fitting algorithm.

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
family(fit)
#> 
#> Family: Weibull 
#> Algorithm: GMIFS 
#> 
```
