# Summarize model

**\[stable\]**

## Usage

``` r
summarize(
 .object = NULL, 
 .alpha  = 0.05,
 .ci     = NULL,
 ...
 )
```

## Arguments

- .object:

  An R object of class
  [cSEMResults](https://floschuberth.github.io/cSEM/reference/csem_results.md)
  resulting from a call to
  [`csem()`](https://floschuberth.github.io/cSEM/reference/csem.md).

- .alpha:

  An integer or a numeric vector of significance levels. Defaults to
  `0.05`.

- .ci:

  A vector of character strings naming the confidence interval to
  compute. For possible choices see
  [`infer()`](https://floschuberth.github.io/cSEM/reference/infer.md).

- ...:

  Further arguments to `summarize()`. Currently ignored.

## Value

An object of class `cSEMSummarize`. A `cSEMSummarize` object has the
same structure as the
[cSEMResults](https://floschuberth.github.io/cSEM/reference/csem_results.md)
object with a couple differences:

1.  Elements `$Path_estimates`, `$Loadings_estimates`,
    `$Weight_estimates`, `$Weight_estimates`, and
    `$Residual_correlation` are standardized data frames instead of
    matrices.

2.  Data frames `$Effect_estimates`, `$Indicator_correlation`, and
    `$Exo_construct_correlation` are added to `$Estimates`.

The data frame format is usually much more convenient if users intend to
present the results in e.g., a paper or a presentation.

## Details

The summary is mainly focused on estimated parameters. For quality
criteria such as the average variance extracted (AVE), reliability
estimates, effect size estimates etc., use
[`assess()`](https://floschuberth.github.io/cSEM/reference/assess.md).

If `.object` contains resamples, standard errors, t-values and p-values
(assuming estimates are standard normally distributed) are printed as
well. By default the percentile confidence interval is given as well.
For other confidence intervals use the `.ci` argument. See
[`infer()`](https://floschuberth.github.io/cSEM/reference/infer.md) for
possible choices and a description.

## See also

[csem](https://floschuberth.github.io/cSEM/reference/csem.md),
[`assess()`](https://floschuberth.github.io/cSEM/reference/assess.md),
[cSEMResults](https://floschuberth.github.io/cSEM/reference/csem_results.md),
[`exportToExcel()`](https://floschuberth.github.io/cSEM/reference/exportToExcel.md)

## Examples

``` r
## Take a look at the dataset
#?threecommonfactors

## Specify the (correct) model
model <- "
# Structural model
eta2 ~ eta1
eta3 ~ eta1 + eta2

# (Reflective) measurement model
eta1 =~ y11 + y12 + y13
eta2 =~ y21 + y22 + y23
eta3 =~ y31 + y32 + y33
"

## Estimate
res <- csem(threecommonfactors, model, .resample_method = "bootstrap", .R = 40)

## Postestimation
res_summarize <- summarize(res)
res_summarize
#> ________________________________________________________________________________
#> ----------------------------------- Overview -----------------------------------
#> 
#>  General information:
#>  ------------------------
#>  Estimation status                  = Ok
#>  Number of observations             = 500
#>  Weight estimator                   = PLS-PM
#>  Inner weighting scheme             = "path"
#>  Type of indicator correlation      = Pearson
#>  Path model estimator               = OLS
#>  Second-order approach              = NA
#>  Type of path model                 = Linear
#>  Disattenuated                      = Yes (PLSc)
#> 
#>  Resample information:
#>  ---------------------
#>  Resample method                    = "bootstrap"
#>  Number of resamples                = 40
#>  Number of admissible results       = 40
#>  Approach to handle inadmissibles   = "drop"
#>  Sign change option                 = "none"
#>  Random seed                        = -1219169535
#> 
#>  Construct details:
#>  ------------------
#>  Name  Modeled as     Order         Mode      
#> 
#>  eta1  Common factor  First order   "modeA"   
#>  eta2  Common factor  First order   "modeA"   
#>  eta3  Common factor  First order   "modeA"   
#> 
#> ----------------------------------- Estimates ----------------------------------
#> 
#> Estimated path coefficients:
#> ============================
#>                                                              CI_percentile   
#>   Path           Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1      0.6713      0.0346   19.3936    0.0000 [ 0.6015; 0.7201 ] 
#>   eta3 ~ eta1      0.4585      0.0693    6.6192    0.0000 [ 0.3736; 0.6111 ] 
#>   eta3 ~ eta2      0.3052      0.0818    3.7310    0.0002 [ 0.1450; 0.4163 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0321   20.6448    0.0000 [ 0.6037; 0.7170 ] 
#>   eta1 =~ y12      0.6493      0.0436   14.8879    0.0000 [ 0.5655; 0.7051 ] 
#>   eta1 =~ y13      0.7613      0.0317   24.0438    0.0000 [ 0.6959; 0.8112 ] 
#>   eta2 =~ y21      0.5165      0.0500   10.3330    0.0000 [ 0.4082; 0.5862 ] 
#>   eta2 =~ y22      0.7554      0.0347   21.7791    0.0000 [ 0.6950; 0.8035 ] 
#>   eta2 =~ y23      0.7997      0.0333   24.0312    0.0000 [ 0.7438; 0.8740 ] 
#>   eta3 =~ y31      0.8223      0.0379   21.7120    0.0000 [ 0.7665; 0.8878 ] 
#>   eta3 =~ y32      0.6581      0.0368   17.8704    0.0000 [ 0.5977; 0.7240 ] 
#>   eta3 =~ y33      0.7474      0.0304   24.6246    0.0000 [ 0.7117; 0.8163 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0220   17.9669    0.0000 [ 0.3610; 0.4363 ] 
#>   eta1 <~ y12      0.3873      0.0191   20.2693    0.0000 [ 0.3532; 0.4169 ] 
#>   eta1 <~ y13      0.4542      0.0170   26.6661    0.0000 [ 0.4276; 0.4895 ] 
#>   eta2 <~ y21      0.3058      0.0249   12.2696    0.0000 [ 0.2533; 0.3542 ] 
#>   eta2 <~ y22      0.4473      0.0172   26.0304    0.0000 [ 0.4071; 0.4744 ] 
#>   eta2 <~ y23      0.4735      0.0225   21.0011    0.0000 [ 0.4426; 0.5270 ] 
#>   eta3 <~ y31      0.4400      0.0155   28.3069    0.0000 [ 0.4050; 0.4595 ] 
#>   eta3 <~ y32      0.3521      0.0181   19.4951    0.0000 [ 0.3210; 0.3864 ] 
#>   eta3 <~ y33      0.3999      0.0179   22.3437    0.0000 [ 0.3739; 0.4456 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0346   19.3936    0.0000 [ 0.6015; 0.7201 ] 
#>   eta3 ~ eta1       0.6634      0.0327   20.2617    0.0000 [ 0.6188; 0.7218 ] 
#>   eta3 ~ eta2       0.3052      0.0818    3.7310    0.0002 [ 0.1450; 0.4163 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0558    3.6703    0.0002 [ 0.0911; 0.2995 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err   t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03211804 20.64478  1.087557e-94
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04361124 14.88786  3.952325e-50
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03166490 24.04384 9.683288e-128
#> 4 eta2 =~ y21  Common factor 0.5164548 0.04998112 10.33300  4.997353e-25
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03468404 21.77911 3.660798e-105
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03327605 24.03121 1.312402e-127
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03787201 21.71201 1.580035e-104
#> 8 eta3 =~ y32  Common factor 0.6580689 0.03682447 17.87043  2.004562e-71
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03035272 24.62462 6.885123e-134
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.6037236          0.7169804
#> 2          0.5655385          0.7050699
#> 3          0.6959308          0.8112391
#> 4          0.4082115          0.5861690
#> 5          0.6950436          0.8035213
#> 6          0.7438384          0.8740047
#> 7          0.7665372          0.8877857
#> 8          0.5977166          0.7239779
#> 9          0.7116853          0.8162580

## By default only the 95% percentile confidence interval is printed. User
## can have several confidence interval computed, however, only the first
## will be printed.

res_summarize <- summarize(res, .ci = c("CI_standard_t", "CI_percentile"), 
                           .alpha = c(0.05, 0.01))
res_summarize
#> ________________________________________________________________________________
#> ----------------------------------- Overview -----------------------------------
#> 
#>  General information:
#>  ------------------------
#>  Estimation status                  = Ok
#>  Number of observations             = 500
#>  Weight estimator                   = PLS-PM
#>  Inner weighting scheme             = "path"
#>  Type of indicator correlation      = Pearson
#>  Path model estimator               = OLS
#>  Second-order approach              = NA
#>  Type of path model                 = Linear
#>  Disattenuated                      = Yes (PLSc)
#> 
#>  Resample information:
#>  ---------------------
#>  Resample method                    = "bootstrap"
#>  Number of resamples                = 40
#>  Number of admissible results       = 40
#>  Approach to handle inadmissibles   = "drop"
#>  Sign change option                 = "none"
#>  Random seed                        = -1219169535
#> 
#>  Construct details:
#>  ------------------
#>  Name  Modeled as     Order         Mode      
#> 
#>  eta1  Common factor  First order   "modeA"   
#>  eta2  Common factor  First order   "modeA"   
#>  eta3  Common factor  First order   "modeA"   
#> 
#> ----------------------------------- Estimates ----------------------------------By default, only one confidence interval supplied to `.ci` is printed.
#> Use `xxx` to print all confidence intervals (not yet implemented).
#> 
#> 
#> 
#> Estimated path coefficients:
#> ============================
#>                                                              CI_standard_t   
#>   Path           Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1      0.6713      0.0346   19.3936    0.0000 [ 0.5875; 0.7666 ] 
#>   eta3 ~ eta1      0.4585      0.0693    6.6192    0.0000 [ 0.2679; 0.6262 ] 
#>   eta3 ~ eta2      0.3052      0.0818    3.7310    0.0002 [ 0.0981; 0.5211 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0321   20.6448    0.0000 [ 0.5789; 0.7450 ] 
#>   eta1 =~ y12      0.6493      0.0436   14.8879    0.0000 [ 0.5408; 0.7664 ] 
#>   eta1 =~ y13      0.7613      0.0317   24.0438    0.0000 [ 0.6795; 0.8432 ] 
#>   eta2 =~ y21      0.5165      0.0500   10.3330    0.0000 [ 0.3959; 0.6544 ] 
#>   eta2 =~ y22      0.7554      0.0347   21.7791    0.0000 [ 0.6660; 0.8453 ] 
#>   eta2 =~ y23      0.7997      0.0333   24.0312    0.0000 [ 0.7048; 0.8769 ] 
#>   eta3 =~ y31      0.8223      0.0379   21.7120    0.0000 [ 0.7266; 0.9224 ] 
#>   eta3 =~ y32      0.6581      0.0368   17.8704    0.0000 [ 0.5633; 0.7537 ] 
#>   eta3 =~ y33      0.7474      0.0304   24.6246    0.0000 [ 0.6593; 0.8163 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0220   17.9669    0.0000 [ 0.3370; 0.4509 ] 
#>   eta1 <~ y12      0.3873      0.0191   20.2693    0.0000 [ 0.3401; 0.4390 ] 
#>   eta1 <~ y13      0.4542      0.0170   26.6661    0.0000 [ 0.4093; 0.4974 ] 
#>   eta2 <~ y21      0.3058      0.0249   12.2696    0.0000 [ 0.2474; 0.3763 ] 
#>   eta2 <~ y22      0.4473      0.0172   26.0304    0.0000 [ 0.4039; 0.4927 ] 
#>   eta2 <~ y23      0.4735      0.0225   21.0011    0.0000 [ 0.4106; 0.5272 ] 
#>   eta3 <~ y31      0.4400      0.0155   28.3069    0.0000 [ 0.4033; 0.4837 ] 
#>   eta3 <~ y32      0.3521      0.0181   19.4951    0.0000 [ 0.3075; 0.4009 ] 
#>   eta3 <~ y33      0.3999      0.0179   22.3437    0.0000 [ 0.3505; 0.4430 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0346   19.3936    0.0000 [ 0.5875; 0.7666 ] 
#>   eta3 ~ eta1       0.6634      0.0327   20.2617    0.0000 [ 0.5719; 0.7412 ] 
#>   eta3 ~ eta2       0.3052      0.0818    3.7310    0.0002 [ 0.0981; 0.5211 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0558    3.6703    0.0002 [ 0.0652; 0.3538 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.03461619 19.393627 8.735490e-84
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.06926886  6.619234 3.610653e-11
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.08178837  3.730985 1.907330e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1          0.5875482          0.7665636          0.6090444          0.7450673
#> 2          0.2679346          0.6261540          0.3109497          0.5831389
#> 3          0.0980937          0.5210569          0.1488833          0.4702673
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5859182          0.7443698          0.6014805          0.7200949
#> 2          0.3701335          0.6159434          0.3736196          0.6111073
#> 3          0.1301087          0.4217187          0.1450137          0.4163313
```
