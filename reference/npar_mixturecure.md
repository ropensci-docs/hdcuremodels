# Number of parameters in fitted mixture cure model

This function returns the number of parameters in a user-specified model
criterion or step for a `curegmifs`, `cureem`, `cv_curegmifs` or
`cv_cureem` fitted object.

## Usage

``` r
npar_mixturecure(object, model_select = "AIC")
```

## Arguments

- object:

  a `mixturecure` object resulting from `curegmifs`, `cureem`,
  `cv_curegmifs`, `cv_cureem`.

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

number of paramaters of the fitted mixture cure model using the
specified criteria.
