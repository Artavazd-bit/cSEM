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
#>  Random seed                        = 1101741650
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
#>   eta2 ~ eta1      0.6713      0.0435   15.4210    0.0000 [ 0.6285; 0.7650 ] 
#>   eta3 ~ eta1      0.4585      0.0853    5.3758    0.0000 [ 0.2887; 0.6141 ] 
#>   eta3 ~ eta2      0.3052      0.0848    3.5972    0.0003 [ 0.1768; 0.5046 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0381   17.3934    0.0000 [ 0.5964; 0.7337 ] 
#>   eta1 =~ y12      0.6493      0.0379   17.1313    0.0000 [ 0.5823; 0.7112 ] 
#>   eta1 =~ y13      0.7613      0.0372   20.4390    0.0000 [ 0.6961; 0.8223 ] 
#>   eta2 =~ y21      0.5165      0.0551    9.3681    0.0000 [ 0.4196; 0.6237 ] 
#>   eta2 =~ y22      0.7554      0.0394   19.1780    0.0000 [ 0.6848; 0.8316 ] 
#>   eta2 =~ y23      0.7997      0.0309   25.9121    0.0000 [ 0.7374; 0.8401 ] 
#>   eta3 =~ y31      0.8223      0.0344   23.9178    0.0000 [ 0.7673; 0.8828 ] 
#>   eta3 =~ y32      0.6581      0.0404   16.2723    0.0000 [ 0.5905; 0.7141 ] 
#>   eta3 =~ y33      0.7474      0.0411   18.2030    0.0000 [ 0.6815; 0.8264 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0201   19.6569    0.0000 [ 0.3676; 0.4426 ] 
#>   eta1 <~ y12      0.3873      0.0196   19.7841    0.0000 [ 0.3562; 0.4285 ] 
#>   eta1 <~ y13      0.4542      0.0191   23.8182    0.0000 [ 0.4179; 0.4850 ] 
#>   eta2 <~ y21      0.3058      0.0297   10.2929    0.0000 [ 0.2574; 0.3717 ] 
#>   eta2 <~ y22      0.4473      0.0209   21.4076    0.0000 [ 0.4205; 0.4932 ] 
#>   eta2 <~ y23      0.4735      0.0208   22.8190    0.0000 [ 0.4378; 0.5018 ] 
#>   eta3 <~ y31      0.4400      0.0184   23.8474    0.0000 [ 0.4119; 0.4849 ] 
#>   eta3 <~ y32      0.3521      0.0201   17.5531    0.0000 [ 0.3120; 0.3952 ] 
#>   eta3 <~ y33      0.3999      0.0202   19.8123    0.0000 [ 0.3671; 0.4411 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0435   15.4210    0.0000 [ 0.6285; 0.7650 ] 
#>   eta3 ~ eta1       0.6634      0.0419   15.8509    0.0000 [ 0.5992; 0.7491 ] 
#>   eta3 ~ eta2       0.3052      0.0848    3.5972    0.0003 [ 0.1768; 0.5046 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0571    3.5889    0.0003 [ 0.1225; 0.3508 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.03812200 17.393365  9.263249e-68
#> 2 eta1 =~ y12  Common factor 0.6492779 0.03790016 17.131273  8.673522e-66
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03724973 20.438959  7.532841e-93
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05512895  9.368123  7.383408e-21
#> 5 eta2 =~ y22  Common factor 0.7553877 0.03938834 19.177951  5.656632e-82
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03086058 25.912144 4.859856e-148
#> 7 eta3 =~ y31  Common factor 0.8222773 0.03437932 23.917789 2.000201e-126
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04044094 16.272345  1.551032e-59
#> 9 eta3 =~ y33  Common factor 0.7474241 0.04106042 18.203031  4.882931e-74
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5964492          0.7337269
#> 2          0.5823189          0.7112274
#> 3          0.6960898          0.8222841
#> 4          0.4195523          0.6236882
#> 5          0.6847731          0.8316425
#> 6          0.7373705          0.8400724
#> 7          0.7673203          0.8827765
#> 8          0.5904553          0.7140541
#> 9          0.6814627          0.8263601

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
#>  Random seed                        = 1101741650
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
#>   eta2 ~ eta1      0.6713      0.0435   15.4210    0.0000 [ 0.5494; 0.7745 ] 
#>   eta3 ~ eta1      0.4585      0.0853    5.3758    0.0000 [ 0.2437; 0.6848 ] 
#>   eta3 ~ eta2      0.3052      0.0848    3.5972    0.0003 [ 0.0830; 0.5217 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0381   17.3934    0.0000 [ 0.5609; 0.7580 ] 
#>   eta1 =~ y12      0.6493      0.0379   17.1313    0.0000 [ 0.5590; 0.7550 ] 
#>   eta1 =~ y13      0.7613      0.0372   20.4390    0.0000 [ 0.6689; 0.8615 ] 
#>   eta2 =~ y21      0.5165      0.0551    9.3681    0.0000 [ 0.3784; 0.6635 ] 
#>   eta2 =~ y22      0.7554      0.0394   19.1780    0.0000 [ 0.6538; 0.8575 ] 
#>   eta2 =~ y23      0.7997      0.0309   25.9121    0.0000 [ 0.7262; 0.8858 ] 
#>   eta3 =~ y31      0.8223      0.0344   23.9178    0.0000 [ 0.7295; 0.9072 ] 
#>   eta3 =~ y32      0.6581      0.0404   16.2723    0.0000 [ 0.5632; 0.7724 ] 
#>   eta3 =~ y33      0.7474      0.0411   18.2030    0.0000 [ 0.6398; 0.8522 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0201   19.6569    0.0000 [ 0.3393; 0.4433 ] 
#>   eta1 <~ y12      0.3873      0.0196   19.7841    0.0000 [ 0.3393; 0.4405 ] 
#>   eta1 <~ y13      0.4542      0.0191   23.8182    0.0000 [ 0.4048; 0.5034 ] 
#>   eta2 <~ y21      0.3058      0.0297   10.2929    0.0000 [ 0.2299; 0.3836 ] 
#>   eta2 <~ y22      0.4473      0.0209   21.4076    0.0000 [ 0.3905; 0.4985 ] 
#>   eta2 <~ y23      0.4735      0.0208   22.8190    0.0000 [ 0.4203; 0.5276 ] 
#>   eta3 <~ y31      0.4400      0.0184   23.8474    0.0000 [ 0.3892; 0.4846 ] 
#>   eta3 <~ y32      0.3521      0.0201   17.5531    0.0000 [ 0.3048; 0.4085 ] 
#>   eta3 <~ y33      0.3999      0.0202   19.8123    0.0000 [ 0.3462; 0.4505 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0435   15.4210    0.0000 [ 0.5494; 0.7745 ] 
#>   eta3 ~ eta1       0.6634      0.0419   15.8509    0.0000 [ 0.5568; 0.7732 ] 
#>   eta3 ~ eta2       0.3052      0.0848    3.5972    0.0003 [ 0.0830; 0.5217 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0571    3.5889    0.0003 [ 0.0531; 0.3483 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.04353361 15.421036 1.181927e-53
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.08529112  5.375785 7.624959e-08
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.08482970  3.597220 3.216359e-04
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1         0.54935768          0.7744889          0.5763915          0.7474551
#> 2         0.24373089          0.6848084          0.2966957          0.6318436
#> 3         0.08304576          0.5217371          0.1357240          0.4690588
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5787643          0.7704601          0.6285414          0.7650254
#> 2          0.2596394          0.6227308          0.2887485          0.6140560
#> 3          0.1361033          0.5121818          0.1768247          0.5046485
```
