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
#>  Random seed                        = -671701414
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
#>   eta2 ~ eta1      0.6713      0.0376   17.8565    0.0000 [ 0.6122; 0.7571 ] 
#>   eta3 ~ eta1      0.4585      0.0876    5.2343    0.0000 [ 0.3100; 0.6377 ] 
#>   eta3 ~ eta2      0.3052      0.0904    3.3742    0.0007 [ 0.1613; 0.4575 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0383   17.3221    0.0000 [ 0.5972; 0.7318 ] 
#>   eta1 =~ y12      0.6493      0.0378   17.1634    0.0000 [ 0.5949; 0.7421 ] 
#>   eta1 =~ y13      0.7613      0.0307   24.7793    0.0000 [ 0.7041; 0.8094 ] 
#>   eta2 =~ y21      0.5165      0.0451   11.4450    0.0000 [ 0.4376; 0.6161 ] 
#>   eta2 =~ y22      0.7554      0.0377   20.0307    0.0000 [ 0.6705; 0.8116 ] 
#>   eta2 =~ y23      0.7997      0.0377   21.1911    0.0000 [ 0.7290; 0.8510 ] 
#>   eta3 =~ y31      0.8223      0.0354   23.2308    0.0000 [ 0.7629; 0.8900 ] 
#>   eta3 =~ y32      0.6581      0.0416   15.8114    0.0000 [ 0.6022; 0.7310 ] 
#>   eta3 =~ y33      0.7474      0.0341   21.9480    0.0000 [ 0.6945; 0.8167 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0200   19.7849    0.0000 [ 0.3452; 0.4323 ] 
#>   eta1 <~ y12      0.3873      0.0191   20.2774    0.0000 [ 0.3555; 0.4301 ] 
#>   eta1 <~ y13      0.4542      0.0209   21.7199    0.0000 [ 0.4109; 0.4956 ] 
#>   eta2 <~ y21      0.3058      0.0219   13.9612    0.0000 [ 0.2713; 0.3471 ] 
#>   eta2 <~ y22      0.4473      0.0208   21.4964    0.0000 [ 0.4056; 0.4835 ] 
#>   eta2 <~ y23      0.4735      0.0228   20.8108    0.0000 [ 0.4307; 0.5077 ] 
#>   eta3 <~ y31      0.4400      0.0168   26.1615    0.0000 [ 0.4117; 0.4783 ] 
#>   eta3 <~ y32      0.3521      0.0162   21.7163    0.0000 [ 0.3287; 0.3809 ] 
#>   eta3 <~ y33      0.3999      0.0212   18.8603    0.0000 [ 0.3609; 0.4404 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0376   17.8565    0.0000 [ 0.6122; 0.7571 ] 
#>   eta3 ~ eta1       0.6634      0.0376   17.6290    0.0000 [ 0.5849; 0.7457 ] 
#>   eta3 ~ eta2       0.3052      0.0904    3.3742    0.0007 [ 0.1613; 0.4575 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0604    3.3933    0.0007 [ 0.1080; 0.3182 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err   t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03827894 17.32206  3.206947e-67
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03782921 17.16340  4.990169e-66
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03072506 24.77931 1.498582e-135
#> 4 eta2 =~ y21  Common factor 0.5164548 0.04512500 11.44498  2.491531e-30
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03771147 20.03072  2.973546e-89
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03773590 21.19106  1.154712e-99
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03539607 23.23075 2.226791e-119
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04161992 15.81139  2.596587e-56
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03405434 21.94798 9.052871e-107
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5972440          0.7318276
#> 2          0.5948631          0.7421128
#> 3          0.7041188          0.8094491
#> 4          0.4376452          0.6160628
#> 5          0.6704786          0.8116182
#> 6          0.7290302          0.8510264
#> 7          0.7629443          0.8900033
#> 8          0.6021916          0.7309592
#> 9          0.6945396          0.8167302

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
#>  Random seed                        = -671701414
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
#>   eta2 ~ eta1      0.6713      0.0376   17.8565    0.0000 [ 0.5747; 0.7692 ] 
#>   eta3 ~ eta1      0.4585      0.0876    5.2343    0.0000 [ 0.2191; 0.6721 ] 
#>   eta3 ~ eta2      0.3052      0.0904    3.3742    0.0007 [ 0.0723; 0.5400 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0383   17.3221    0.0000 [ 0.5625; 0.7604 ] 
#>   eta1 =~ y12      0.6493      0.0378   17.1634    0.0000 [ 0.5342; 0.7299 ] 
#>   eta1 =~ y13      0.7613      0.0307   24.7793    0.0000 [ 0.6830; 0.8419 ] 
#>   eta2 =~ y21      0.5165      0.0451   11.4450    0.0000 [ 0.3909; 0.6243 ] 
#>   eta2 =~ y22      0.7554      0.0377   20.0307    0.0000 [ 0.6653; 0.8603 ] 
#>   eta2 =~ y23      0.7997      0.0377   21.1911    0.0000 [ 0.7086; 0.9038 ] 
#>   eta3 =~ y31      0.8223      0.0354   23.2308    0.0000 [ 0.7282; 0.9112 ] 
#>   eta3 =~ y32      0.6581      0.0416   15.8114    0.0000 [ 0.5473; 0.7626 ] 
#>   eta3 =~ y33      0.7474      0.0341   21.9480    0.0000 [ 0.6528; 0.8289 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0200   19.7849    0.0000 [ 0.3481; 0.4515 ] 
#>   eta1 <~ y12      0.3873      0.0191   20.2774    0.0000 [ 0.3329; 0.4317 ] 
#>   eta1 <~ y13      0.4542      0.0209   21.7199    0.0000 [ 0.4064; 0.5146 ] 
#>   eta2 <~ y21      0.3058      0.0219   13.9612    0.0000 [ 0.2429; 0.3562 ] 
#>   eta2 <~ y22      0.4473      0.0208   21.4964    0.0000 [ 0.3960; 0.5036 ] 
#>   eta2 <~ y23      0.4735      0.0228   20.8108    0.0000 [ 0.4164; 0.5340 ] 
#>   eta3 <~ y31      0.4400      0.0168   26.1615    0.0000 [ 0.3989; 0.4859 ] 
#>   eta3 <~ y32      0.3521      0.0162   21.7163    0.0000 [ 0.3119; 0.3957 ] 
#>   eta3 <~ y33      0.3999      0.0212   18.8603    0.0000 [ 0.3449; 0.4545 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0376   17.8565    0.0000 [ 0.5747; 0.7692 ] 
#>   eta3 ~ eta1       0.6634      0.0376   17.6290    0.0000 [ 0.5541; 0.7487 ] 
#>   eta3 ~ eta2       0.3052      0.0904    3.3742    0.0007 [ 0.0723; 0.5400 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0604    3.3933    0.0007 [ 0.0497; 0.3619 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.03759602 17.856503 2.572613e-71
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.08759690  5.234281 1.656283e-07
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.09043722  3.374176 7.403694e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1          0.5747389          0.7691643          0.5980856          0.7458176
#> 2          0.2190985          0.6721002          0.2734952          0.6177036
#> 3          0.0723333          0.5400235          0.1284938          0.4838631
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.6085072          0.7646975          0.6121679          0.7571279
#> 2          0.3046032          0.6497308          0.3100170          0.6376930
#> 3          0.1597613          0.5110361          0.1612605          0.4575015
```
