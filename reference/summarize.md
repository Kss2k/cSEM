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
#>  Random seed                        = 1192022785
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
#>   eta2 ~ eta1      0.6713      0.0407   16.4940    0.0000 [ 0.6017; 0.7643 ] 
#>   eta3 ~ eta1      0.4585      0.0765    5.9959    0.0000 [ 0.2939; 0.5934 ] 
#>   eta3 ~ eta2      0.3052      0.0822    3.7138    0.0002 [ 0.1907; 0.4576 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0368   18.0096    0.0000 [ 0.5855; 0.7241 ] 
#>   eta1 =~ y12      0.6493      0.0379   17.1490    0.0000 [ 0.5801; 0.7042 ] 
#>   eta1 =~ y13      0.7613      0.0397   19.1555    0.0000 [ 0.6595; 0.8232 ] 
#>   eta2 =~ y21      0.5165      0.0561    9.2119    0.0000 [ 0.4170; 0.5943 ] 
#>   eta2 =~ y22      0.7554      0.0328   23.0438    0.0000 [ 0.6914; 0.8071 ] 
#>   eta2 =~ y23      0.7997      0.0419   19.0839    0.0000 [ 0.7335; 0.8717 ] 
#>   eta3 =~ y31      0.8223      0.0367   22.3889    0.0000 [ 0.7658; 0.8801 ] 
#>   eta3 =~ y32      0.6581      0.0398   16.5147    0.0000 [ 0.5889; 0.7232 ] 
#>   eta3 =~ y33      0.7474      0.0368   20.3346    0.0000 [ 0.6748; 0.8130 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0208   18.9824    0.0000 [ 0.3574; 0.4347 ] 
#>   eta1 <~ y12      0.3873      0.0192   20.1419    0.0000 [ 0.3517; 0.4261 ] 
#>   eta1 <~ y13      0.4542      0.0192   23.7144    0.0000 [ 0.4190; 0.4834 ] 
#>   eta2 <~ y21      0.3058      0.0297   10.2948    0.0000 [ 0.2495; 0.3486 ] 
#>   eta2 <~ y22      0.4473      0.0218   20.5134    0.0000 [ 0.4164; 0.4894 ] 
#>   eta2 <~ y23      0.4735      0.0242   19.5601    0.0000 [ 0.4430; 0.5190 ] 
#>   eta3 <~ y31      0.4400      0.0179   24.5139    0.0000 [ 0.4071; 0.4655 ] 
#>   eta3 <~ y32      0.3521      0.0187   18.8354    0.0000 [ 0.3188; 0.3820 ] 
#>   eta3 <~ y33      0.3999      0.0201   19.9018    0.0000 [ 0.3592; 0.4323 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0407   16.4940    0.0000 [ 0.6017; 0.7643 ] 
#>   eta3 ~ eta1       0.6634      0.0346   19.1976    0.0000 [ 0.6091; 0.7371 ] 
#>   eta3 ~ eta2       0.3052      0.0822    3.7138    0.0002 [ 0.1907; 0.4576 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0552    3.7139    0.0002 [ 0.1294; 0.3159 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03681757 18.009602  1.638011e-72
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03786091 17.149031  6.390967e-66
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03974550 19.155522  8.704889e-82
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05606378  9.211915  3.203573e-20
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03278051 23.043807 1.697023e-117
#> 6 eta2 =~ y23  Common factor 0.7996637 0.04190252 19.083906  3.435955e-81
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03672698 22.388920 5.046660e-111
#> 8 eta3 =~ y32  Common factor 0.6580689 0.03984757 16.514655  2.878037e-61
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03675624 20.334621  6.352881e-92
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5854641          0.7241446
#> 2          0.5801180          0.7041725
#> 3          0.6594572          0.8231946
#> 4          0.4169552          0.5943250
#> 5          0.6913722          0.8071448
#> 6          0.7334972          0.8717003
#> 7          0.7657692          0.8801372
#> 8          0.5889413          0.7232390
#> 9          0.6747793          0.8129701

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
#>  Random seed                        = 1192022785
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
#>   eta2 ~ eta1      0.6713      0.0407   16.4940    0.0000 [ 0.5681; 0.7786 ] 
#>   eta3 ~ eta1      0.4585      0.0765    5.9959    0.0000 [ 0.2672; 0.6627 ] 
#>   eta3 ~ eta2      0.3052      0.0822    3.7138    0.0002 [ 0.0783; 0.5033 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0368   18.0096    0.0000 [ 0.5613; 0.7517 ] 
#>   eta1 =~ y12      0.6493      0.0379   17.1490    0.0000 [ 0.5513; 0.7471 ] 
#>   eta1 =~ y13      0.7613      0.0397   19.1555    0.0000 [ 0.6612; 0.8667 ] 
#>   eta2 =~ y21      0.5165      0.0561    9.2119    0.0000 [ 0.3872; 0.6771 ] 
#>   eta2 =~ y22      0.7554      0.0328   23.0438    0.0000 [ 0.6736; 0.8431 ] 
#>   eta2 =~ y23      0.7997      0.0419   19.0839    0.0000 [ 0.6892; 0.9059 ] 
#>   eta3 =~ y31      0.8223      0.0367   22.3889    0.0000 [ 0.7291; 0.9191 ] 
#>   eta3 =~ y32      0.6581      0.0398   16.5147    0.0000 [ 0.5449; 0.7509 ] 
#>   eta3 =~ y33      0.7474      0.0368   20.3346    0.0000 [ 0.6479; 0.8379 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0208   18.9824    0.0000 [ 0.3390; 0.4467 ] 
#>   eta1 <~ y12      0.3873      0.0192   20.1419    0.0000 [ 0.3388; 0.4383 ] 
#>   eta1 <~ y13      0.4542      0.0192   23.7144    0.0000 [ 0.4077; 0.5068 ] 
#>   eta2 <~ y21      0.3058      0.0297   10.2948    0.0000 [ 0.2359; 0.3895 ] 
#>   eta2 <~ y22      0.4473      0.0218   20.5134    0.0000 [ 0.3884; 0.5012 ] 
#>   eta2 <~ y23      0.4735      0.0242   19.5601    0.0000 [ 0.4053; 0.5305 ] 
#>   eta3 <~ y31      0.4400      0.0179   24.5139    0.0000 [ 0.3985; 0.4913 ] 
#>   eta3 <~ y32      0.3521      0.0187   18.8354    0.0000 [ 0.3016; 0.3983 ] 
#>   eta3 <~ y33      0.3999      0.0201   19.9018    0.0000 [ 0.3491; 0.4530 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0407   16.4940    0.0000 [ 0.5681; 0.7786 ] 
#>   eta3 ~ eta1       0.6634      0.0346   19.1976    0.0000 [ 0.5720; 0.7507 ] 
#>   eta3 ~ eta2       0.3052      0.0822    3.7138    0.0002 [ 0.0783; 0.5033 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0552    3.7139    0.0002 [ 0.0538; 0.3390 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04070174 16.493972 4.054003e-61
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.07647002  5.995902 2.023589e-09
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.08216680  3.713801 2.041697e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.56809189          0.7785783          0.5933672          0.7533030
#> 2         0.26720003          0.6626598          0.3146870          0.6151728
#> 3         0.07834075          0.5032611          0.1293654          0.4522364
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5889030          0.7725756          0.6016658          0.7643163
#> 2          0.2740104          0.5965790          0.2939416          0.5934393
#> 3          0.1628738          0.5130038          0.1907173          0.4576344
```
