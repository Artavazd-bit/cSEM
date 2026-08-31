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
#>  Random seed                        = 1242803978
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
#>   eta2 ~ eta1      0.6713      0.0406   16.5397    0.0000 [ 0.5796; 0.7360 ] 
#>   eta3 ~ eta1      0.4585      0.0930    4.9289    0.0000 [ 0.2922; 0.6174 ] 
#>   eta3 ~ eta2      0.3052      0.1019    2.9951    0.0027 [ 0.1470; 0.4880 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0461   14.3797    0.0000 [ 0.5828; 0.7458 ] 
#>   eta1 =~ y12      0.6493      0.0417   15.5643    0.0000 [ 0.5722; 0.7251 ] 
#>   eta1 =~ y13      0.7613      0.0336   22.6701    0.0000 [ 0.6962; 0.8037 ] 
#>   eta2 =~ y21      0.5165      0.0572    9.0297    0.0000 [ 0.3638; 0.5915 ] 
#>   eta2 =~ y22      0.7554      0.0450   16.7833    0.0000 [ 0.6794; 0.8300 ] 
#>   eta2 =~ y23      0.7997      0.0412   19.4019    0.0000 [ 0.7281; 0.8642 ] 
#>   eta3 =~ y31      0.8223      0.0285   28.8550    0.0000 [ 0.7670; 0.8691 ] 
#>   eta3 =~ y32      0.6581      0.0412   15.9826    0.0000 [ 0.5798; 0.7327 ] 
#>   eta3 =~ y33      0.7474      0.0377   19.8053    0.0000 [ 0.6773; 0.8183 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0252   15.7002    0.0000 [ 0.3474; 0.4375 ] 
#>   eta1 <~ y12      0.3873      0.0194   19.9618    0.0000 [ 0.3584; 0.4220 ] 
#>   eta1 <~ y13      0.4542      0.0228   19.9231    0.0000 [ 0.4065; 0.4884 ] 
#>   eta2 <~ y21      0.3058      0.0342    8.9458    0.0000 [ 0.2256; 0.3605 ] 
#>   eta2 <~ y22      0.4473      0.0237   18.8398    0.0000 [ 0.4085; 0.4782 ] 
#>   eta2 <~ y23      0.4735      0.0211   22.4280    0.0000 [ 0.4451; 0.5106 ] 
#>   eta3 <~ y31      0.4400      0.0146   30.0796    0.0000 [ 0.4148; 0.4705 ] 
#>   eta3 <~ y32      0.3521      0.0207   16.9940    0.0000 [ 0.3153; 0.3890 ] 
#>   eta3 <~ y33      0.3999      0.0180   22.2437    0.0000 [ 0.3645; 0.4280 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0406   16.5397    0.0000 [ 0.5796; 0.7360 ] 
#>   eta3 ~ eta1       0.6634      0.0381   17.3913    0.0000 [ 0.6152; 0.7408 ] 
#>   eta3 ~ eta2       0.3052      0.1019    2.9951    0.0027 [ 0.1470; 0.4880 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0675    3.0332    0.0024 [ 0.1037; 0.3322 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04611167 14.379653  6.944205e-47
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04171582 15.564310  1.272349e-54
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03358368 22.670116 8.836197e-114
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05719505  9.029712  1.721243e-19
#> 5 eta2 =~ y22  Common factor 0.7553877 0.04500824 16.783318  3.232569e-63
#> 6 eta2 =~ y23  Common factor 0.7996637 0.04121576 19.401890  7.438567e-84
#> 7 eta3 =~ y31  Common factor 0.8222773 0.02849685 28.855021 4.382737e-183
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04117419 15.982560  1.690592e-57
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03773867 19.805257  2.681928e-87
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5827887          0.7457723
#> 2          0.5721679          0.7251215
#> 3          0.6961622          0.8037032
#> 4          0.3638389          0.5914587
#> 5          0.6793723          0.8300413
#> 6          0.7281069          0.8642415
#> 7          0.7670487          0.8690951
#> 8          0.5798096          0.7327178
#> 9          0.6772615          0.8182815

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
#>  Random seed                        = 1242803978
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
#>   eta2 ~ eta1      0.6713      0.0406   16.5397    0.0000 [ 0.5574; 0.7673 ] 
#>   eta3 ~ eta1      0.4585      0.0930    4.9289    0.0000 [ 0.2013; 0.6824 ] 
#>   eta3 ~ eta2      0.3052      0.1019    2.9951    0.0027 [ 0.0542; 0.5811 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0461   14.3797    0.0000 [ 0.5430; 0.7815 ] 
#>   eta1 =~ y12      0.6493      0.0417   15.5643    0.0000 [ 0.5448; 0.7606 ] 
#>   eta1 =~ y13      0.7613      0.0336   22.6701    0.0000 [ 0.6837; 0.8574 ] 
#>   eta2 =~ y21      0.5165      0.0572    9.0297    0.0000 [ 0.3792; 0.6750 ] 
#>   eta2 =~ y22      0.7554      0.0450   16.7833    0.0000 [ 0.6465; 0.8792 ] 
#>   eta2 =~ y23      0.7997      0.0412   19.4019    0.0000 [ 0.6901; 0.9032 ] 
#>   eta3 =~ y31      0.8223      0.0285   28.8550    0.0000 [ 0.7508; 0.8981 ] 
#>   eta3 =~ y32      0.6581      0.0412   15.9826    0.0000 [ 0.5493; 0.7622 ] 
#>   eta3 =~ y33      0.7474      0.0377   19.8053    0.0000 [ 0.6496; 0.8447 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0252   15.7002    0.0000 [ 0.3268; 0.4571 ] 
#>   eta1 <~ y12      0.3873      0.0194   19.9618    0.0000 [ 0.3363; 0.4367 ] 
#>   eta1 <~ y13      0.4542      0.0228   19.9231    0.0000 [ 0.3969; 0.5148 ] 
#>   eta2 <~ y21      0.3058      0.0342    8.9458    0.0000 [ 0.2212; 0.3980 ] 
#>   eta2 <~ y22      0.4473      0.0237   18.8398    0.0000 [ 0.3866; 0.5094 ] 
#>   eta2 <~ y23      0.4735      0.0211   22.4280    0.0000 [ 0.4131; 0.5223 ] 
#>   eta3 <~ y31      0.4400      0.0146   30.0796    0.0000 [ 0.4036; 0.4792 ] 
#>   eta3 <~ y32      0.3521      0.0207   16.9940    0.0000 [ 0.2975; 0.4047 ] 
#>   eta3 <~ y33      0.3999      0.0180   22.2437    0.0000 [ 0.3536; 0.4466 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0406   16.5397    0.0000 [ 0.5574; 0.7673 ] 
#>   eta3 ~ eta1       0.6634      0.0381   17.3913    0.0000 [ 0.5545; 0.7517 ] 
#>   eta3 ~ eta2       0.3052      0.1019    2.9951    0.0027 [ 0.0542; 0.5811 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0675    3.0332    0.0024 [ 0.0366; 0.3859 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04058916 16.539724 1.898900e-61
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.09302406  4.928905 8.269163e-07
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.10188284  2.995118 2.743387e-03
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.55739730          0.7673015          0.5826027          0.7420961
#> 2         0.20129320          0.6823611          0.2590601          0.6245942
#> 3         0.05420971          0.5810902          0.1174778          0.5178222
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5619516          0.7564193          0.5796332          0.7360178
#> 2          0.2833234          0.6288185          0.2921805          0.6174017
#> 3          0.1275570          0.5085140          0.1469828          0.4880273
```
