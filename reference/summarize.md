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
#>  Random seed                        = 950108728
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
#>   eta2 ~ eta1      0.6713      0.0480   13.9991    0.0000 [ 0.5845; 0.7427 ] 
#>   eta3 ~ eta1      0.4585      0.0713    6.4262    0.0000 [ 0.3525; 0.6120 ] 
#>   eta3 ~ eta2      0.3052      0.0749    4.0715    0.0000 [ 0.1323; 0.4020 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0359   18.4468    0.0000 [ 0.6008; 0.7235 ] 
#>   eta1 =~ y12      0.6493      0.0493   13.1699    0.0000 [ 0.5553; 0.7333 ] 
#>   eta1 =~ y13      0.7613      0.0327   23.2790    0.0000 [ 0.7068; 0.8308 ] 
#>   eta2 =~ y21      0.5165      0.0495   10.4284    0.0000 [ 0.4115; 0.5916 ] 
#>   eta2 =~ y22      0.7554      0.0369   20.4869    0.0000 [ 0.6694; 0.8050 ] 
#>   eta2 =~ y23      0.7997      0.0387   20.6452    0.0000 [ 0.7274; 0.8585 ] 
#>   eta3 =~ y31      0.8223      0.0370   22.2031    0.0000 [ 0.7491; 0.8857 ] 
#>   eta3 =~ y32      0.6581      0.0474   13.8764    0.0000 [ 0.5704; 0.7439 ] 
#>   eta3 =~ y33      0.7474      0.0396   18.8674    0.0000 [ 0.6548; 0.8037 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0210   18.8620    0.0000 [ 0.3654; 0.4283 ] 
#>   eta1 <~ y12      0.3873      0.0226   17.1161    0.0000 [ 0.3439; 0.4257 ] 
#>   eta1 <~ y13      0.4542      0.0194   23.3791    0.0000 [ 0.4256; 0.4916 ] 
#>   eta2 <~ y21      0.3058      0.0276   11.0937    0.0000 [ 0.2456; 0.3512 ] 
#>   eta2 <~ y22      0.4473      0.0204   21.9069    0.0000 [ 0.4116; 0.4739 ] 
#>   eta2 <~ y23      0.4735      0.0204   23.2239    0.0000 [ 0.4440; 0.5128 ] 
#>   eta3 <~ y31      0.4400      0.0195   22.5853    0.0000 [ 0.4155; 0.4787 ] 
#>   eta3 <~ y32      0.3521      0.0231   15.2122    0.0000 [ 0.3130; 0.3901 ] 
#>   eta3 <~ y33      0.3999      0.0221   18.0709    0.0000 [ 0.3583; 0.4364 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0480   13.9991    0.0000 [ 0.5845; 0.7427 ] 
#>   eta3 ~ eta1       0.6634      0.0359   18.4769    0.0000 [ 0.6021; 0.7289 ] 
#>   eta3 ~ eta2       0.3052      0.0749    4.0715    0.0000 [ 0.1323; 0.4020 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0519    3.9495    0.0001 [ 0.0901; 0.2826 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err   t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03594489 18.44685  5.527736e-76
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04930024 13.16987  1.308172e-39
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03270522 23.27903 7.231810e-120
#> 4 eta2 =~ y21  Common factor 0.5164548 0.04952408 10.42836  1.840410e-25
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03687172 20.48691  2.816988e-93
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03873357 20.64524  1.077310e-94
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03703435 22.20310 3.205516e-109
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04742361 13.87640  8.805812e-44
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03961450 18.86744  2.112930e-79
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.6007567          0.7235157
#> 2          0.5553016          0.7333487
#> 3          0.7067840          0.8308091
#> 4          0.4114626          0.5915728
#> 5          0.6693706          0.8050413
#> 6          0.7274310          0.8585070
#> 7          0.7490838          0.8856536
#> 8          0.5704383          0.7438766
#> 9          0.6547628          0.8037203

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
#>  Random seed                        = 950108728
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
#>   eta2 ~ eta1      0.6713      0.0480   13.9991    0.0000 [ 0.5496; 0.7976 ] 
#>   eta3 ~ eta1      0.4585      0.0713    6.4262    0.0000 [ 0.2559; 0.6248 ] 
#>   eta3 ~ eta2      0.3052      0.0749    4.0715    0.0000 [ 0.1335; 0.5211 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0359   18.4468    0.0000 [ 0.5722; 0.7580 ] 
#>   eta1 =~ y12      0.6493      0.0493   13.1699    0.0000 [ 0.5168; 0.7718 ] 
#>   eta1 =~ y13      0.7613      0.0327   23.2790    0.0000 [ 0.6772; 0.8464 ] 
#>   eta2 =~ y21      0.5165      0.0495   10.4284    0.0000 [ 0.3884; 0.6445 ] 
#>   eta2 =~ y22      0.7554      0.0369   20.4869    0.0000 [ 0.6660; 0.8567 ] 
#>   eta2 =~ y23      0.7997      0.0387   20.6452    0.0000 [ 0.7049; 0.9052 ] 
#>   eta3 =~ y31      0.8223      0.0370   22.2031    0.0000 [ 0.7225; 0.9141 ] 
#>   eta3 =~ y32      0.6581      0.0474   13.8764    0.0000 [ 0.5366; 0.7818 ] 
#>   eta3 =~ y33      0.7474      0.0396   18.8674    0.0000 [ 0.6483; 0.8531 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0210   18.8620    0.0000 [ 0.3433; 0.4518 ] 
#>   eta1 <~ y12      0.3873      0.0226   17.1161    0.0000 [ 0.3270; 0.4441 ] 
#>   eta1 <~ y13      0.4542      0.0194   23.3791    0.0000 [ 0.4051; 0.5055 ] 
#>   eta2 <~ y21      0.3058      0.0276   11.0937    0.0000 [ 0.2323; 0.3749 ] 
#>   eta2 <~ y22      0.4473      0.0204   21.9069    0.0000 [ 0.3946; 0.5002 ] 
#>   eta2 <~ y23      0.4735      0.0204   23.2239    0.0000 [ 0.4204; 0.5258 ] 
#>   eta3 <~ y31      0.4400      0.0195   22.5853    0.0000 [ 0.3877; 0.4884 ] 
#>   eta3 <~ y32      0.3521      0.0231   15.2122    0.0000 [ 0.2932; 0.4129 ] 
#>   eta3 <~ y33      0.3999      0.0221   18.0709    0.0000 [ 0.3446; 0.4590 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0480   13.9991    0.0000 [ 0.5496; 0.7976 ] 
#>   eta3 ~ eta1       0.6634      0.0359   18.4769    0.0000 [ 0.5682; 0.7539 ] 
#>   eta3 ~ eta2       0.3052      0.0749    4.0715    0.0000 [ 0.1335; 0.5211 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0519    3.9495    0.0001 [ 0.0866; 0.3548 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04795542 13.999114 1.578263e-44
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.07134921  6.426235 1.308034e-10
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.07494818  4.071495 4.671238e-05
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1          0.5496292          0.7976276          0.5794090          0.7678478
#> 2          0.2558583          0.6248361          0.3001653          0.5805291
#> 3          0.1334629          0.5210525          0.1800048          0.4745106
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5663824          0.7535298          0.5845285          0.7427058
#> 2          0.3389936          0.6155643          0.3525285          0.6120224
#> 3          0.1007633          0.4054711          0.1323321          0.4019630
```
