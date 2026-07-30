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
#>  Random seed                        = -723511124
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
#>   eta2 ~ eta1      0.6713      0.0462   14.5283    0.0000 [ 0.5915; 0.7546 ] 
#>   eta3 ~ eta1      0.4585      0.0997    4.6000    0.0000 [ 0.2800; 0.6670 ] 
#>   eta3 ~ eta2      0.3052      0.1057    2.8878    0.0039 [ 0.1045; 0.4902 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0442   14.9940    0.0000 [ 0.5738; 0.7488 ] 
#>   eta1 =~ y12      0.6493      0.0478   13.5755    0.0000 [ 0.5702; 0.7292 ] 
#>   eta1 =~ y13      0.7613      0.0304   25.0534    0.0000 [ 0.7038; 0.8106 ] 
#>   eta2 =~ y21      0.5165      0.0487   10.6021    0.0000 [ 0.4265; 0.5815 ] 
#>   eta2 =~ y22      0.7554      0.0360   20.9945    0.0000 [ 0.6879; 0.8042 ] 
#>   eta2 =~ y23      0.7997      0.0320   25.0079    0.0000 [ 0.7489; 0.8697 ] 
#>   eta3 =~ y31      0.8223      0.0348   23.6284    0.0000 [ 0.7620; 0.8787 ] 
#>   eta3 =~ y32      0.6581      0.0437   15.0424    0.0000 [ 0.5719; 0.7323 ] 
#>   eta3 =~ y33      0.7474      0.0372   20.0681    0.0000 [ 0.6640; 0.8037 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0234   16.9077    0.0000 [ 0.3546; 0.4412 ] 
#>   eta1 <~ y12      0.3873      0.0214   18.0794    0.0000 [ 0.3624; 0.4238 ] 
#>   eta1 <~ y13      0.4542      0.0226   20.1187    0.0000 [ 0.4197; 0.5100 ] 
#>   eta2 <~ y21      0.3058      0.0252   12.1533    0.0000 [ 0.2530; 0.3399 ] 
#>   eta2 <~ y22      0.4473      0.0195   22.9661    0.0000 [ 0.3946; 0.4748 ] 
#>   eta2 <~ y23      0.4735      0.0202   23.4447    0.0000 [ 0.4433; 0.5194 ] 
#>   eta3 <~ y31      0.4400      0.0159   27.6443    0.0000 [ 0.4110; 0.4665 ] 
#>   eta3 <~ y32      0.3521      0.0206   17.0796    0.0000 [ 0.3161; 0.3895 ] 
#>   eta3 <~ y33      0.3999      0.0201   19.8515    0.0000 [ 0.3604; 0.4384 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0462   14.5283    0.0000 [ 0.5915; 0.7546 ] 
#>   eta3 ~ eta1       0.6634      0.0410   16.1833    0.0000 [ 0.5801; 0.7282 ] 
#>   eta3 ~ eta2       0.3052      0.1057    2.8878    0.0039 [ 0.1045; 0.4902 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0705    2.9049    0.0037 [ 0.0694; 0.3315 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err   t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04422222 14.99404  8.031290e-51
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04782703 13.57554  5.592719e-42
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03038896 25.05337 1.604458e-138
#> 4 eta2 =~ y21  Common factor 0.5164548 0.04871273 10.60205  2.915162e-26
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03598020 20.99454  7.357536e-98
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03197646 25.00789 5.017574e-138
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03480040 23.62839 1.969253e-123
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04374770 15.04237  3.874491e-51
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03724440 20.06809  1.402822e-89
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5738359          0.7487927
#> 2          0.5702087          0.7292004
#> 3          0.7038246          0.8105830
#> 4          0.4265068          0.5814951
#> 5          0.6878706          0.8042333
#> 6          0.7488777          0.8697416
#> 7          0.7619610          0.8787032
#> 8          0.5718738          0.7322729
#> 9          0.6640028          0.8037489

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
#>  Random seed                        = -723511124
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
#>   eta2 ~ eta1      0.6713      0.0462   14.5283    0.0000 [ 0.5504; 0.7893 ] 
#>   eta3 ~ eta1      0.4585      0.0997    4.6000    0.0000 [ 0.1934; 0.7089 ] 
#>   eta3 ~ eta2      0.3052      0.1057    2.8878    0.0039 [ 0.0387; 0.5852 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0442   14.9940    0.0000 [ 0.5522; 0.7809 ] 
#>   eta1 =~ y12      0.6493      0.0478   13.5755    0.0000 [ 0.5256; 0.7730 ] 
#>   eta1 =~ y13      0.7613      0.0304   25.0534    0.0000 [ 0.6840; 0.8411 ] 
#>   eta2 =~ y21      0.5165      0.0487   10.6021    0.0000 [ 0.3911; 0.6430 ] 
#>   eta2 =~ y22      0.7554      0.0360   20.9945    0.0000 [ 0.6659; 0.8519 ] 
#>   eta2 =~ y23      0.7997      0.0320   25.0079    0.0000 [ 0.7042; 0.8695 ] 
#>   eta3 =~ y31      0.8223      0.0348   23.6284    0.0000 [ 0.7317; 0.9117 ] 
#>   eta3 =~ y32      0.6581      0.0437   15.0424    0.0000 [ 0.5384; 0.7646 ] 
#>   eta3 =~ y33      0.7474      0.0372   20.0681    0.0000 [ 0.6620; 0.8546 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0234   16.9077    0.0000 [ 0.3361; 0.4571 ] 
#>   eta1 <~ y12      0.3873      0.0214   18.0794    0.0000 [ 0.3312; 0.4420 ] 
#>   eta1 <~ y13      0.4542      0.0226   20.1187    0.0000 [ 0.3949; 0.5117 ] 
#>   eta2 <~ y21      0.3058      0.0252   12.1533    0.0000 [ 0.2436; 0.3737 ] 
#>   eta2 <~ y22      0.4473      0.0195   22.9661    0.0000 [ 0.4023; 0.5030 ] 
#>   eta2 <~ y23      0.4735      0.0202   23.4447    0.0000 [ 0.4171; 0.5216 ] 
#>   eta3 <~ y31      0.4400      0.0159   27.6443    0.0000 [ 0.3975; 0.4798 ] 
#>   eta3 <~ y32      0.3521      0.0206   17.0796    0.0000 [ 0.2946; 0.4012 ] 
#>   eta3 <~ y33      0.3999      0.0201   19.8515    0.0000 [ 0.3527; 0.4569 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0462   14.5283    0.0000 [ 0.5504; 0.7893 ] 
#>   eta3 ~ eta1       0.6634      0.0410   16.1833    0.0000 [ 0.5553; 0.7673 ] 
#>   eta3 ~ eta2       0.3052      0.1057    2.8878    0.0039 [ 0.0387; 0.5852 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0705    2.9049    0.0037 [ 0.0278; 0.3925 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04620867 14.528301 8.018426e-48
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.09967470  4.600032 4.224269e-06
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.10567077  2.887753 3.880040e-03
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.55037913          0.7893443          0.5790742          0.7606492
#> 2         0.19344353          0.7089048          0.2553404          0.6470080
#> 3         0.03869848          0.5851680          0.1043188          0.5195477
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1         0.56585844          0.7563476          0.5915454          0.7546136
#> 2         0.25054506          0.6878852          0.2799890          0.6670444
#> 3         0.05745389          0.4937650          0.1044794          0.4902500
```
