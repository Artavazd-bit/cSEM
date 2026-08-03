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
#>  Random seed                        = 1980041663
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
#>   eta2 ~ eta1      0.6713      0.0582   11.5309    0.0000 [ 0.5548; 0.7601 ] 
#>   eta3 ~ eta1      0.4585      0.0919    4.9917    0.0000 [ 0.3255; 0.6239 ] 
#>   eta3 ~ eta2      0.3052      0.0853    3.5766    0.0003 [ 0.1629; 0.4366 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0477   13.9106    0.0000 [ 0.5819; 0.7470 ] 
#>   eta1 =~ y12      0.6493      0.0422   15.3935    0.0000 [ 0.5676; 0.7289 ] 
#>   eta1 =~ y13      0.7613      0.0348   21.8559    0.0000 [ 0.6968; 0.8107 ] 
#>   eta2 =~ y21      0.5165      0.0514   10.0552    0.0000 [ 0.4329; 0.6107 ] 
#>   eta2 =~ y22      0.7554      0.0377   20.0536    0.0000 [ 0.6933; 0.8169 ] 
#>   eta2 =~ y23      0.7997      0.0360   22.2292    0.0000 [ 0.7274; 0.8684 ] 
#>   eta3 =~ y31      0.8223      0.0314   26.1466    0.0000 [ 0.7742; 0.8864 ] 
#>   eta3 =~ y32      0.6581      0.0429   15.3477    0.0000 [ 0.5713; 0.7247 ] 
#>   eta3 =~ y33      0.7474      0.0454   16.4452    0.0000 [ 0.6655; 0.8451 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0243   16.2834    0.0000 [ 0.3436; 0.4311 ] 
#>   eta1 <~ y12      0.3873      0.0249   15.5579    0.0000 [ 0.3454; 0.4307 ] 
#>   eta1 <~ y13      0.4542      0.0217   20.9204    0.0000 [ 0.4120; 0.4898 ] 
#>   eta2 <~ y21      0.3058      0.0251   12.1735    0.0000 [ 0.2670; 0.3497 ] 
#>   eta2 <~ y22      0.4473      0.0226   19.8343    0.0000 [ 0.4059; 0.4868 ] 
#>   eta2 <~ y23      0.4735      0.0228   20.7343    0.0000 [ 0.4377; 0.5077 ] 
#>   eta3 <~ y31      0.4400      0.0199   22.1627    0.0000 [ 0.4071; 0.4770 ] 
#>   eta3 <~ y32      0.3521      0.0190   18.5370    0.0000 [ 0.3073; 0.3798 ] 
#>   eta3 <~ y33      0.3999      0.0229   17.4993    0.0000 [ 0.3585; 0.4494 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0582   11.5309    0.0000 [ 0.5548; 0.7601 ] 
#>   eta3 ~ eta1       0.6634      0.0551   12.0356    0.0000 [ 0.5760; 0.7643 ] 
#>   eta3 ~ eta2       0.3052      0.0853    3.5766    0.0003 [ 0.1629; 0.4366 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0559    3.6671    0.0002 [ 0.1140; 0.2831 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err   t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04766639 13.91064  5.459179e-44
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04217860 15.39354  1.808645e-53
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03483473 21.85594 6.824634e-106
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05136207 10.05518  8.716283e-24
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03766836 20.05364  1.876142e-89
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03597353 22.22922 1.792064e-109
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03144873 26.14660 1.077379e-150
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04287745 15.34767  3.671543e-53
#> 9 eta3 =~ y33  Common factor 0.7474241 0.04544939 16.44520  9.079093e-61
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5819390          0.7470360
#> 2          0.5675850          0.7288731
#> 3          0.6967716          0.8107469
#> 4          0.4329238          0.6107445
#> 5          0.6932950          0.8168691
#> 6          0.7273928          0.8683860
#> 7          0.7742332          0.8864391
#> 8          0.5713063          0.7247269
#> 9          0.6655398          0.8451225

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
#>  Random seed                        = 1980041663
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
#>   eta2 ~ eta1      0.6713      0.0582   11.5309    0.0000 [ 0.5311; 0.8322 ] 
#>   eta3 ~ eta1      0.4585      0.0919    4.9917    0.0000 [ 0.2307; 0.7057 ] 
#>   eta3 ~ eta2      0.3052      0.0853    3.5766    0.0003 [ 0.0724; 0.5136 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0477   13.9106    0.0000 [ 0.5473; 0.7938 ] 
#>   eta1 =~ y12      0.6493      0.0422   15.3935    0.0000 [ 0.5410; 0.7591 ] 
#>   eta1 =~ y13      0.7613      0.0348   21.8559    0.0000 [ 0.6731; 0.8533 ] 
#>   eta2 =~ y21      0.5165      0.0514   10.0552    0.0000 [ 0.3719; 0.6375 ] 
#>   eta2 =~ y22      0.7554      0.0377   20.0536    0.0000 [ 0.6714; 0.8662 ] 
#>   eta2 =~ y23      0.7997      0.0360   22.2292    0.0000 [ 0.7081; 0.8942 ] 
#>   eta3 =~ y31      0.8223      0.0314   26.1466    0.0000 [ 0.7391; 0.9017 ] 
#>   eta3 =~ y32      0.6581      0.0429   15.3477    0.0000 [ 0.5558; 0.7776 ] 
#>   eta3 =~ y33      0.7474      0.0454   16.4452    0.0000 [ 0.6281; 0.8631 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0243   16.2834    0.0000 [ 0.3349; 0.4605 ] 
#>   eta1 <~ y12      0.3873      0.0249   15.5579    0.0000 [ 0.3209; 0.4496 ] 
#>   eta1 <~ y13      0.4542      0.0217   20.9204    0.0000 [ 0.3961; 0.5084 ] 
#>   eta2 <~ y21      0.3058      0.0251   12.1735    0.0000 [ 0.2334; 0.3633 ] 
#>   eta2 <~ y22      0.4473      0.0226   19.8343    0.0000 [ 0.3955; 0.5122 ] 
#>   eta2 <~ y23      0.4735      0.0228   20.7343    0.0000 [ 0.4138; 0.5319 ] 
#>   eta3 <~ y31      0.4400      0.0199   22.1627    0.0000 [ 0.3864; 0.4890 ] 
#>   eta3 <~ y32      0.3521      0.0190   18.5370    0.0000 [ 0.3069; 0.4052 ] 
#>   eta3 <~ y33      0.3999      0.0229   17.4993    0.0000 [ 0.3389; 0.4571 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0582   11.5309    0.0000 [ 0.5311; 0.8322 ] 
#>   eta3 ~ eta1       0.6634      0.0551   12.0356    0.0000 [ 0.5272; 0.8122 ] 
#>   eta3 ~ eta2       0.3052      0.0853    3.5766    0.0003 [ 0.0724; 0.5136 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0559    3.6671    0.0002 [ 0.0570; 0.3459 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.05822038 11.530901 9.217423e-31
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.09185341  4.991723 5.984312e-07
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.08531828  3.576621 3.480645e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.53110570          0.8321886          0.5672599          0.7960345
#> 2         0.23068875          0.7057027          0.2877286          0.6486628
#> 3         0.07240023          0.5136182          0.1253819          0.4606365
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5530679          0.7819865          0.5548140          0.7600846
#> 2          0.2818950          0.7062782          0.3254955          0.6238774
#> 3          0.0718501          0.4464380          0.1628619          0.4365672
```
