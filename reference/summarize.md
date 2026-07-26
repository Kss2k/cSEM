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
#>  Random seed                        = 1204729849
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
#>   eta2 ~ eta1      0.6713      0.0458   14.6491    0.0000 [ 0.5932; 0.7545 ] 
#>   eta3 ~ eta1      0.4585      0.0828    5.5395    0.0000 [ 0.3526; 0.6380 ] 
#>   eta3 ~ eta2      0.3052      0.0857    3.5616    0.0004 [ 0.1190; 0.4353 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0426   15.5741    0.0000 [ 0.5813; 0.7367 ] 
#>   eta1 =~ y12      0.6493      0.0389   16.6996    0.0000 [ 0.5673; 0.7105 ] 
#>   eta1 =~ y13      0.7613      0.0319   23.8595    0.0000 [ 0.7125; 0.8356 ] 
#>   eta2 =~ y21      0.5165      0.0574    8.9936    0.0000 [ 0.4162; 0.5990 ] 
#>   eta2 =~ y22      0.7554      0.0355   21.3072    0.0000 [ 0.6817; 0.8089 ] 
#>   eta2 =~ y23      0.7997      0.0450   17.7754    0.0000 [ 0.7274; 0.8850 ] 
#>   eta3 =~ y31      0.8223      0.0334   24.5868    0.0000 [ 0.7370; 0.8737 ] 
#>   eta3 =~ y32      0.6581      0.0340   19.3504    0.0000 [ 0.5913; 0.7339 ] 
#>   eta3 =~ y33      0.7474      0.0372   20.0682    0.0000 [ 0.6900; 0.8145 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0218   18.1470    0.0000 [ 0.3585; 0.4357 ] 
#>   eta1 <~ y12      0.3873      0.0218   17.7982    0.0000 [ 0.3334; 0.4220 ] 
#>   eta1 <~ y13      0.4542      0.0190   23.8584    0.0000 [ 0.4161; 0.4835 ] 
#>   eta2 <~ y21      0.3058      0.0305   10.0137    0.0000 [ 0.2612; 0.3629 ] 
#>   eta2 <~ y22      0.4473      0.0248   18.0639    0.0000 [ 0.4134; 0.5004 ] 
#>   eta2 <~ y23      0.4735      0.0226   20.9754    0.0000 [ 0.4377; 0.5163 ] 
#>   eta3 <~ y31      0.4400      0.0170   25.9114    0.0000 [ 0.4089; 0.4631 ] 
#>   eta3 <~ y32      0.3521      0.0166   21.2561    0.0000 [ 0.3247; 0.3888 ] 
#>   eta3 <~ y33      0.3999      0.0190   21.1043    0.0000 [ 0.3742; 0.4394 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0458   14.6491    0.0000 [ 0.5932; 0.7545 ] 
#>   eta3 ~ eta1       0.6634      0.0400   16.5836    0.0000 [ 0.6065; 0.7395 ] 
#>   eta3 ~ eta2       0.3052      0.0857    3.5616    0.0004 [ 0.1190; 0.4353 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0566    3.6218    0.0003 [ 0.0766; 0.2872 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04257505 15.574141  1.091097e-54
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03887980 16.699621  1.319004e-62
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03190952 23.859519 8.066093e-126
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05742466  8.993607  2.392466e-19
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03545219 21.307220 9.730292e-101
#> 6 eta2 =~ y23  Common factor 0.7996637 0.04498708 17.775409  1.095954e-70
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03344381 24.586829 1.747341e-133
#> 8 eta3 =~ y32  Common factor 0.6580689 0.03400806 19.350378  2.023521e-83
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03724414 20.068232  1.398929e-89
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5812647          0.7367121
#> 2          0.5672817          0.7104781
#> 3          0.7125346          0.8355654
#> 4          0.4162272          0.5989818
#> 5          0.6817271          0.8089047
#> 6          0.7273860          0.8849951
#> 7          0.7370043          0.8737333
#> 8          0.5913249          0.7338874
#> 9          0.6899619          0.8145133

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
#>  Random seed                        = 1204729849
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
#>   eta2 ~ eta1      0.6713      0.0458   14.6491    0.0000 [ 0.5580; 0.7950 ] 
#>   eta3 ~ eta1      0.4585      0.0828    5.5395    0.0000 [ 0.2283; 0.6563 ] 
#>   eta3 ~ eta2      0.3052      0.0857    3.5616    0.0004 [ 0.0985; 0.5416 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0426   15.5741    0.0000 [ 0.5549; 0.7751 ] 
#>   eta1 =~ y12      0.6493      0.0389   16.6996    0.0000 [ 0.5482; 0.7492 ] 
#>   eta1 =~ y13      0.7613      0.0319   23.8595    0.0000 [ 0.6746; 0.8396 ] 
#>   eta2 =~ y21      0.5165      0.0574    8.9936    0.0000 [ 0.3782; 0.6752 ] 
#>   eta2 =~ y22      0.7554      0.0355   21.3072    0.0000 [ 0.6741; 0.8574 ] 
#>   eta2 =~ y23      0.7997      0.0450   17.7754    0.0000 [ 0.6898; 0.9225 ] 
#>   eta3 =~ y31      0.8223      0.0334   24.5868    0.0000 [ 0.7465; 0.9195 ] 
#>   eta3 =~ y32      0.6581      0.0340   19.3504    0.0000 [ 0.5598; 0.7357 ] 
#>   eta3 =~ y33      0.7474      0.0372   20.0682    0.0000 [ 0.6544; 0.8470 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0218   18.1470    0.0000 [ 0.3416; 0.4543 ] 
#>   eta1 <~ y12      0.3873      0.0218   17.7982    0.0000 [ 0.3318; 0.4443 ] 
#>   eta1 <~ y13      0.4542      0.0190   23.8584    0.0000 [ 0.4036; 0.5020 ] 
#>   eta2 <~ y21      0.3058      0.0305   10.0137    0.0000 [ 0.2278; 0.3858 ] 
#>   eta2 <~ y22      0.4473      0.0248   18.0639    0.0000 [ 0.3812; 0.5092 ] 
#>   eta2 <~ y23      0.4735      0.0226   20.9754    0.0000 [ 0.4106; 0.5274 ] 
#>   eta3 <~ y31      0.4400      0.0170   25.9114    0.0000 [ 0.4006; 0.4884 ] 
#>   eta3 <~ y32      0.3521      0.0166   21.2561    0.0000 [ 0.3028; 0.3885 ] 
#>   eta3 <~ y33      0.3999      0.0190   21.1043    0.0000 [ 0.3516; 0.4496 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0458   14.6491    0.0000 [ 0.5580; 0.7950 ] 
#>   eta3 ~ eta1       0.6634      0.0400   16.5836    0.0000 [ 0.5559; 0.7627 ] 
#>   eta3 ~ eta2       0.3052      0.0857    3.5616    0.0004 [ 0.0985; 0.5416 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0566    3.6218    0.0003 [ 0.0707; 0.3632 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04582748 14.649145 1.364214e-48
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.08276998  5.539530 3.032844e-08
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.08567721  3.561637 3.685497e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.55798191          0.7949758          0.5864402          0.7665174
#> 2         0.22829892          0.6563385          0.2796981          0.6049393
#> 3         0.09854784          0.5416220          0.1517524          0.4884175
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1         0.58664497          0.7663928          0.5931699          0.7545049
#> 2         0.33653425          0.6734806          0.3525862          0.6379606
#> 3         0.09135408          0.4470390          0.1189501          0.4352757
```
