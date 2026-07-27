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
#>  Random seed                        = -1493095939
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
#>   eta2 ~ eta1      0.6713      0.0450   14.9210    0.0000 [ 0.5856; 0.7237 ] 
#>   eta3 ~ eta1      0.4585      0.0783    5.8591    0.0000 [ 0.3113; 0.6124 ] 
#>   eta3 ~ eta2      0.3052      0.0799    3.8173    0.0001 [ 0.1756; 0.4857 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0378   17.5358    0.0000 [ 0.5889; 0.7187 ] 
#>   eta1 =~ y12      0.6493      0.0350   18.5460    0.0000 [ 0.5779; 0.7036 ] 
#>   eta1 =~ y13      0.7613      0.0351   21.6931    0.0000 [ 0.6954; 0.8323 ] 
#>   eta2 =~ y21      0.5165      0.0607    8.5015    0.0000 [ 0.3794; 0.5983 ] 
#>   eta2 =~ y22      0.7554      0.0376   20.1149    0.0000 [ 0.6736; 0.8275 ] 
#>   eta2 =~ y23      0.7997      0.0311   25.7291    0.0000 [ 0.7310; 0.8505 ] 
#>   eta3 =~ y31      0.8223      0.0367   22.4081    0.0000 [ 0.7323; 0.8670 ] 
#>   eta3 =~ y32      0.6581      0.0366   17.9940    0.0000 [ 0.5933; 0.7288 ] 
#>   eta3 =~ y33      0.7474      0.0287   26.0701    0.0000 [ 0.6925; 0.8022 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0215   18.3882    0.0000 [ 0.3525; 0.4312 ] 
#>   eta1 <~ y12      0.3873      0.0169   22.9441    0.0000 [ 0.3559; 0.4132 ] 
#>   eta1 <~ y13      0.4542      0.0191   23.7438    0.0000 [ 0.4232; 0.4873 ] 
#>   eta2 <~ y21      0.3058      0.0297   10.3096    0.0000 [ 0.2381; 0.3464 ] 
#>   eta2 <~ y22      0.4473      0.0258   17.3123    0.0000 [ 0.4177; 0.4908 ] 
#>   eta2 <~ y23      0.4735      0.0194   24.4351    0.0000 [ 0.4417; 0.5058 ] 
#>   eta3 <~ y31      0.4400      0.0172   25.6071    0.0000 [ 0.4044; 0.4705 ] 
#>   eta3 <~ y32      0.3521      0.0177   19.8889    0.0000 [ 0.3271; 0.3837 ] 
#>   eta3 <~ y33      0.3999      0.0173   23.1820    0.0000 [ 0.3761; 0.4369 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0450   14.9210    0.0000 [ 0.5856; 0.7237 ] 
#>   eta3 ~ eta1       0.6634      0.0449   14.7785    0.0000 [ 0.5904; 0.7387 ] 
#>   eta3 ~ eta2       0.3052      0.0799    3.8173    0.0001 [ 0.1756; 0.4857 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0543    3.7734    0.0002 [ 0.1173; 0.2935 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03781226 17.535843  7.630977e-69
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03500901 18.546026  8.780898e-77
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03509630 21.693052 2.386170e-104
#> 4 eta2 =~ y21  Common factor 0.5164548 0.06074887  8.501472  1.872013e-17
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03755354 20.114950  5.459442e-90
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03108010 25.729126 5.520852e-146
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03669549 22.408132 3.279099e-111
#> 8 eta3 =~ y32  Common factor 0.6580689 0.03657160 17.993986  2.171609e-72
#> 9 eta3 =~ y33  Common factor 0.7474241 0.02866980 26.070085 7.965056e-150
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5888640          0.7186823
#> 2          0.5779389          0.7035680
#> 3          0.6954286          0.8322671
#> 4          0.3793636          0.5982729
#> 5          0.6736198          0.8274694
#> 6          0.7309807          0.8504853
#> 7          0.7323286          0.8670262
#> 8          0.5932526          0.7287925
#> 9          0.6925430          0.8021518

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
#>  Random seed                        = -1493095939
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
#>   eta2 ~ eta1      0.6713      0.0450   14.9210    0.0000 [ 0.5680; 0.8006 ] 
#>   eta3 ~ eta1      0.4585      0.0783    5.8591    0.0000 [ 0.2653; 0.6700 ] 
#>   eta3 ~ eta2      0.3052      0.0799    3.8173    0.0001 [ 0.0830; 0.4964 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0378   17.5358    0.0000 [ 0.5624; 0.7580 ] 
#>   eta1 =~ y12      0.6493      0.0350   18.5460    0.0000 [ 0.5551; 0.7361 ] 
#>   eta1 =~ y13      0.7613      0.0351   21.6931    0.0000 [ 0.6715; 0.8530 ] 
#>   eta2 =~ y21      0.5165      0.0607    8.5015    0.0000 [ 0.3659; 0.6801 ] 
#>   eta2 =~ y22      0.7554      0.0376   20.1149    0.0000 [ 0.6600; 0.8542 ] 
#>   eta2 =~ y23      0.7997      0.0311   25.7291    0.0000 [ 0.7180; 0.8788 ] 
#>   eta3 =~ y31      0.8223      0.0367   22.4081    0.0000 [ 0.7360; 0.9258 ] 
#>   eta3 =~ y32      0.6581      0.0366   17.9940    0.0000 [ 0.5606; 0.7498 ] 
#>   eta3 =~ y33      0.7474      0.0287   26.0701    0.0000 [ 0.6792; 0.8275 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0215   18.3882    0.0000 [ 0.3399; 0.4512 ] 
#>   eta1 <~ y12      0.3873      0.0169   22.9441    0.0000 [ 0.3433; 0.4306 ] 
#>   eta1 <~ y13      0.4542      0.0191   23.7438    0.0000 [ 0.4072; 0.5061 ] 
#>   eta2 <~ y21      0.3058      0.0297   10.3096    0.0000 [ 0.2327; 0.3861 ] 
#>   eta2 <~ y22      0.4473      0.0258   17.3123    0.0000 [ 0.3798; 0.5134 ] 
#>   eta2 <~ y23      0.4735      0.0194   24.4351    0.0000 [ 0.4210; 0.5212 ] 
#>   eta3 <~ y31      0.4400      0.0172   25.6071    0.0000 [ 0.3968; 0.4856 ] 
#>   eta3 <~ y32      0.3521      0.0177   19.8889    0.0000 [ 0.3021; 0.3936 ] 
#>   eta3 <~ y33      0.3999      0.0173   23.1820    0.0000 [ 0.3552; 0.4444 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0450   14.9210    0.0000 [ 0.5680; 0.8006 ] 
#>   eta3 ~ eta1       0.6634      0.0449   14.7785    0.0000 [ 0.5502; 0.7823 ] 
#>   eta3 ~ eta2       0.3052      0.0799    3.8173    0.0001 [ 0.0830; 0.4964 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0543    3.7734    0.0002 [ 0.0582; 0.3390 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04499262 14.920969 2.407514e-50
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.07825495  5.859141 4.652674e-09
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.07993796  3.817350 1.348930e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.56796111          0.8006375          0.5959010          0.7726976
#> 2         0.26532331          0.6700137          0.3139187          0.6214183
#> 3         0.08299809          0.4963921          0.1326386          0.4467515
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5721329          0.7591549          0.5855944          0.7236879
#> 2          0.2736644          0.6174699          0.3112584          0.6124268
#> 3          0.1370578          0.5054564          0.1755923          0.4856887
```
