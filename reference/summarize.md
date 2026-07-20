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
#>  Random seed                        = -1686906343
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
#>   eta2 ~ eta1      0.6713      0.0420   15.9797    0.0000 [ 0.6148; 0.7347 ] 
#>   eta3 ~ eta1      0.4585      0.0991    4.6252    0.0000 [ 0.2658; 0.6417 ] 
#>   eta3 ~ eta2      0.3052      0.1106    2.7592    0.0058 [ 0.0761; 0.5091 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0334   19.8763    0.0000 [ 0.6087; 0.7328 ] 
#>   eta1 =~ y12      0.6493      0.0386   16.8023    0.0000 [ 0.5768; 0.6971 ] 
#>   eta1 =~ y13      0.7613      0.0374   20.3448    0.0000 [ 0.6864; 0.8142 ] 
#>   eta2 =~ y21      0.5165      0.0530    9.7377    0.0000 [ 0.4032; 0.5905 ] 
#>   eta2 =~ y22      0.7554      0.0366   20.6436    0.0000 [ 0.6942; 0.8214 ] 
#>   eta2 =~ y23      0.7997      0.0375   21.3437    0.0000 [ 0.7372; 0.8656 ] 
#>   eta3 =~ y31      0.8223      0.0255   32.1864    0.0000 [ 0.7767; 0.8548 ] 
#>   eta3 =~ y32      0.6581      0.0486   13.5379    0.0000 [ 0.5786; 0.7461 ] 
#>   eta3 =~ y33      0.7474      0.0475   15.7363    0.0000 [ 0.6592; 0.8200 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0216   18.3428    0.0000 [ 0.3664; 0.4484 ] 
#>   eta1 <~ y12      0.3873      0.0213   18.1599    0.0000 [ 0.3429; 0.4152 ] 
#>   eta1 <~ y13      0.4542      0.0178   25.5325    0.0000 [ 0.4196; 0.4759 ] 
#>   eta2 <~ y21      0.3058      0.0298   10.2736    0.0000 [ 0.2386; 0.3474 ] 
#>   eta2 <~ y22      0.4473      0.0212   21.1480    0.0000 [ 0.4164; 0.4934 ] 
#>   eta2 <~ y23      0.4735      0.0219   21.6194    0.0000 [ 0.4431; 0.5202 ] 
#>   eta3 <~ y31      0.4400      0.0193   22.8380    0.0000 [ 0.4084; 0.4756 ] 
#>   eta3 <~ y32      0.3521      0.0208   16.9404    0.0000 [ 0.3179; 0.3894 ] 
#>   eta3 <~ y33      0.3999      0.0216   18.5093    0.0000 [ 0.3560; 0.4401 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0420   15.9797    0.0000 [ 0.6148; 0.7347 ] 
#>   eta3 ~ eta1       0.6634      0.0398   16.6531    0.0000 [ 0.5890; 0.7459 ] 
#>   eta3 ~ eta2       0.3052      0.1106    2.7592    0.0058 [ 0.0761; 0.5091 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0740    2.7674    0.0056 [ 0.0491; 0.3646 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03335978 19.876328  6.523675e-88
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03864210 16.802346  2.345812e-63
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03742207 20.344833  5.158793e-92
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05303673  9.737682  2.082500e-22
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03659177 20.643648  1.113343e-94
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03746598 21.343729 4.459226e-101
#> 7 eta3 =~ y31  Common factor 0.8222773 0.02554736 32.186389 2.736542e-227
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04860926 13.537934  9.337899e-42
#> 9 eta3 =~ y33  Common factor 0.7474241 0.04749666 15.736350  8.522003e-56
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.6087349          0.7328395
#> 2          0.5768174          0.6970648
#> 3          0.6864219          0.8142260
#> 4          0.4032261          0.5905272
#> 5          0.6942201          0.8213761
#> 6          0.7371550          0.8655815
#> 7          0.7766567          0.8548205
#> 8          0.5786204          0.7461286
#> 9          0.6591506          0.8199810

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
#>  Random seed                        = -1686906343
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
#>   eta2 ~ eta1      0.6713      0.0420   15.9797    0.0000 [ 0.5615; 0.7788 ] 
#>   eta3 ~ eta1      0.4585      0.0991    4.6252    0.0000 [ 0.2037; 0.7164 ] 
#>   eta3 ~ eta2      0.3052      0.1106    2.7592    0.0058 [ 0.0195; 0.5914 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0334   19.8763    0.0000 [ 0.5665; 0.7391 ] 
#>   eta1 =~ y12      0.6493      0.0386   16.8023    0.0000 [ 0.5484; 0.7482 ] 
#>   eta1 =~ y13      0.7613      0.0374   20.3448    0.0000 [ 0.6707; 0.8643 ] 
#>   eta2 =~ y21      0.5165      0.0530    9.7377    0.0000 [ 0.3900; 0.6642 ] 
#>   eta2 =~ y22      0.7554      0.0366   20.6436    0.0000 [ 0.6686; 0.8579 ] 
#>   eta2 =~ y23      0.7997      0.0375   21.3437    0.0000 [ 0.7022; 0.8959 ] 
#>   eta3 =~ y31      0.8223      0.0255   32.1864    0.0000 [ 0.7568; 0.8889 ] 
#>   eta3 =~ y32      0.6581      0.0486   13.5379    0.0000 [ 0.5193; 0.7707 ] 
#>   eta3 =~ y33      0.7474      0.0475   15.7363    0.0000 [ 0.6345; 0.8801 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0216   18.3428    0.0000 [ 0.3351; 0.4466 ] 
#>   eta1 <~ y12      0.3873      0.0213   18.1599    0.0000 [ 0.3332; 0.4435 ] 
#>   eta1 <~ y13      0.4542      0.0178   25.5325    0.0000 [ 0.4138; 0.5058 ] 
#>   eta2 <~ y21      0.3058      0.0298   10.2736    0.0000 [ 0.2321; 0.3860 ] 
#>   eta2 <~ y22      0.4473      0.0212   21.1480    0.0000 [ 0.3924; 0.5018 ] 
#>   eta2 <~ y23      0.4735      0.0219   21.6194    0.0000 [ 0.4113; 0.5245 ] 
#>   eta3 <~ y31      0.4400      0.0193   22.8380    0.0000 [ 0.3910; 0.4906 ] 
#>   eta3 <~ y32      0.3521      0.0208   16.9404    0.0000 [ 0.2923; 0.3998 ] 
#>   eta3 <~ y33      0.3999      0.0216   18.5093    0.0000 [ 0.3502; 0.4620 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0420   15.9797    0.0000 [ 0.5615; 0.7788 ] 
#>   eta3 ~ eta1       0.6634      0.0398   16.6531    0.0000 [ 0.5631; 0.7691 ] 
#>   eta3 ~ eta2       0.3052      0.1106    2.7592    0.0058 [ 0.0195; 0.5914 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0740    2.7674    0.0056 [ 0.0147; 0.3975 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04201166 15.979693 1.770166e-57
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.09913185  4.625221 3.741983e-06
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.11059593  2.759153 5.795133e-03
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.56152209          0.7787827         0.58761084          0.7526939
#> 2         0.20372529          0.7163792         0.26528501          0.6548195
#> 3         0.01949158          0.5914313         0.08817036          0.5227525
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1         0.54380027          0.7692254          0.6148233          0.7346772
#> 2         0.23170994          0.6675947          0.2658029          0.6417036
#> 3         0.04598692          0.5554881          0.0760509          0.5091176
```
