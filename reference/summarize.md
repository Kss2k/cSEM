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
#>  Random seed                        = 3570613
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
#>   eta2 ~ eta1      0.6713      0.0422   15.9206    0.0000 [ 0.5975; 0.7422 ] 
#>   eta3 ~ eta1      0.4585      0.0652    7.0307    0.0000 [ 0.3642; 0.5578 ] 
#>   eta3 ~ eta2      0.3052      0.0645    4.7294    0.0000 [ 0.1705; 0.4069 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0456   14.5407    0.0000 [ 0.5824; 0.7338 ] 
#>   eta1 =~ y12      0.6493      0.0385   16.8557    0.0000 [ 0.5923; 0.7239 ] 
#>   eta1 =~ y13      0.7613      0.0380   20.0502    0.0000 [ 0.7015; 0.8354 ] 
#>   eta2 =~ y21      0.5165      0.0647    7.9816    0.0000 [ 0.3890; 0.6196 ] 
#>   eta2 =~ y22      0.7554      0.0303   24.9013    0.0000 [ 0.7006; 0.7984 ] 
#>   eta2 =~ y23      0.7997      0.0405   19.7462    0.0000 [ 0.7271; 0.8654 ] 
#>   eta3 =~ y31      0.8223      0.0325   25.3366    0.0000 [ 0.7604; 0.8676 ] 
#>   eta3 =~ y32      0.6581      0.0444   14.8331    0.0000 [ 0.5904; 0.7387 ] 
#>   eta3 =~ y33      0.7474      0.0496   15.0776    0.0000 [ 0.6761; 0.8377 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0220   18.0191    0.0000 [ 0.3474; 0.4383 ] 
#>   eta1 <~ y12      0.3873      0.0223   17.3824    0.0000 [ 0.3509; 0.4284 ] 
#>   eta1 <~ y13      0.4542      0.0216   21.0400    0.0000 [ 0.4129; 0.4815 ] 
#>   eta2 <~ y21      0.3058      0.0334    9.1592    0.0000 [ 0.2457; 0.3631 ] 
#>   eta2 <~ y22      0.4473      0.0243   18.4299    0.0000 [ 0.4057; 0.4891 ] 
#>   eta2 <~ y23      0.4735      0.0202   23.4460    0.0000 [ 0.4410; 0.5129 ] 
#>   eta3 <~ y31      0.4400      0.0217   20.2482    0.0000 [ 0.3975; 0.4731 ] 
#>   eta3 <~ y32      0.3521      0.0203   17.3654    0.0000 [ 0.3219; 0.3905 ] 
#>   eta3 <~ y33      0.3999      0.0231   17.3336    0.0000 [ 0.3593; 0.4398 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0422   15.9206    0.0000 [ 0.5975; 0.7422 ] 
#>   eta3 ~ eta1       0.6634      0.0357   18.5830    0.0000 [ 0.5729; 0.7203 ] 
#>   eta3 ~ eta2       0.3052      0.0645    4.7294    0.0000 [ 0.1705; 0.4069 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0446    4.5881    0.0000 [ 0.1109; 0.2829 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04560098 14.540692  6.691253e-48
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03851975 16.855717  9.524646e-64
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03797199 20.050194  2.010603e-89
#> 4 eta2 =~ y21  Common factor 0.5164548 0.06470559  7.981611  1.444358e-15
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03033523 24.901329 7.197501e-137
#> 6 eta2 =~ y23  Common factor 0.7996637 0.04049710 19.746197  8.649079e-87
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03245416 25.336579 1.263368e-141
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04436500 14.833066  8.954793e-50
#> 9 eta3 =~ y33  Common factor 0.7474241 0.04957188 15.077582  2.274485e-51
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5824028          0.7338496
#> 2          0.5923206          0.7238845
#> 3          0.7015427          0.8354021
#> 4          0.3889529          0.6196103
#> 5          0.7006419          0.7984007
#> 6          0.7270519          0.8654085
#> 7          0.7603857          0.8675651
#> 8          0.5903726          0.7386704
#> 9          0.6760622          0.8377102

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
#>  Random seed                        = 3570613
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
#>   eta2 ~ eta1      0.6713      0.0422   15.9206    0.0000 [ 0.5665; 0.7845 ] 
#>   eta3 ~ eta1      0.4585      0.0652    7.0307    0.0000 [ 0.2791; 0.6164 ] 
#>   eta3 ~ eta2      0.3052      0.0645    4.7294    0.0000 [ 0.1514; 0.4851 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0456   14.5407    0.0000 [ 0.5360; 0.7718 ] 
#>   eta1 =~ y12      0.6493      0.0385   16.8557    0.0000 [ 0.5401; 0.7393 ] 
#>   eta1 =~ y13      0.7613      0.0380   20.0502    0.0000 [ 0.6638; 0.8601 ] 
#>   eta2 =~ y21      0.5165      0.0647    7.9816    0.0000 [ 0.3436; 0.6782 ] 
#>   eta2 =~ y22      0.7554      0.0303   24.9013    0.0000 [ 0.6882; 0.8451 ] 
#>   eta2 =~ y23      0.7997      0.0405   19.7462    0.0000 [ 0.6897; 0.8991 ] 
#>   eta3 =~ y31      0.8223      0.0325   25.3366    0.0000 [ 0.7439; 0.9117 ] 
#>   eta3 =~ y32      0.6581      0.0444   14.8331    0.0000 [ 0.5357; 0.7651 ] 
#>   eta3 =~ y33      0.7474      0.0496   15.0776    0.0000 [ 0.6106; 0.8670 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0220   18.0191    0.0000 [ 0.3389; 0.4524 ] 
#>   eta1 <~ y12      0.3873      0.0223   17.3824    0.0000 [ 0.3291; 0.4443 ] 
#>   eta1 <~ y13      0.4542      0.0216   21.0400    0.0000 [ 0.4046; 0.5163 ] 
#>   eta2 <~ y21      0.3058      0.0334    9.1592    0.0000 [ 0.2169; 0.3896 ] 
#>   eta2 <~ y22      0.4473      0.0243   18.4299    0.0000 [ 0.3910; 0.5165 ] 
#>   eta2 <~ y23      0.4735      0.0202   23.4460    0.0000 [ 0.4184; 0.5229 ] 
#>   eta3 <~ y31      0.4400      0.0217   20.2482    0.0000 [ 0.3898; 0.5022 ] 
#>   eta3 <~ y32      0.3521      0.0203   17.3654    0.0000 [ 0.2985; 0.4033 ] 
#>   eta3 <~ y33      0.3999      0.0231   17.3336    0.0000 [ 0.3389; 0.4582 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0422   15.9206    0.0000 [ 0.5665; 0.7845 ] 
#>   eta3 ~ eta1       0.6634      0.0357   18.5830    0.0000 [ 0.5705; 0.7551 ] 
#>   eta3 ~ eta2       0.3052      0.0645    4.7294    0.0000 [ 0.1514; 0.4851 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0446    4.5881    0.0000 [ 0.0996; 0.3305 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04216764 15.920585 4.560921e-57
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.06521526  7.030667 2.055490e-12
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.06452241  4.729382 2.252042e-06
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1          0.5664674          0.7845346          0.5926530          0.7583490
#> 2          0.2791483          0.6164048          0.3196462          0.5759069
#> 3          0.1513787          0.4850522          0.1914463          0.4449845
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5928342          0.7440040          0.5974519          0.7422124
#> 2          0.2860189          0.6025490          0.3641741          0.5577629
#> 3          0.1195344          0.4175417          0.1704814          0.4069108
```
