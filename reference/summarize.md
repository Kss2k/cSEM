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
#>  Random seed                        = 2006129414
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
#>   eta2 ~ eta1      0.6713      0.0426   15.7743    0.0000 [ 0.5806; 0.7457 ] 
#>   eta3 ~ eta1      0.4585      0.0905    5.0646    0.0000 [ 0.3205; 0.6437 ] 
#>   eta3 ~ eta2      0.3052      0.0973    3.1365    0.0017 [ 0.0874; 0.4681 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0384   17.2472    0.0000 [ 0.5968; 0.7229 ] 
#>   eta1 =~ y12      0.6493      0.0376   17.2907    0.0000 [ 0.5968; 0.7097 ] 
#>   eta1 =~ y13      0.7613      0.0337   22.5861    0.0000 [ 0.7002; 0.8069 ] 
#>   eta2 =~ y21      0.5165      0.0523    9.8778    0.0000 [ 0.4104; 0.6092 ] 
#>   eta2 =~ y22      0.7554      0.0281   26.8560    0.0000 [ 0.6959; 0.8149 ] 
#>   eta2 =~ y23      0.7997      0.0353   22.6835    0.0000 [ 0.7317; 0.8580 ] 
#>   eta3 =~ y31      0.8223      0.0323   25.4919    0.0000 [ 0.7919; 0.9164 ] 
#>   eta3 =~ y32      0.6581      0.0467   14.0911    0.0000 [ 0.5630; 0.7391 ] 
#>   eta3 =~ y33      0.7474      0.0331   22.6053    0.0000 [ 0.6912; 0.7978 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0236   16.7498    0.0000 [ 0.3656; 0.4458 ] 
#>   eta1 <~ y12      0.3873      0.0196   19.7602    0.0000 [ 0.3544; 0.4301 ] 
#>   eta1 <~ y13      0.4542      0.0183   24.7612    0.0000 [ 0.4187; 0.4834 ] 
#>   eta2 <~ y21      0.3058      0.0267   11.4568    0.0000 [ 0.2534; 0.3563 ] 
#>   eta2 <~ y22      0.4473      0.0196   22.7898    0.0000 [ 0.4043; 0.4844 ] 
#>   eta2 <~ y23      0.4735      0.0199   23.8354    0.0000 [ 0.4344; 0.5082 ] 
#>   eta3 <~ y31      0.4400      0.0204   21.5607    0.0000 [ 0.4156; 0.4953 ] 
#>   eta3 <~ y32      0.3521      0.0212   16.5803    0.0000 [ 0.3079; 0.3899 ] 
#>   eta3 <~ y33      0.3999      0.0151   26.5051    0.0000 [ 0.3723; 0.4240 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0426   15.7743    0.0000 [ 0.5806; 0.7457 ] 
#>   eta3 ~ eta1       0.6634      0.0332   19.9721    0.0000 [ 0.5996; 0.7099 ] 
#>   eta3 ~ eta2       0.3052      0.0973    3.1365    0.0017 [ 0.0874; 0.4681 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0669    3.0628    0.0022 [ 0.0653; 0.3291 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03844507 17.247202  1.174504e-66
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03755069 17.290705  5.527192e-67
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03370867 22.586056 5.942164e-113
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05228456  9.877769  5.197726e-23
#> 5 eta2 =~ y22  Common factor 0.7553877 0.02812729 26.856037 7.170711e-159
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03525314 22.683474 6.523012e-114
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03225636 25.491943 2.421551e-143
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04670088 14.091147  4.305112e-45
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03306410 22.605304 3.843213e-113
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5968371          0.7229385
#> 2          0.5967796          0.7097102
#> 3          0.7002013          0.8068629
#> 4          0.4103616          0.6092116
#> 5          0.6958715          0.8149080
#> 6          0.7316511          0.8580139
#> 7          0.7919229          0.9163913
#> 8          0.5629645          0.7390644
#> 9          0.6912061          0.7978066

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
#>  Random seed                        = 2006129414
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
#>   eta2 ~ eta1      0.6713      0.0426   15.7743    0.0000 [ 0.5548; 0.7749 ] 
#>   eta3 ~ eta1      0.4585      0.0905    5.0646    0.0000 [ 0.2357; 0.7039 ] 
#>   eta3 ~ eta2      0.3052      0.0973    3.1365    0.0017 [ 0.0431; 0.5463 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0384   17.2472    0.0000 [ 0.5650; 0.7638 ] 
#>   eta1 =~ y12      0.6493      0.0376   17.2907    0.0000 [ 0.5519; 0.7461 ] 
#>   eta1 =~ y13      0.7613      0.0337   22.5861    0.0000 [ 0.6791; 0.8534 ] 
#>   eta2 =~ y21      0.5165      0.0523    9.8778    0.0000 [ 0.3806; 0.6510 ] 
#>   eta2 =~ y22      0.7554      0.0281   26.8560    0.0000 [ 0.6800; 0.8254 ] 
#>   eta2 =~ y23      0.7997      0.0353   22.6835    0.0000 [ 0.7076; 0.8899 ] 
#>   eta3 =~ y31      0.8223      0.0323   25.4919    0.0000 [ 0.7283; 0.8951 ] 
#>   eta3 =~ y32      0.6581      0.0467   14.0911    0.0000 [ 0.5436; 0.7851 ] 
#>   eta3 =~ y33      0.7474      0.0331   22.6053    0.0000 [ 0.6662; 0.8372 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0236   16.7498    0.0000 [ 0.3337; 0.4558 ] 
#>   eta1 <~ y12      0.3873      0.0196   19.7602    0.0000 [ 0.3351; 0.4365 ] 
#>   eta1 <~ y13      0.4542      0.0183   24.7612    0.0000 [ 0.4080; 0.5028 ] 
#>   eta2 <~ y21      0.3058      0.0267   11.4568    0.0000 [ 0.2380; 0.3760 ] 
#>   eta2 <~ y22      0.4473      0.0196   22.7898    0.0000 [ 0.3965; 0.4980 ] 
#>   eta2 <~ y23      0.4735      0.0199   23.8354    0.0000 [ 0.4235; 0.5262 ] 
#>   eta3 <~ y31      0.4400      0.0204   21.5607    0.0000 [ 0.3818; 0.4873 ] 
#>   eta3 <~ y32      0.3521      0.0212   16.5803    0.0000 [ 0.3011; 0.4109 ] 
#>   eta3 <~ y33      0.3999      0.0151   26.5051    0.0000 [ 0.3636; 0.4416 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0426   15.7743    0.0000 [ 0.5548; 0.7749 ] 
#>   eta3 ~ eta1       0.6634      0.0332   19.9721    0.0000 [ 0.5802; 0.7519 ] 
#>   eta3 ~ eta2       0.3052      0.0973    3.1365    0.0017 [ 0.0431; 0.5463 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0669    3.0628    0.0022 [ 0.0233; 0.3692 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04255872 15.774284 4.676678e-56
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.09053090  5.064644 4.091646e-07
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.09729135  3.136467 1.709965e-03
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.55482430          0.7749140          0.5812528          0.7484855
#> 2         0.23568810          0.7038628          0.2919067          0.6476442
#> 3         0.04311795          0.5462538          0.1035347          0.4858371
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1         0.56975896          0.7474289         0.58060177          0.7456605
#> 2         0.25841050          0.6459391         0.32050164          0.6437439
#> 3         0.07767177          0.4926724         0.08735124          0.4680715
```
