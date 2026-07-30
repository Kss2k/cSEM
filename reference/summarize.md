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
#>  Random seed                        = 977748267
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
#>   eta2 ~ eta1      0.6713      0.0435   15.4453    0.0000 [ 0.5764; 0.7242 ] 
#>   eta3 ~ eta1      0.4585      0.0739    6.2068    0.0000 [ 0.3390; 0.6279 ] 
#>   eta3 ~ eta2      0.3052      0.0859    3.5510    0.0004 [ 0.0989; 0.4273 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0409   16.2065    0.0000 [ 0.5993; 0.7517 ] 
#>   eta1 =~ y12      0.6493      0.0392   16.5731    0.0000 [ 0.5554; 0.6952 ] 
#>   eta1 =~ y13      0.7613      0.0378   20.1255    0.0000 [ 0.7047; 0.8340 ] 
#>   eta2 =~ y21      0.5165      0.0584    8.8464    0.0000 [ 0.3855; 0.5787 ] 
#>   eta2 =~ y22      0.7554      0.0437   17.2692    0.0000 [ 0.6792; 0.8385 ] 
#>   eta2 =~ y23      0.7997      0.0351   22.7943    0.0000 [ 0.7212; 0.8544 ] 
#>   eta3 =~ y31      0.8223      0.0313   26.2966    0.0000 [ 0.7811; 0.8820 ] 
#>   eta3 =~ y32      0.6581      0.0434   15.1695    0.0000 [ 0.5675; 0.7274 ] 
#>   eta3 =~ y33      0.7474      0.0368   20.3208    0.0000 [ 0.6927; 0.8116 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0226   17.4797    0.0000 [ 0.3541; 0.4319 ] 
#>   eta1 <~ y12      0.3873      0.0188   20.5839    0.0000 [ 0.3462; 0.4192 ] 
#>   eta1 <~ y13      0.4542      0.0224   20.2820    0.0000 [ 0.4088; 0.4971 ] 
#>   eta2 <~ y21      0.3058      0.0303   10.0829    0.0000 [ 0.2517; 0.3505 ] 
#>   eta2 <~ y22      0.4473      0.0244   18.3128    0.0000 [ 0.4132; 0.5063 ] 
#>   eta2 <~ y23      0.4735      0.0224   21.0977    0.0000 [ 0.4337; 0.5126 ] 
#>   eta3 <~ y31      0.4400      0.0208   21.1750    0.0000 [ 0.4042; 0.4751 ] 
#>   eta3 <~ y32      0.3521      0.0180   19.5235    0.0000 [ 0.3172; 0.3784 ] 
#>   eta3 <~ y33      0.3999      0.0174   23.0219    0.0000 [ 0.3715; 0.4308 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0435   15.4453    0.0000 [ 0.5764; 0.7242 ] 
#>   eta3 ~ eta1       0.6634      0.0351   18.8983    0.0000 [ 0.5687; 0.6998 ] 
#>   eta3 ~ eta2       0.3052      0.0859    3.5510    0.0004 [ 0.0989; 0.4273 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0539    3.8027    0.0001 [ 0.0709; 0.2807 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04091380 16.206509  4.536142e-59
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03917671 16.573061  1.091266e-61
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03782994 20.125485  4.414317e-90
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05838044  8.846368  9.041416e-19
#> 5 eta2 =~ y22  Common factor 0.7553877 0.04374181 17.269236  8.019754e-67
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03508168 22.794339 5.218015e-115
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03126938 26.296567 2.099170e-152
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04338095 15.169536  5.627302e-52
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03678131 20.320756  8.426868e-92
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5992575          0.7517191
#> 2          0.5554017          0.6951806
#> 3          0.7047305          0.8340130
#> 4          0.3855324          0.5787340
#> 5          0.6792425          0.8385344
#> 6          0.7212354          0.8544492
#> 7          0.7810508          0.8819662
#> 8          0.5675207          0.7273963
#> 9          0.6926923          0.8115573

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
#>  Random seed                        = 977748267
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
#>   eta2 ~ eta1      0.6713      0.0435   15.4453    0.0000 [ 0.5715; 0.7962 ] 
#>   eta3 ~ eta1      0.4585      0.0739    6.2068    0.0000 [ 0.2617; 0.6437 ] 
#>   eta3 ~ eta2      0.3052      0.0859    3.5510    0.0004 [ 0.0899; 0.5343 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0409   16.2065    0.0000 [ 0.5623; 0.7739 ] 
#>   eta1 =~ y12      0.6493      0.0392   16.5731    0.0000 [ 0.5540; 0.7566 ] 
#>   eta1 =~ y13      0.7613      0.0378   20.1255    0.0000 [ 0.6564; 0.8521 ] 
#>   eta2 =~ y21      0.5165      0.0584    8.8464    0.0000 [ 0.3786; 0.6805 ] 
#>   eta2 =~ y22      0.7554      0.0437   17.2692    0.0000 [ 0.6386; 0.8648 ] 
#>   eta2 =~ y23      0.7997      0.0351   22.7943    0.0000 [ 0.7121; 0.8935 ] 
#>   eta3 =~ y31      0.8223      0.0313   26.2966    0.0000 [ 0.7387; 0.9005 ] 
#>   eta3 =~ y32      0.6581      0.0434   15.1695    0.0000 [ 0.5518; 0.7761 ] 
#>   eta3 =~ y33      0.7474      0.0368   20.3208    0.0000 [ 0.6479; 0.8381 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0226   17.4797    0.0000 [ 0.3392; 0.4563 ] 
#>   eta1 <~ y12      0.3873      0.0188   20.5839    0.0000 [ 0.3417; 0.4390 ] 
#>   eta1 <~ y13      0.4542      0.0224   20.2820    0.0000 [ 0.3910; 0.5068 ] 
#>   eta2 <~ y21      0.3058      0.0303   10.0829    0.0000 [ 0.2337; 0.3905 ] 
#>   eta2 <~ y22      0.4473      0.0244   18.3128    0.0000 [ 0.3790; 0.5053 ] 
#>   eta2 <~ y23      0.4735      0.0224   21.0977    0.0000 [ 0.4140; 0.5301 ] 
#>   eta3 <~ y31      0.4400      0.0208   21.1750    0.0000 [ 0.3853; 0.4927 ] 
#>   eta3 <~ y32      0.3521      0.0180   19.5235    0.0000 [ 0.3094; 0.4027 ] 
#>   eta3 <~ y33      0.3999      0.0174   23.0219    0.0000 [ 0.3533; 0.4431 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0435   15.4453    0.0000 [ 0.5715; 0.7962 ] 
#>   eta3 ~ eta1       0.6634      0.0351   18.8983    0.0000 [ 0.5768; 0.7584 ] 
#>   eta3 ~ eta2       0.3052      0.0859    3.5510    0.0004 [ 0.0899; 0.5343 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0539    3.8027    0.0001 [ 0.0756; 0.3542 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04346532 15.445267 8.119126e-54
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.07387210  6.206765 5.408637e-10
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.08593487  3.550958 3.838313e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.57146912          0.7962472          0.5984606          0.7692558
#> 2         0.26165763          0.6436824          0.3075313          0.5978087
#> 3         0.08991067          0.5343173          0.1432752          0.4809527
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1         0.55944110          0.7282426         0.57640325          0.7241839
#> 2         0.31865203          0.6555130         0.33898016          0.6278525
#> 3         0.05442647          0.4744017         0.09887035          0.4272675
```
