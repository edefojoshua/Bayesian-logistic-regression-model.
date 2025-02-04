Bayesian logistic regression model
================
Joshua Edefo
2025-02-04

This Bayesian model demonstrates how to use to perform logistic
regression with simulated data. It begins by installing, loading the
necessary package and setting a seed for reproducibility. The data was
simulated which consists of 200 data points, with BMI values generated
from a normal distribution, and a binary outcome variable (heart
disease) generated using a logistic function based on BMI. After
visualizing the data, the code sets normal priors for the intercept and
slope of the logistic regression model, which links BMI to the
probability of heart disease. The model is then fitted using and the
summary of the model is printed.

Libraries

``` r
library(usethis)
```

    ## Warning: package 'usethis' was built under R version 4.3.2

``` r
## Install rstanarm 
library(rstanarm)
```

    ## Warning: package 'rstanarm' was built under R version 4.3.3

Data preparation and visualisation

``` r
## Simulating data
n <- 200  # Number of data points
BMI <- rnorm(n, mean = 28, sd = 5)  # BMI (mean = 28, SD = 5)
heart_disease <- rbinom(n, 1, prob = 1 / (1 + exp(-(0.1 * BMI - 3))))  # Logistic relationship between BMI and heart disease

## Data visualization
plot(BMI, heart_disease, main="BMI vs Heart Disease Risk", 
     xlab="BMI", ylab= "Heart Disease (0 = No, 1 = Yes)", 
     pch = 19, col = "blue")
```

![](Bayesian-model_files/figure-gfm/cars-1.png)<!-- -->

``` r
## Setting prior for the intercept and slope
prior_intercept <- normal(location = 2, scale = 1)  # Prior for intercept: Mean = 2, SD = 2
prior_slope <- normal(location = 2, scale = 1)  # Prior for slope: Mean = 2, SD = 2

## Print prior
prior_intercept
```

    ## $dist
    ## [1] "normal"
    ## 
    ## $df
    ## [1] NA
    ## 
    ## $location
    ## [1] 2
    ## 
    ## $scale
    ## [1] 1
    ## 
    ## $autoscale
    ## [1] FALSE

``` r
prior_slope
```

    ## $dist
    ## [1] "normal"
    ## 
    ## $df
    ## [1] NA
    ## 
    ## $location
    ## [1] 2
    ## 
    ## $scale
    ## [1] 1
    ## 
    ## $autoscale
    ## [1] FALSE

Fitting the model and getting the estimates

