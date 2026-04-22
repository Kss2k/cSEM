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
#>  Random seed                        = -1453344813
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
#>   eta2 ~ eta1      0.6713      0.0353   19.0334    0.0000 [ 0.5963; 0.7277 ] 
#>   eta3 ~ eta1      0.4585      0.0739    6.2070    0.0000 [ 0.3499; 0.5869 ] 
#>   eta3 ~ eta2      0.3052      0.0767    3.9777    0.0001 [ 0.1623; 0.4268 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0308   21.5512    0.0000 [ 0.6202; 0.7233 ] 
#>   eta1 =~ y12      0.6493      0.0454   14.2990    0.0000 [ 0.5645; 0.7113 ] 
#>   eta1 =~ y13      0.7613      0.0325   23.4145    0.0000 [ 0.7046; 0.8165 ] 
#>   eta2 =~ y21      0.5165      0.0556    9.2826    0.0000 [ 0.4377; 0.6136 ] 
#>   eta2 =~ y22      0.7554      0.0383   19.7329    0.0000 [ 0.6823; 0.8232 ] 
#>   eta2 =~ y23      0.7997      0.0406   19.6957    0.0000 [ 0.7321; 0.8630 ] 
#>   eta3 =~ y31      0.8223      0.0374   21.9788    0.0000 [ 0.7432; 0.8699 ] 
#>   eta3 =~ y32      0.6581      0.0461   14.2742    0.0000 [ 0.5732; 0.7332 ] 
#>   eta3 =~ y33      0.7474      0.0354   21.0946    0.0000 [ 0.6795; 0.8101 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0211   18.7452    0.0000 [ 0.3589; 0.4309 ] 
#>   eta1 <~ y12      0.3873      0.0199   19.4312    0.0000 [ 0.3518; 0.4152 ] 
#>   eta1 <~ y13      0.4542      0.0187   24.2688    0.0000 [ 0.4238; 0.4922 ] 
#>   eta2 <~ y21      0.3058      0.0306   10.0100    0.0000 [ 0.2580; 0.3478 ] 
#>   eta2 <~ y22      0.4473      0.0237   18.8832    0.0000 [ 0.4071; 0.4926 ] 
#>   eta2 <~ y23      0.4735      0.0202   23.4355    0.0000 [ 0.4433; 0.5087 ] 
#>   eta3 <~ y31      0.4400      0.0209   21.0719    0.0000 [ 0.3984; 0.4866 ] 
#>   eta3 <~ y32      0.3521      0.0188   18.7368    0.0000 [ 0.3187; 0.3901 ] 
#>   eta3 <~ y33      0.3999      0.0217   18.4382    0.0000 [ 0.3731; 0.4425 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0353   19.0334    0.0000 [ 0.5963; 0.7277 ] 
#>   eta3 ~ eta1       0.6634      0.0450   14.7578    0.0000 [ 0.5822; 0.7332 ] 
#>   eta3 ~ eta2       0.3052      0.0767    3.9777    0.0001 [ 0.1623; 0.4268 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0475    4.3124    0.0000 [ 0.1179; 0.2722 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03076723 21.551173 5.162289e-103
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04540708 14.299045  2.218338e-46
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03251606 23.414457 3.044786e-121
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05563686  9.282602  1.653893e-20
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03828068 19.732871  1.125899e-86
#> 6 eta2 =~ y23  Common factor 0.7996637 0.04060101 19.695659  2.349217e-86
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03741227 21.978816 4.592881e-107
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04610206 14.274177  3.170075e-46
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03543205 21.094579  8.919580e-99
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.6202109          0.7232963
#> 2          0.5644599          0.7112735
#> 3          0.7045702          0.8165178
#> 4          0.4376635          0.6136291
#> 5          0.6823015          0.8232473
#> 6          0.7321326          0.8630174
#> 7          0.7432339          0.8699056
#> 8          0.5732048          0.7331547
#> 9          0.6794963          0.8100977

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
#>  Random seed                        = -1453344813
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
#>   eta2 ~ eta1      0.6713      0.0353   19.0334    0.0000 [ 0.5920; 0.7744 ] 
#>   eta3 ~ eta1      0.4585      0.0739    6.2070    0.0000 [ 0.2582; 0.6402 ] 
#>   eta3 ~ eta2      0.3052      0.0767    3.9777    0.0001 [ 0.1167; 0.5134 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0308   21.5512    0.0000 [ 0.5858; 0.7449 ] 
#>   eta1 =~ y12      0.6493      0.0454   14.2990    0.0000 [ 0.5335; 0.7683 ] 
#>   eta1 =~ y13      0.7613      0.0325   23.4145    0.0000 [ 0.6767; 0.8449 ] 
#>   eta2 =~ y21      0.5165      0.0556    9.2826    0.0000 [ 0.3781; 0.6658 ] 
#>   eta2 =~ y22      0.7554      0.0383   19.7329    0.0000 [ 0.6604; 0.8583 ] 
#>   eta2 =~ y23      0.7997      0.0406   19.6957    0.0000 [ 0.6906; 0.9006 ] 
#>   eta3 =~ y31      0.8223      0.0374   21.9788    0.0000 [ 0.7394; 0.9329 ] 
#>   eta3 =~ y32      0.6581      0.0461   14.2742    0.0000 [ 0.5338; 0.7723 ] 
#>   eta3 =~ y33      0.7474      0.0354   21.0946    0.0000 [ 0.6572; 0.8404 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0211   18.7452    0.0000 [ 0.3414; 0.4505 ] 
#>   eta1 <~ y12      0.3873      0.0199   19.4312    0.0000 [ 0.3364; 0.4395 ] 
#>   eta1 <~ y13      0.4542      0.0187   24.2688    0.0000 [ 0.4045; 0.5013 ] 
#>   eta2 <~ y21      0.3058      0.0306   10.0100    0.0000 [ 0.2296; 0.3876 ] 
#>   eta2 <~ y22      0.4473      0.0237   18.8832    0.0000 [ 0.3873; 0.5098 ] 
#>   eta2 <~ y23      0.4735      0.0202   23.4355    0.0000 [ 0.4179; 0.5224 ] 
#>   eta3 <~ y31      0.4400      0.0209   21.0719    0.0000 [ 0.3903; 0.4983 ] 
#>   eta3 <~ y32      0.3521      0.0188   18.7368    0.0000 [ 0.2987; 0.3959 ] 
#>   eta3 <~ y33      0.3999      0.0217   18.4382    0.0000 [ 0.3416; 0.4538 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0353   19.0334    0.0000 [ 0.5920; 0.7744 ] 
#>   eta3 ~ eta1       0.6634      0.0450   14.7578    0.0000 [ 0.5490; 0.7814 ] 
#>   eta3 ~ eta2       0.3052      0.0767    3.9777    0.0001 [ 0.1167; 0.5134 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0475    4.3124    0.0000 [ 0.0932; 0.3389 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.03527136 19.033389 9.022509e-81
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.07386943  6.206989 5.400934e-10
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.07671620  3.977662 6.959618e-05
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1          0.5919751          0.7743786          0.6138782          0.7524755
#> 2          0.2581646          0.6401756          0.3040366          0.5943035
#> 3          0.1166679          0.5134007          0.1643077          0.4657609
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5768399          0.7373283          0.5962632          0.7277117
#> 2          0.3215229          0.6128767          0.3498639          0.5869013
#> 3          0.1426103          0.4311623          0.1623392          0.4268206
```
