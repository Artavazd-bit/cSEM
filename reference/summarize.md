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
#>  Random seed                        = 566036623
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
#>   eta2 ~ eta1      0.6713      0.0376   17.8670    0.0000 [ 0.5874; 0.7291 ] 
#>   eta3 ~ eta1      0.4585      0.0786    5.8326    0.0000 [ 0.2985; 0.5752 ] 
#>   eta3 ~ eta2      0.3052      0.0810    3.7669    0.0002 [ 0.1768; 0.4507 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0418   15.8757    0.0000 [ 0.5651; 0.7273 ] 
#>   eta1 =~ y12      0.6493      0.0430   15.1011    0.0000 [ 0.5650; 0.7299 ] 
#>   eta1 =~ y13      0.7613      0.0339   22.4534    0.0000 [ 0.7075; 0.8337 ] 
#>   eta2 =~ y21      0.5165      0.0559    9.2309    0.0000 [ 0.4275; 0.6028 ] 
#>   eta2 =~ y22      0.7554      0.0353   21.4104    0.0000 [ 0.6916; 0.7944 ] 
#>   eta2 =~ y23      0.7997      0.0320   24.9603    0.0000 [ 0.7289; 0.8358 ] 
#>   eta3 =~ y31      0.8223      0.0330   24.8837    0.0000 [ 0.7573; 0.8646 ] 
#>   eta3 =~ y32      0.6581      0.0343   19.1599    0.0000 [ 0.5897; 0.7101 ] 
#>   eta3 =~ y33      0.7474      0.0354   21.1399    0.0000 [ 0.6814; 0.8086 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0247   16.0451    0.0000 [ 0.3455; 0.4324 ] 
#>   eta1 <~ y12      0.3873      0.0212   18.3132    0.0000 [ 0.3316; 0.4158 ] 
#>   eta1 <~ y13      0.4542      0.0202   22.4684    0.0000 [ 0.4257; 0.5032 ] 
#>   eta2 <~ y21      0.3058      0.0306    9.9845    0.0000 [ 0.2639; 0.3560 ] 
#>   eta2 <~ y22      0.4473      0.0224   19.9647    0.0000 [ 0.4118; 0.4964 ] 
#>   eta2 <~ y23      0.4735      0.0158   29.8889    0.0000 [ 0.4397; 0.4939 ] 
#>   eta3 <~ y31      0.4400      0.0170   25.8775    0.0000 [ 0.4041; 0.4665 ] 
#>   eta3 <~ y32      0.3521      0.0169   20.8402    0.0000 [ 0.3196; 0.3775 ] 
#>   eta3 <~ y33      0.3999      0.0183   21.8544    0.0000 [ 0.3749; 0.4422 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0376   17.8670    0.0000 [ 0.5874; 0.7291 ] 
#>   eta3 ~ eta1       0.6634      0.0399   16.6181    0.0000 [ 0.5816; 0.7181 ] 
#>   eta3 ~ eta2       0.3052      0.0810    3.7669    0.0002 [ 0.1768; 0.4507 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0492    4.1650    0.0000 [ 0.1219; 0.2943 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err   t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04176641 15.87567  9.340582e-57
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04299554 15.10105  1.593702e-51
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03390776 22.45344 1.184536e-111
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05594818  9.23095  2.682433e-20
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03528131 21.41042 1.068496e-101
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03203736 24.96035 1.648745e-137
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03304485 24.88367 1.117801e-136
#> 8 eta3 =~ y32  Common factor 0.6580689 0.03434624 19.15985  8.009620e-82
#> 9 eta3 =~ y33  Common factor 0.7474241 0.03535602 21.13994  3.415060e-99
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5650940          0.7273180
#> 2          0.5649852          0.7299032
#> 3          0.7074600          0.8337482
#> 4          0.4275137          0.6028147
#> 5          0.6916383          0.7944053
#> 6          0.7288749          0.8358059
#> 7          0.7573159          0.8646002
#> 8          0.5897352          0.7101383
#> 9          0.6813647          0.8085996

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
#>  Random seed                        = 566036623
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
#>   eta2 ~ eta1      0.6713      0.0376   17.8670    0.0000 [ 0.5781; 0.7724 ] 
#>   eta3 ~ eta1      0.4585      0.0786    5.8326    0.0000 [ 0.2595; 0.6661 ] 
#>   eta3 ~ eta2      0.3052      0.0810    3.7669    0.0002 [ 0.0963; 0.5152 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0418   15.8757    0.0000 [ 0.5594; 0.7754 ] 
#>   eta1 =~ y12      0.6493      0.0430   15.1011    0.0000 [ 0.5420; 0.7643 ] 
#>   eta1 =~ y13      0.7613      0.0339   22.4534    0.0000 [ 0.6692; 0.8446 ] 
#>   eta2 =~ y21      0.5165      0.0559    9.2309    0.0000 [ 0.3704; 0.6597 ] 
#>   eta2 =~ y22      0.7554      0.0353   21.4104    0.0000 [ 0.6679; 0.8504 ] 
#>   eta2 =~ y23      0.7997      0.0320   24.9603    0.0000 [ 0.7258; 0.8915 ] 
#>   eta3 =~ y31      0.8223      0.0330   24.8837    0.0000 [ 0.7385; 0.9094 ] 
#>   eta3 =~ y32      0.6581      0.0343   19.1599    0.0000 [ 0.5778; 0.7554 ] 
#>   eta3 =~ y33      0.7474      0.0354   21.1399    0.0000 [ 0.6505; 0.8333 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0247   16.0451    0.0000 [ 0.3336; 0.4611 ] 
#>   eta1 <~ y12      0.3873      0.0212   18.3132    0.0000 [ 0.3344; 0.4438 ] 
#>   eta1 <~ y13      0.4542      0.0202   22.4684    0.0000 [ 0.3983; 0.5029 ] 
#>   eta2 <~ y21      0.3058      0.0306    9.9845    0.0000 [ 0.2237; 0.3821 ] 
#>   eta2 <~ y22      0.4473      0.0224   19.9647    0.0000 [ 0.3881; 0.5039 ] 
#>   eta2 <~ y23      0.4735      0.0158   29.8889    0.0000 [ 0.4343; 0.5163 ] 
#>   eta3 <~ y31      0.4400      0.0170   25.8775    0.0000 [ 0.3958; 0.4837 ] 
#>   eta3 <~ y32      0.3521      0.0169   20.8402    0.0000 [ 0.3121; 0.3995 ] 
#>   eta3 <~ y33      0.3999      0.0183   21.8544    0.0000 [ 0.3486; 0.4433 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0376   17.8670    0.0000 [ 0.5781; 0.7724 ] 
#>   eta3 ~ eta1       0.6634      0.0399   16.6181    0.0000 [ 0.5674; 0.7738 ] 
#>   eta3 ~ eta2       0.3052      0.0810    3.7669    0.0002 [ 0.0963; 0.5152 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0492    4.1650    0.0000 [ 0.0806; 0.3350 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.03757384 17.867045 2.129830e-71
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.07861116  5.832591 5.457309e-09
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.08100769  3.766940 1.652607e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.57806025          0.7723709          0.6013932          0.7490380
#> 2         0.25953360          0.6660661          0.3083502          0.6172495
#> 3         0.09625523          0.5151813          0.1465601          0.4648765
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5661280          0.7327858          0.5873812          0.7290701
#> 2          0.2690883          0.6154873          0.2985193          0.5752212
#> 3          0.1424999          0.4963144          0.1768382          0.4506718
```
