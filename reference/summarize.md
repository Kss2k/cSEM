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
#>  Random seed                        = 573825297
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
#>   eta2 ~ eta1      0.6713      0.0523   12.8257    0.0000 [ 0.5528; 0.7542 ] 
#>   eta3 ~ eta1      0.4585      0.0810    5.6581    0.0000 [ 0.3404; 0.6044 ] 
#>   eta3 ~ eta2      0.3052      0.0787    3.8779    0.0001 [ 0.1767; 0.4140 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0339   19.5701    0.0000 [ 0.5971; 0.7162 ] 
#>   eta1 =~ y12      0.6493      0.0404   16.0796    0.0000 [ 0.5796; 0.7098 ] 
#>   eta1 =~ y13      0.7613      0.0393   19.3646    0.0000 [ 0.6598; 0.8209 ] 
#>   eta2 =~ y21      0.5165      0.0509   10.1551    0.0000 [ 0.4255; 0.6145 ] 
#>   eta2 =~ y22      0.7554      0.0431   17.5294    0.0000 [ 0.6787; 0.8387 ] 
#>   eta2 =~ y23      0.7997      0.0407   19.6571    0.0000 [ 0.7320; 0.8639 ] 
#>   eta3 =~ y31      0.8223      0.0351   23.3988    0.0000 [ 0.7539; 0.8732 ] 
#>   eta3 =~ y32      0.6581      0.0407   16.1852    0.0000 [ 0.5573; 0.7248 ] 
#>   eta3 =~ y33      0.7474      0.0379   19.7370    0.0000 [ 0.6837; 0.8324 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0184   21.5057    0.0000 [ 0.3608; 0.4238 ] 
#>   eta1 <~ y12      0.3873      0.0222   17.4263    0.0000 [ 0.3518; 0.4253 ] 
#>   eta1 <~ y13      0.4542      0.0217   20.9006    0.0000 [ 0.4112; 0.4941 ] 
#>   eta2 <~ y21      0.3058      0.0291   10.4947    0.0000 [ 0.2597; 0.3651 ] 
#>   eta2 <~ y22      0.4473      0.0224   19.9474    0.0000 [ 0.4100; 0.4911 ] 
#>   eta2 <~ y23      0.4735      0.0228   20.7668    0.0000 [ 0.4340; 0.5064 ] 
#>   eta3 <~ y31      0.4400      0.0169   26.0202    0.0000 [ 0.4092; 0.4705 ] 
#>   eta3 <~ y32      0.3521      0.0212   16.6013    0.0000 [ 0.3145; 0.3930 ] 
#>   eta3 <~ y33      0.3999      0.0186   21.5078    0.0000 [ 0.3750; 0.4382 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0523   12.8257    0.0000 [ 0.5528; 0.7542 ] 
#>   eta3 ~ eta1       0.6634      0.0393   16.8941    0.0000 [ 0.5792; 0.7301 ] 
#>   eta3 ~ eta2       0.3052      0.0787    3.8779    0.0001 [ 0.1767; 0.4140 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0527    3.8868    0.0001 [ 0.1144; 0.2942 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err   t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03388182 19.57008  2.782490e-85
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04037900 16.07959  3.546976e-58
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03931640 19.36459  1.535835e-83
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05085685 10.15507  3.145902e-24
#> 5 eta2 =~ y22  Common factor 0.7553877 0.04309254 17.52943  8.542381e-69
#> 6 eta2 =~ y23  Common factor 0.7996637 0.04068067 19.65709  5.027431e-86
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03514182 23.39883 4.392762e-121
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04065879 16.18516  6.418645e-59
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03786925 19.73697  1.038249e-86
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5971031          0.7161682
#> 2          0.5795785          0.7098003
#> 3          0.6598301          0.8208520
#> 4          0.4254842          0.6145230
#> 5          0.6787021          0.8386639
#> 6          0.7319919          0.8639366
#> 7          0.7538984          0.8732475
#> 8          0.5572715          0.7248017
#> 9          0.6836650          0.8324391

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
#>  Random seed                        = 573825297
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
#>   eta2 ~ eta1      0.6713      0.0523   12.8257    0.0000 [ 0.5385; 0.8092 ] 
#>   eta3 ~ eta1      0.4585      0.0810    5.6581    0.0000 [ 0.2374; 0.6565 ] 
#>   eta3 ~ eta2      0.3052      0.0787    3.8779    0.0001 [ 0.1181; 0.5250 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0339   19.5701    0.0000 [ 0.5801; 0.7553 ] 
#>   eta1 =~ y12      0.6493      0.0404   16.0796    0.0000 [ 0.5483; 0.7572 ] 
#>   eta1 =~ y13      0.7613      0.0393   19.3646    0.0000 [ 0.6629; 0.8662 ] 
#>   eta2 =~ y21      0.5165      0.0509   10.1551    0.0000 [ 0.3876; 0.6506 ] 
#>   eta2 =~ y22      0.7554      0.0431   17.5294    0.0000 [ 0.6429; 0.8658 ] 
#>   eta2 =~ y23      0.7997      0.0407   19.6571    0.0000 [ 0.7024; 0.9128 ] 
#>   eta3 =~ y31      0.8223      0.0351   23.3988    0.0000 [ 0.7374; 0.9191 ] 
#>   eta3 =~ y32      0.6581      0.0407   16.1852    0.0000 [ 0.5560; 0.7663 ] 
#>   eta3 =~ y33      0.7474      0.0379   19.7370    0.0000 [ 0.6384; 0.8343 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0184   21.5057    0.0000 [ 0.3478; 0.4429 ] 
#>   eta1 <~ y12      0.3873      0.0222   17.4263    0.0000 [ 0.3290; 0.4440 ] 
#>   eta1 <~ y13      0.4542      0.0217   20.9006    0.0000 [ 0.3965; 0.5088 ] 
#>   eta2 <~ y21      0.3058      0.0291   10.4947    0.0000 [ 0.2303; 0.3810 ] 
#>   eta2 <~ y22      0.4473      0.0224   19.9474    0.0000 [ 0.3861; 0.5021 ] 
#>   eta2 <~ y23      0.4735      0.0228   20.7668    0.0000 [ 0.4165; 0.5344 ] 
#>   eta3 <~ y31      0.4400      0.0169   26.0202    0.0000 [ 0.4003; 0.4877 ] 
#>   eta3 <~ y32      0.3521      0.0212   16.6013    0.0000 [ 0.2996; 0.4093 ] 
#>   eta3 <~ y33      0.3999      0.0186   21.5078    0.0000 [ 0.3467; 0.4429 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0523   12.8257    0.0000 [ 0.5385; 0.8092 ] 
#>   eta3 ~ eta1       0.6634      0.0393   16.8941    0.0000 [ 0.5627; 0.7657 ] 
#>   eta3 ~ eta2       0.3052      0.0787    3.8779    0.0001 [ 0.1181; 0.5250 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0527    3.8868    0.0001 [ 0.0810; 0.3536 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.05234266 12.825741 1.176568e-37
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.08103515  5.658122 1.530385e-08
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.07868904  3.877937 1.053462e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1          0.5384667          0.8091534          0.5709709          0.7766492
#> 2          0.2373988          0.6564668          0.2877206          0.6061449
#> 3          0.1180762          0.5250115          0.1669412          0.4761465
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5480070          0.7630984          0.5528040          0.7542243
#> 2          0.2608680          0.6158544          0.3404109          0.6044324
#> 3          0.1673803          0.4803037          0.1767033          0.4140005
```
