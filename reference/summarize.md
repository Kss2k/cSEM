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
#>  Random seed                        = 1725324745
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
#>   eta2 ~ eta1      0.6713      0.0514   13.0653    0.0000 [ 0.5465; 0.7421 ] 
#>   eta3 ~ eta1      0.4585      0.0731    6.2715    0.0000 [ 0.3586; 0.6218 ] 
#>   eta3 ~ eta2      0.3052      0.0767    3.9805    0.0001 [ 0.1240; 0.4105 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0475   13.9472    0.0000 [ 0.5545; 0.7335 ] 
#>   eta1 =~ y12      0.6493      0.0385   16.8462    0.0000 [ 0.5874; 0.7272 ] 
#>   eta1 =~ y13      0.7613      0.0284   26.8441    0.0000 [ 0.6976; 0.8013 ] 
#>   eta2 =~ y21      0.5165      0.0573    9.0122    0.0000 [ 0.4150; 0.6115 ] 
#>   eta2 =~ y22      0.7554      0.0489   15.4330    0.0000 [ 0.6690; 0.8406 ] 
#>   eta2 =~ y23      0.7997      0.0342   23.3785    0.0000 [ 0.7272; 0.8515 ] 
#>   eta3 =~ y31      0.8223      0.0397   20.7267    0.0000 [ 0.7562; 0.8637 ] 
#>   eta3 =~ y32      0.6581      0.0361   18.2199    0.0000 [ 0.5803; 0.7247 ] 
#>   eta3 =~ y33      0.7474      0.0421   17.7393    0.0000 [ 0.6473; 0.8050 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0230   17.1747    0.0000 [ 0.3515; 0.4320 ] 
#>   eta1 <~ y12      0.3873      0.0195   19.8526    0.0000 [ 0.3638; 0.4349 ] 
#>   eta1 <~ y13      0.4542      0.0203   22.4098    0.0000 [ 0.4138; 0.4791 ] 
#>   eta2 <~ y21      0.3058      0.0287   10.6487    0.0000 [ 0.2596; 0.3541 ] 
#>   eta2 <~ y22      0.4473      0.0246   18.1619    0.0000 [ 0.4072; 0.4963 ] 
#>   eta2 <~ y23      0.4735      0.0251   18.9021    0.0000 [ 0.4371; 0.5362 ] 
#>   eta3 <~ y31      0.4400      0.0200   21.9497    0.0000 [ 0.4053; 0.4724 ] 
#>   eta3 <~ y32      0.3521      0.0202   17.4610    0.0000 [ 0.3245; 0.4021 ] 
#>   eta3 <~ y33      0.3999      0.0193   20.7471    0.0000 [ 0.3677; 0.4290 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0514   13.0653    0.0000 [ 0.5465; 0.7421 ] 
#>   eta3 ~ eta1       0.6634      0.0353   18.7855    0.0000 [ 0.5879; 0.7148 ] 
#>   eta3 ~ eta2       0.3052      0.0767    3.9805    0.0001 [ 0.1240; 0.4105 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0549    3.7282    0.0002 [ 0.0925; 0.2723 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04754143 13.947200  3.271995e-44
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03854153 16.846190  1.118949e-63
#> 3 eta1 =~ y13  Common factor 0.7613458 0.02836174 26.844112 9.881183e-159
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05730611  9.012212  2.019411e-19
#> 5 eta2 =~ y22  Common factor 0.7553877 0.04894633 15.432980  9.822845e-54
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03420504 23.378535 7.066908e-121
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03967242 20.726676  1.990753e-95
#> 8 eta3 =~ y32  Common factor 0.6580689 0.03611812 18.219911  3.587379e-74
#> 9 eta3 =~ y33  Common factor 0.7474241 0.04213389 17.739264  2.086517e-70
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5544972          0.7334566
#> 2          0.5874475          0.7271640
#> 3          0.6975708          0.8013085
#> 4          0.4149921          0.6115262
#> 5          0.6690129          0.8406288
#> 6          0.7271571          0.8514678
#> 7          0.7561575          0.8637060
#> 8          0.5803228          0.7246868
#> 9          0.6472586          0.8049929

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
#>  Random seed                        = 1725324745
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
#>   eta2 ~ eta1      0.6713      0.0514   13.0653    0.0000 [ 0.5399; 0.8056 ] 
#>   eta3 ~ eta1      0.4585      0.0731    6.2715    0.0000 [ 0.2470; 0.6251 ] 
#>   eta3 ~ eta2      0.3052      0.0767    3.9805    0.0001 [ 0.1332; 0.5296 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0475   13.9472    0.0000 [ 0.5426; 0.7884 ] 
#>   eta1 =~ y12      0.6493      0.0385   16.8462    0.0000 [ 0.5478; 0.7471 ] 
#>   eta1 =~ y13      0.7613      0.0284   26.8441    0.0000 [ 0.6976; 0.8443 ] 
#>   eta2 =~ y21      0.5165      0.0573    9.0122    0.0000 [ 0.3637; 0.6600 ] 
#>   eta2 =~ y22      0.7554      0.0489   15.4330    0.0000 [ 0.6325; 0.8856 ] 
#>   eta2 =~ y23      0.7997      0.0342   23.3785    0.0000 [ 0.7199; 0.8968 ] 
#>   eta3 =~ y31      0.8223      0.0397   20.7267    0.0000 [ 0.7319; 0.9370 ] 
#>   eta3 =~ y32      0.6581      0.0361   18.2199    0.0000 [ 0.5584; 0.7452 ] 
#>   eta3 =~ y33      0.7474      0.0421   17.7393    0.0000 [ 0.6453; 0.8632 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0230   17.1747    0.0000 [ 0.3349; 0.4540 ] 
#>   eta1 <~ y12      0.3873      0.0195   19.8526    0.0000 [ 0.3331; 0.4340 ] 
#>   eta1 <~ y13      0.4542      0.0203   22.4098    0.0000 [ 0.4041; 0.5089 ] 
#>   eta2 <~ y21      0.3058      0.0287   10.6487    0.0000 [ 0.2276; 0.3761 ] 
#>   eta2 <~ y22      0.4473      0.0246   18.1619    0.0000 [ 0.3835; 0.5109 ] 
#>   eta2 <~ y23      0.4735      0.0251   18.9021    0.0000 [ 0.4110; 0.5405 ] 
#>   eta3 <~ y31      0.4400      0.0200   21.9497    0.0000 [ 0.3909; 0.4945 ] 
#>   eta3 <~ y32      0.3521      0.0202   17.4610    0.0000 [ 0.2934; 0.3977 ] 
#>   eta3 <~ y33      0.3999      0.0193   20.7471    0.0000 [ 0.3504; 0.4501 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0514   13.0653    0.0000 [ 0.5399; 0.8056 ] 
#>   eta3 ~ eta1       0.6634      0.0353   18.7855    0.0000 [ 0.5673; 0.7499 ] 
#>   eta3 ~ eta2       0.3052      0.0767    3.9805    0.0001 [ 0.1332; 0.5296 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0549    3.7282    0.0002 [ 0.0804; 0.3646 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.05138304 13.065272 5.199878e-39
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.07311010  6.271456 3.576883e-10
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.07666118  3.980517 6.876555e-05
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1          0.5398623          0.8055864          0.5717705          0.7736781
#> 2          0.2470481          0.6251323          0.2924486          0.5797318
#> 3          0.1331627          0.5296111          0.1807684          0.4820054
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5434455          0.7473117          0.5464553          0.7420942
#> 2          0.3117670          0.6254747          0.3585755          0.6218349
#> 3          0.1004540          0.4480320          0.1240363          0.4105264
```
