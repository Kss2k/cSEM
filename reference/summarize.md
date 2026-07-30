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
#>  Random seed                        = 468837798
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
#>   eta2 ~ eta1      0.6713      0.0411   16.3490    0.0000 [ 0.6097; 0.7565 ] 
#>   eta3 ~ eta1      0.4585      0.0795    5.7696    0.0000 [ 0.3060; 0.6135 ] 
#>   eta3 ~ eta2      0.3052      0.0893    3.4184    0.0006 [ 0.1408; 0.4443 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0392   16.9047    0.0000 [ 0.5921; 0.7174 ] 
#>   eta1 =~ y12      0.6493      0.0371   17.4852    0.0000 [ 0.5765; 0.6979 ] 
#>   eta1 =~ y13      0.7613      0.0286   26.6172    0.0000 [ 0.7077; 0.8113 ] 
#>   eta2 =~ y21      0.5165      0.0571    9.0507    0.0000 [ 0.4264; 0.6045 ] 
#>   eta2 =~ y22      0.7554      0.0396   19.0949    0.0000 [ 0.6894; 0.8240 ] 
#>   eta2 =~ y23      0.7997      0.0438   18.2392    0.0000 [ 0.7157; 0.8761 ] 
#>   eta3 =~ y31      0.8223      0.0326   25.2219    0.0000 [ 0.7706; 0.8796 ] 
#>   eta3 =~ y32      0.6581      0.0386   17.0320    0.0000 [ 0.5795; 0.7192 ] 
#>   eta3 =~ y33      0.7474      0.0339   22.0191    0.0000 [ 0.6838; 0.7983 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0191   20.7035    0.0000 [ 0.3635; 0.4287 ] 
#>   eta1 <~ y12      0.3873      0.0186   20.8576    0.0000 [ 0.3478; 0.4176 ] 
#>   eta1 <~ y13      0.4542      0.0214   21.2644    0.0000 [ 0.4218; 0.5008 ] 
#>   eta2 <~ y21      0.3058      0.0316    9.6778    0.0000 [ 0.2606; 0.3592 ] 
#>   eta2 <~ y22      0.4473      0.0233   19.2029    0.0000 [ 0.4127; 0.4902 ] 
#>   eta2 <~ y23      0.4735      0.0246   19.2867    0.0000 [ 0.4312; 0.5192 ] 
#>   eta3 <~ y31      0.4400      0.0174   25.2872    0.0000 [ 0.4160; 0.4737 ] 
#>   eta3 <~ y32      0.3521      0.0204   17.2361    0.0000 [ 0.3097; 0.3831 ] 
#>   eta3 <~ y33      0.3999      0.0146   27.4778    0.0000 [ 0.3832; 0.4356 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0411   16.3490    0.0000 [ 0.6097; 0.7565 ] 
#>   eta3 ~ eta1       0.6634      0.0285   23.2430    0.0000 [ 0.6157; 0.7287 ] 
#>   eta3 ~ eta2       0.3052      0.0893    3.4184    0.0006 [ 0.1408; 0.4443 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0639    3.2055    0.0013 [ 0.0921; 0.3194 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03922406 16.904673  4.156263e-64
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03713297 17.485214  1.857162e-68
#> 3 eta1 =~ y13  Common factor 0.7613458 0.02860357 26.617156 4.297404e-156
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05706232  9.050715  1.420346e-19
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03955969 19.094883  2.784785e-81
#> 6 eta2 =~ y23  Common factor 0.7996637 0.04384313 18.239203  2.521059e-74
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03260166 25.221946 2.301472e-140
#> 8 eta3 =~ y32  Common factor 0.6580689 0.03863713 17.032035  4.752210e-65
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03394439 22.019076 1.890782e-107
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5920835          0.7173708
#> 2          0.5765354          0.6979072
#> 3          0.7076543          0.8112625
#> 4          0.4264071          0.6044616
#> 5          0.6894241          0.8240139
#> 6          0.7156792          0.8761256
#> 7          0.7705635          0.8795793
#> 8          0.5795058          0.7192413
#> 9          0.6838403          0.7983302

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
#>  Random seed                        = 468837798
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
#>   eta2 ~ eta1      0.6713      0.0411   16.3490    0.0000 [ 0.5540; 0.7664 ] 
#>   eta3 ~ eta1      0.4585      0.0795    5.7696    0.0000 [ 0.2504; 0.6614 ] 
#>   eta3 ~ eta2      0.3052      0.0893    3.4184    0.0006 [ 0.0786; 0.5402 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0392   16.9047    0.0000 [ 0.5649; 0.7677 ] 
#>   eta1 =~ y12      0.6493      0.0371   17.4852    0.0000 [ 0.5658; 0.7578 ] 
#>   eta1 =~ y13      0.7613      0.0286   26.6172    0.0000 [ 0.6880; 0.8359 ] 
#>   eta2 =~ y21      0.5165      0.0571    9.0507    0.0000 [ 0.3765; 0.6716 ] 
#>   eta2 =~ y22      0.7554      0.0396   19.0949    0.0000 [ 0.6522; 0.8568 ] 
#>   eta2 =~ y23      0.7997      0.0438   18.2392    0.0000 [ 0.6878; 0.9145 ] 
#>   eta3 =~ y31      0.8223      0.0326   25.2219    0.0000 [ 0.7403; 0.9089 ] 
#>   eta3 =~ y32      0.6581      0.0386   17.0320    0.0000 [ 0.5723; 0.7721 ] 
#>   eta3 =~ y33      0.7474      0.0339   22.0191    0.0000 [ 0.6607; 0.8363 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0191   20.7035    0.0000 [ 0.3439; 0.4427 ] 
#>   eta1 <~ y12      0.3873      0.0186   20.8576    0.0000 [ 0.3427; 0.4387 ] 
#>   eta1 <~ y13      0.4542      0.0214   21.2644    0.0000 [ 0.3940; 0.5045 ] 
#>   eta2 <~ y21      0.3058      0.0316    9.6778    0.0000 [ 0.2277; 0.3911 ] 
#>   eta2 <~ y22      0.4473      0.0233   19.2029    0.0000 [ 0.3847; 0.5052 ] 
#>   eta2 <~ y23      0.4735      0.0246   19.2867    0.0000 [ 0.4091; 0.5360 ] 
#>   eta3 <~ y31      0.4400      0.0174   25.2872    0.0000 [ 0.3912; 0.4812 ] 
#>   eta3 <~ y32      0.3521      0.0204   17.2361    0.0000 [ 0.3029; 0.4086 ] 
#>   eta3 <~ y33      0.3999      0.0146   27.4778    0.0000 [ 0.3585; 0.4338 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0411   16.3490    0.0000 [ 0.5540; 0.7664 ] 
#>   eta3 ~ eta1       0.6634      0.0285   23.2430    0.0000 [ 0.5860; 0.7336 ] 
#>   eta3 ~ eta2       0.3052      0.0893    3.4184    0.0006 [ 0.0786; 0.5402 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0639    3.2055    0.0013 [ 0.0387; 0.3692 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04106272 16.348975 4.423562e-60
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.07946928  5.769610 7.945499e-09
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.08926812  3.418366 6.299830e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.55403015          0.7663834          0.5795296          0.7408839
#> 2         0.25038408          0.6613543          0.2997336          0.6120048
#> 3         0.07859579          0.5402401          0.1340302          0.4848057
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5939666          0.7799661          0.6097282          0.7564981
#> 2          0.2960984          0.6418050          0.3059919          0.6135378
#> 3          0.1252949          0.4760655          0.1408353          0.4443007
```