``` r
model <- stan_glm(heart_disease ~ BMI, 
                  family = binomial(link = "logit"),  # Logit link for logistic regression
                  data = data.frame(BMI, heart_disease),
                  prior = prior_slope, 
                  prior_intercept = prior_intercept,
                  chains = 4, iter = 2000, warmup = 1000)
```

    ## 
    ## SAMPLING FOR MODEL 'bernoulli' NOW (CHAIN 1).
    ## Chain 1: 
    ## Chain 1: Gradient evaluation took 3.2e-05 seconds
    ## Chain 1: 1000 transitions using 10 leapfrog steps per transition would take 0.32 seconds.
    ## Chain 1: Adjust your expectations accordingly!
    ## Chain 1: 
    ## Chain 1: 
    ## Chain 1: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 1: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 1: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 1: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 1: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 1: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 1: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 1: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 1: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 1: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 1: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 1: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 1: 
    ## Chain 1:  Elapsed Time: 0.044 seconds (Warm-up)
    ## Chain 1:                0.055 seconds (Sampling)
    ## Chain 1:                0.099 seconds (Total)
    ## Chain 1: 
    ## 
    ## SAMPLING FOR MODEL 'bernoulli' NOW (CHAIN 2).
    ## Chain 2: 
    ## Chain 2: Gradient evaluation took 1e-05 seconds
    ## Chain 2: 1000 transitions using 10 leapfrog steps per transition would take 0.1 seconds.
    ## Chain 2: Adjust your expectations accordingly!
    ## Chain 2: 
    ## Chain 2: 
    ## Chain 2: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 2: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 2: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 2: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 2: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 2: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 2: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 2: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 2: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 2: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 2: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 2: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 2: 
    ## Chain 2:  Elapsed Time: 0.042 seconds (Warm-up)
    ## Chain 2:                0.052 seconds (Sampling)
    ## Chain 2:                0.094 seconds (Total)
    ## Chain 2: 
    ## 
    ## SAMPLING FOR MODEL 'bernoulli' NOW (CHAIN 3).
    ## Chain 3: 
    ## Chain 3: Gradient evaluation took 1e-05 seconds
    ## Chain 3: 1000 transitions using 10 leapfrog steps per transition would take 0.1 seconds.
    ## Chain 3: Adjust your expectations accordingly!
    ## Chain 3: 
    ## Chain 3: 
    ## Chain 3: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 3: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 3: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 3: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 3: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 3: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 3: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 3: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 3: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 3: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 3: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 3: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 3: 
    ## Chain 3:  Elapsed Time: 0.043 seconds (Warm-up)
    ## Chain 3:                0.047 seconds (Sampling)
    ## Chain 3:                0.09 seconds (Total)
    ## Chain 3: 
    ## 
    ## SAMPLING FOR MODEL 'bernoulli' NOW (CHAIN 4).
    ## Chain 4: 
    ## Chain 4: Gradient evaluation took 1e-05 seconds
    ## Chain 4: 1000 transitions using 10 leapfrog steps per transition would take 0.1 seconds.
    ## Chain 4: Adjust your expectations accordingly!
    ## Chain 4: 
    ## Chain 4: 
    ## Chain 4: Iteration:    1 / 2000 [  0%]  (Warmup)
    ## Chain 4: Iteration:  200 / 2000 [ 10%]  (Warmup)
    ## Chain 4: Iteration:  400 / 2000 [ 20%]  (Warmup)
    ## Chain 4: Iteration:  600 / 2000 [ 30%]  (Warmup)
    ## Chain 4: Iteration:  800 / 2000 [ 40%]  (Warmup)
    ## Chain 4: Iteration: 1000 / 2000 [ 50%]  (Warmup)
    ## Chain 4: Iteration: 1001 / 2000 [ 50%]  (Sampling)
    ## Chain 4: Iteration: 1200 / 2000 [ 60%]  (Sampling)
    ## Chain 4: Iteration: 1400 / 2000 [ 70%]  (Sampling)
    ## Chain 4: Iteration: 1600 / 2000 [ 80%]  (Sampling)
    ## Chain 4: Iteration: 1800 / 2000 [ 90%]  (Sampling)
    ## Chain 4: Iteration: 2000 / 2000 [100%]  (Sampling)
    ## Chain 4: 
    ## Chain 4:  Elapsed Time: 0.045 seconds (Warm-up)
    ## Chain 4:                0.05 seconds (Sampling)
    ## Chain 4:                0.095 seconds (Total)
    ## Chain 4:

``` r
# Print the summary of the model
summary(model)
```

    ## 
    ## Model Info:
    ##  function:     stan_glm
    ##  family:       binomial [logit]
    ##  formula:      heart_disease ~ BMI
    ##  algorithm:    sampling
    ##  sample:       4000 (posterior sample size)
    ##  priors:       see help('prior_summary')
    ##  observations: 200
    ##  predictors:   2
    ## 
    ## Estimates:
    ##               mean   sd   10%   50%   90%
    ## (Intercept) -3.2    0.8 -4.3  -3.2  -2.1 
    ## BMI          0.1    0.0  0.1   0.1   0.1 
    ## 
    ## Fit Diagnostics:
    ##            mean   sd   10%   50%   90%
    ## mean_PPD 0.5    0.0  0.4   0.5   0.5  
    ## 
    ## The mean_ppd is the sample average posterior predictive distribution of the outcome variable (for details see help('summary.stanreg')).
    ## 
    ## MCMC diagnostics
    ##               mcse Rhat n_eff
    ## (Intercept)   0.0  1.0  2432 
    ## BMI           0.0  1.0  2439 
    ## mean_PPD      0.0  1.0  3373 
    ## log-posterior 0.0  1.0  2011 
    ## 
    ## For each parameter, mcse is Monte Carlo standard error, n_eff is a crude measure of effective sample size, and Rhat is the potential scale reduction factor on split chains (at convergence Rhat=1).

Results and interpretation The model includes two predictors: the
intercept and BMI. The estimated intercept is -1.8, suggesting that when
BMI is zero, the log-odds of heart disease are low. The coefficient for
BMI is 0.1, indicating a small but positive association between BMI and
heart disease risk. This means that as BMI increases, the likelihood of
heart disease also increases. The small standard deviation of the BMI
coefficient suggests that the estimate is relatively stable.

