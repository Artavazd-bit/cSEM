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
#>  Random seed                        = 1268196911
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
#>   eta2 ~ eta1      0.6713      0.0489   13.7379    0.0000 [ 0.6071; 0.7585 ] 
#>   eta3 ~ eta1      0.4585      0.0843    5.4421    0.0000 [ 0.2998; 0.6114 ] 
#>   eta3 ~ eta2      0.3052      0.0894    3.4121    0.0006 [ 0.1419; 0.5053 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0471   14.0740    0.0000 [ 0.5786; 0.7528 ] 
#>   eta1 =~ y12      0.6493      0.0335   19.3768    0.0000 [ 0.6000; 0.7253 ] 
#>   eta1 =~ y13      0.7613      0.0271   28.1286    0.0000 [ 0.7114; 0.7999 ] 
#>   eta2 =~ y21      0.5165      0.0563    9.1793    0.0000 [ 0.4159; 0.6075 ] 
#>   eta2 =~ y22      0.7554      0.0306   24.6827    0.0000 [ 0.6966; 0.8065 ] 
#>   eta2 =~ y23      0.7997      0.0403   19.8393    0.0000 [ 0.7225; 0.8714 ] 
#>   eta3 =~ y31      0.8223      0.0292   28.1259    0.0000 [ 0.7807; 0.8779 ] 
#>   eta3 =~ y32      0.6581      0.0372   17.7004    0.0000 [ 0.5796; 0.7146 ] 
#>   eta3 =~ y33      0.7474      0.0286   26.1648    0.0000 [ 0.7032; 0.8115 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0207   19.0984    0.0000 [ 0.3599; 0.4243 ] 
#>   eta1 <~ y12      0.3873      0.0172   22.5555    0.0000 [ 0.3637; 0.4238 ] 
#>   eta1 <~ y13      0.4542      0.0219   20.7169    0.0000 [ 0.4140; 0.4910 ] 
#>   eta2 <~ y21      0.3058      0.0290   10.5513    0.0000 [ 0.2425; 0.3454 ] 
#>   eta2 <~ y22      0.4473      0.0212   21.1447    0.0000 [ 0.4058; 0.4925 ] 
#>   eta2 <~ y23      0.4735      0.0195   24.3218    0.0000 [ 0.4410; 0.5063 ] 
#>   eta3 <~ y31      0.4400      0.0160   27.5723    0.0000 [ 0.4109; 0.4662 ] 
#>   eta3 <~ y32      0.3521      0.0129   27.3114    0.0000 [ 0.3200; 0.3674 ] 
#>   eta3 <~ y33      0.3999      0.0171   23.3223    0.0000 [ 0.3638; 0.4293 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0489   13.7379    0.0000 [ 0.6071; 0.7585 ] 
#>   eta3 ~ eta1       0.6634      0.0388   17.1189    0.0000 [ 0.6105; 0.7333 ] 
#>   eta3 ~ eta2       0.3052      0.0894    3.4121    0.0006 [ 0.1419; 0.5053 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0587    3.4914    0.0005 [ 0.1012; 0.3577 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04711322 14.073967  5.490100e-45
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03350804 19.376781  1.211949e-83
#> 3 eta1 =~ y13  Common factor 0.7613458 0.02706663 28.128578 4.382047e-174
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05626291  9.179311  4.338571e-20
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03060390 24.682722 1.639654e-134
#> 6 eta2 =~ y23  Common factor 0.7996637 0.04030713 19.839264  1.364408e-87
#> 7 eta3 =~ y31  Common factor 0.8222773 0.02923557 28.125920 4.722634e-174
#> 8 eta3 =~ y32  Common factor 0.6580689 0.03717820 17.700399  4.163470e-70
#> 9 eta3 =~ y33  Common factor 0.7474241 0.02856604 26.164775 6.692402e-151
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5785862          0.7528309
#> 2          0.6000316          0.7253046
#> 3          0.7114394          0.7999082
#> 4          0.4159016          0.6074805
#> 5          0.6965760          0.8064739
#> 6          0.7225218          0.8714331
#> 7          0.7806586          0.8779431
#> 8          0.5795702          0.7146196
#> 9          0.7032140          0.8114512

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
#>  Random seed                        = 1268196911
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
#>   eta2 ~ eta1      0.6713      0.0489   13.7379    0.0000 [ 0.5357; 0.7884 ] 
#>   eta3 ~ eta1      0.4585      0.0843    5.4421    0.0000 [ 0.2232; 0.6589 ] 
#>   eta3 ~ eta2      0.3052      0.0894    3.4121    0.0006 [ 0.0845; 0.5470 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0471   14.0740    0.0000 [ 0.5383; 0.7820 ] 
#>   eta1 =~ y12      0.6493      0.0335   19.3768    0.0000 [ 0.5544; 0.7277 ] 
#>   eta1 =~ y13      0.7613      0.0271   28.1286    0.0000 [ 0.6913; 0.8313 ] 
#>   eta2 =~ y21      0.5165      0.0563    9.1793    0.0000 [ 0.3853; 0.6763 ] 
#>   eta2 =~ y22      0.7554      0.0306   24.6827    0.0000 [ 0.6722; 0.8305 ] 
#>   eta2 =~ y23      0.7997      0.0403   19.8393    0.0000 [ 0.6937; 0.9021 ] 
#>   eta3 =~ y31      0.8223      0.0292   28.1259    0.0000 [ 0.7484; 0.8995 ] 
#>   eta3 =~ y32      0.6581      0.0372   17.7004    0.0000 [ 0.5713; 0.7636 ] 
#>   eta3 =~ y33      0.7474      0.0286   26.1648    0.0000 [ 0.6687; 0.8164 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0207   19.0984    0.0000 [ 0.3439; 0.4510 ] 
#>   eta1 <~ y12      0.3873      0.0172   22.5555    0.0000 [ 0.3413; 0.4301 ] 
#>   eta1 <~ y13      0.4542      0.0219   20.7169    0.0000 [ 0.4009; 0.5143 ] 
#>   eta2 <~ y21      0.3058      0.0290   10.5513    0.0000 [ 0.2387; 0.3886 ] 
#>   eta2 <~ y22      0.4473      0.0212   21.1447    0.0000 [ 0.3884; 0.4978 ] 
#>   eta2 <~ y23      0.4735      0.0195   24.3218    0.0000 [ 0.4205; 0.5212 ] 
#>   eta3 <~ y31      0.4400      0.0160   27.5723    0.0000 [ 0.3979; 0.4804 ] 
#>   eta3 <~ y32      0.3521      0.0129   27.3114    0.0000 [ 0.3228; 0.3895 ] 
#>   eta3 <~ y33      0.3999      0.0171   23.3223    0.0000 [ 0.3513; 0.4400 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0489   13.7379    0.0000 [ 0.5357; 0.7884 ] 
#>   eta3 ~ eta1       0.6634      0.0388   17.1189    0.0000 [ 0.5516; 0.7520 ] 
#>   eta3 ~ eta2       0.3052      0.0894    3.4121    0.0006 [ 0.0845; 0.5470 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0587    3.4914    0.0005 [ 0.0590; 0.3625 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04886714 13.737931 6.017110e-43
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.08425126  5.442136 5.264555e-08
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.08943319  3.412057 6.447468e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.53566529          0.7883786          0.5660112          0.7580326
#> 2         0.22322982          0.6589297          0.2755489          0.6066107
#> 3         0.08446672          0.5469647          0.1400037          0.4914277
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5871759          0.7719851          0.6071260          0.7585409
#> 2          0.2908418          0.6142451          0.2998433          0.6114027
#> 3          0.1285101          0.5156450          0.1419148          0.5053353
```
