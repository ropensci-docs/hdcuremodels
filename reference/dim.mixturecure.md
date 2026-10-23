# Dimension method for mixturecure objects

Dimension method for `mixturecure` objects.

## Usage

``` r
# S3 method for class 'mixturecure'
dim(x)
```

## Arguments

- x:

  An object of class \`mixturecure\`.

## Value

- nobs:

  number of subjects in the dataset.

- p_incidence:

  number of variables in the incidence portion of the model.

- p_latency:

  number of variables in the latency portion of the model.

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
dim(fit)
#>        nobs p_incidence   p_latency 
#>          75          12          12 
```
