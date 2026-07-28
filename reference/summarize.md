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
#>  Random seed                        = 435021136
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
#>   eta2 ~ eta1      0.6713      0.0441   15.2382    0.0000 [ 0.5967; 0.7381 ] 
#>   eta3 ~ eta1      0.4585      0.0844    5.4355    0.0000 [ 0.2887; 0.5878 ] 
#>   eta3 ~ eta2      0.3052      0.0915    3.3337    0.0009 [ 0.1655; 0.4977 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0457   14.5237    0.0000 [ 0.5818; 0.7330 ] 
#>   eta1 =~ y12      0.6493      0.0437   14.8736    0.0000 [ 0.5615; 0.7165 ] 
#>   eta1 =~ y13      0.7613      0.0324   23.4952    0.0000 [ 0.7179; 0.8506 ] 
#>   eta2 =~ y21      0.5165      0.0643    8.0300    0.0000 [ 0.4306; 0.6388 ] 
#>   eta2 =~ y22      0.7554      0.0370   20.4259    0.0000 [ 0.6903; 0.8338 ] 
#>   eta2 =~ y23      0.7997      0.0378   21.1830    0.0000 [ 0.7397; 0.8616 ] 
#>   eta3 =~ y31      0.8223      0.0329   24.9937    0.0000 [ 0.7635; 0.8828 ] 
#>   eta3 =~ y32      0.6581      0.0404   16.3075    0.0000 [ 0.5844; 0.7216 ] 
#>   eta3 =~ y33      0.7474      0.0477   15.6607    0.0000 [ 0.6888; 0.8476 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0248   15.9482    0.0000 [ 0.3557; 0.4403 ] 
#>   eta1 <~ y12      0.3873      0.0247   15.6732    0.0000 [ 0.3422; 0.4229 ] 
#>   eta1 <~ y13      0.4542      0.0169   26.9005    0.0000 [ 0.4272; 0.4948 ] 
#>   eta2 <~ y21      0.3058      0.0329    9.2989    0.0000 [ 0.2517; 0.3666 ] 
#>   eta2 <~ y22      0.4473      0.0248   18.0048    0.0000 [ 0.4056; 0.4953 ] 
#>   eta2 <~ y23      0.4735      0.0202   23.3962    0.0000 [ 0.4326; 0.5027 ] 
#>   eta3 <~ y31      0.4400      0.0201   21.9021    0.0000 [ 0.4086; 0.4794 ] 
#>   eta3 <~ y32      0.3521      0.0194   18.1732    0.0000 [ 0.3150; 0.3869 ] 
#>   eta3 <~ y33      0.3999      0.0225   17.7460    0.0000 [ 0.3733; 0.4468 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0441   15.2382    0.0000 [ 0.5967; 0.7381 ] 
#>   eta3 ~ eta1       0.6634      0.0418   15.8513    0.0000 [ 0.5944; 0.7225 ] 
#>   eta3 ~ eta2       0.3052      0.0915    3.3337    0.0009 [ 0.1655; 0.4977 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0602    3.4025    0.0007 [ 0.1091; 0.3437 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err   t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04565445 14.52366  8.580080e-48
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04365312 14.87357  4.893036e-50
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03240433 23.49519 4.568001e-122
#> 4 eta2 =~ y21  Common factor 0.5164548 0.06431582  8.02998  9.748818e-16
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03698179 20.42594  9.835715e-93
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03775031 21.18297  1.371064e-99
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03289934 24.99373 7.152383e-138
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04035385 16.30746  8.734780e-60
#> 9 eta3 =~ y33  Common factor 0.7474241 0.04772598 15.66074  2.806159e-55
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5817678          0.7329901
#> 2          0.5614870          0.7165441
#> 3          0.7178987          0.8506325
#> 4          0.4305846          0.6387581
#> 5          0.6903496          0.8338017
#> 6          0.7397135          0.8615691
#> 7          0.7634556          0.8828386
#> 8          0.5843811          0.7215848
#> 9          0.6888301          0.8476163

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
#>  Random seed                        = 435021136
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
#>   eta2 ~ eta1      0.6713      0.0441   15.2382    0.0000 [ 0.5555; 0.7833 ] 
#>   eta3 ~ eta1      0.4585      0.0844    5.4355    0.0000 [ 0.2451; 0.6814 ] 
#>   eta3 ~ eta2      0.3052      0.0915    3.3337    0.0009 [ 0.0681; 0.5414 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0457   14.5237    0.0000 [ 0.5436; 0.7797 ] 
#>   eta1 =~ y12      0.6493      0.0437   14.8736    0.0000 [ 0.5391; 0.7648 ] 
#>   eta1 =~ y13      0.7613      0.0324   23.4952    0.0000 [ 0.6654; 0.8330 ] 
#>   eta2 =~ y21      0.5165      0.0643    8.0300    0.0000 [ 0.3388; 0.6714 ] 
#>   eta2 =~ y22      0.7554      0.0370   20.4259    0.0000 [ 0.6604; 0.8516 ] 
#>   eta2 =~ y23      0.7997      0.0378   21.1830    0.0000 [ 0.7047; 0.8999 ] 
#>   eta3 =~ y31      0.8223      0.0329   24.9937    0.0000 [ 0.7338; 0.9040 ] 
#>   eta3 =~ y32      0.6581      0.0404   16.3075    0.0000 [ 0.5716; 0.7803 ] 
#>   eta3 =~ y33      0.7474      0.0477   15.6607    0.0000 [ 0.6217; 0.8685 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0248   15.9482    0.0000 [ 0.3341; 0.4624 ] 
#>   eta1 <~ y12      0.3873      0.0247   15.6732    0.0000 [ 0.3284; 0.4562 ] 
#>   eta1 <~ y13      0.4542      0.0169   26.9005    0.0000 [ 0.4073; 0.4946 ] 
#>   eta2 <~ y21      0.3058      0.0329    9.2989    0.0000 [ 0.2164; 0.3865 ] 
#>   eta2 <~ y22      0.4473      0.0248   18.0048    0.0000 [ 0.3858; 0.5143 ] 
#>   eta2 <~ y23      0.4735      0.0202   23.3962    0.0000 [ 0.4255; 0.5302 ] 
#>   eta3 <~ y31      0.4400      0.0201   21.9021    0.0000 [ 0.3829; 0.4868 ] 
#>   eta3 <~ y32      0.3521      0.0194   18.1732    0.0000 [ 0.3092; 0.4094 ] 
#>   eta3 <~ y33      0.3999      0.0225   17.7460    0.0000 [ 0.3377; 0.4542 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0441   15.2382    0.0000 [ 0.5555; 0.7833 ] 
#>   eta3 ~ eta1       0.6634      0.0418   15.8513    0.0000 [ 0.5600; 0.7764 ] 
#>   eta3 ~ eta2       0.3052      0.0915    3.3337    0.0009 [ 0.0681; 0.5414 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0602    3.4025    0.0007 [ 0.0493; 0.3606 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04405606 15.238163 1.973423e-52
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.08435447  5.435477 5.465002e-08
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.09153579  3.333681 8.570494e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.55549353          0.7833266          0.5828518          0.7559683
#> 2         0.24513791          0.6813716          0.2975210          0.6289884
#> 3         0.06805378          0.5414252          0.1248964          0.4845826
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1         0.58286326          0.7619666          0.5966997          0.7381044
#> 2         0.26280687          0.6505850          0.2887196          0.5877655
#> 3         0.09850643          0.5081440          0.1654950          0.4976769
```