The model’s predictive performance is reflected in the mean posterior
predictive distribution (mean_PPD), which is approximately 0.5,
indicating an even probability of heart disease across the dataset. MCMC
diagnostics confirm that the model has converged, as all parameters have
an Rhat value of 1.0. The effective sample sizes (n_eff) are
sufficiently large, and the Monte Carlo standard errors (mcse) are low,
ensuring reliable and stable parameter estimates. Overall, the model
appears well-calibrated, though additional predictors could potentially
improve its accuracy.

In conclusion, the results suggest that BMI has a small but positive
effect on the risk of heart disease. The model has converged well, and
the estimates appear to be stable. However, further investigation could
explore additional predictors to improve the model’s accuracy.

session information

``` r
sessionInfo()
```

    ## R version 4.3.1 (2023-06-16 ucrt)
    ## Platform: x86_64-w64-mingw32/x64 (64-bit)
    ## Running under: Windows 11 x64 (build 22631)
    ## 
    ## Matrix products: default
    ## 
    ## 
    ## locale:
    ## [1] LC_COLLATE=English_United Kingdom.utf8 
    ## [2] LC_CTYPE=English_United Kingdom.utf8   
    ## [3] LC_MONETARY=English_United Kingdom.utf8
    ## [4] LC_NUMERIC=C                           
    ## [5] LC_TIME=English_United Kingdom.utf8    
    ## 
    ## time zone: Europe/London
    ## tzcode source: internal
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ## [1] rstanarm_2.32.1 Rcpp_1.0.11     usethis_2.2.2  
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] tidyselect_1.2.0     dplyr_1.1.4          farver_2.1.1        
    ##  [4] loo_2.7.0            fastmap_1.2.0        tensorA_0.36.2.1    
    ##  [7] shinystan_2.6.0      promises_1.2.1       shinyjs_2.1.0       
    ## [10] digest_0.6.33        mime_0.12            lifecycle_1.0.3     
    ## [13] StanHeaders_2.32.6   survival_3.7-0       magrittr_2.0.3      
    ## [16] posterior_1.5.0      compiler_4.3.1       rlang_1.1.1         
    ## [19] tools_4.3.1          igraph_2.1.2         utf8_1.2.3          
    ## [22] yaml_2.3.7           knitr_1.44           htmlwidgets_1.6.2   
    ## [25] pkgbuild_1.4.3       plyr_1.8.9           dygraphs_1.1.1.6    
    ## [28] abind_1.4-5          miniUI_0.1.1.1       purrr_1.0.2         
    ## [31] grid_4.3.1           stats4_4.3.1         fansi_1.0.4         
    ## [34] xts_0.13.2           xtable_1.8-4         colorspace_2.1-0    
    ## [37] inline_0.3.19        ggplot2_3.5.1        scales_1.3.0        
    ## [40] gtools_3.9.5         MASS_7.3-60          cli_3.6.1           
    ## [43] rmarkdown_2.25       generics_0.1.3       RcppParallel_5.1.7  
    ## [46] rstudioapi_0.15.0    reshape2_1.4.4       minqa_1.2.6         
    ## [49] rstan_2.32.6         stringr_1.5.0        shinythemes_1.2.0   
    ## [52] splines_4.3.1        bayesplot_1.11.1     parallel_4.3.1      
    ## [55] matrixStats_1.3.0    base64enc_0.1-3      vctrs_0.6.5         
    ## [58] boot_1.3-28.1        Matrix_1.6-1.1       jsonlite_1.8.7      
    ## [61] crosstalk_1.2.1      glue_1.6.2           nloptr_2.0.3        
    ## [64] codetools_0.2-19     DT_0.33              distributional_0.3.2
    ## [67] stringi_1.7.12       gtable_0.3.4         later_1.3.2         
    ## [70] QuickJSR_1.1.3       lme4_1.1-35.1        munsell_0.5.0       
    ## [73] tibble_3.2.1         colourpicker_1.3.0   pillar_1.9.0        
    ## [76] htmltools_0.5.8.1    R6_2.5.1             evaluate_0.21       
    ## [79] shiny_1.9.1          lattice_0.21-8       markdown_1.13       
    ## [82] backports_1.4.1      threejs_0.3.3        httpuv_1.6.13       
    ## [85] rstantools_2.4.0     gridExtra_2.3        nlme_3.1-162        
    ## [88] checkmate_2.2.0      xfun_0.40            fs_1.6.3            
    ## [91] zoo_1.8-12           pkgconfig_2.0.3
