writing\_functions
================

``` r
library(tidyverse) 
```

    ## ── Attaching packages ────────

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.1     ✓ dplyr   1.0.0
    ## ✓ tidyr   1.1.0     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ─────────────────
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rvest)
```

    ## Loading required package: xml2

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     pluck

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

x_vec
```

    ##  [1] 2.160353 1.151729 4.301953 4.620363 2.741472 2.078347 3.313877 9.735448
    ##  [9] 3.960227 7.772652 2.381796 6.454680 4.917067 8.007235 5.127892 5.847663
    ## [17] 8.454538 3.393861 4.441302 3.850104 0.464389 6.799170 6.520414 2.009592
    ## [25] 7.692052 4.525905 9.563025 3.777690 3.120805 6.250607

``` r
(x_vec - mean(x_vec))/sd(x_vec)
```

    ##  [1] -1.09299745 -1.50319846 -0.22202235 -0.09252717 -0.85666006 -1.12634856
    ##  [7] -0.62386638  1.98774524 -0.36099997  1.18948861 -1.00293787  0.65347799
    ## [13]  0.02814041  1.28489212  0.11388162  0.40660777  1.46680713 -0.59133772
    ## [19] -0.16535002 -0.40578663 -1.78273504  0.79357989  0.68021147 -1.15431089
    ## [25]  1.15670924 -0.13094275  1.91762182 -0.43523682 -0.70238782  0.57048263

``` r
#^ the Z score
```

I want a function to compute Z scores

``` r
z_score = function(x){
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("input must have 3 numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
}


z_score(x_vec)
```

    ##  [1] -1.09299745 -1.50319846 -0.22202235 -0.09252717 -0.85666006 -1.12634856
    ##  [7] -0.62386638  1.98774524 -0.36099997  1.18948861 -1.00293787  0.65347799
    ## [13]  0.02814041  1.28489212  0.11388162  0.40660777  1.46680713 -0.59133772
    ## [19] -0.16535002 -0.40578663 -1.78273504  0.79357989  0.68021147 -1.15431089
    ## [25]  1.15670924 -0.13094275  1.91762182 -0.43523682 -0.70238782  0.57048263

what my functions won’t work on - check error messages

``` r
z_score(3)
```

    ## Error in z_score(3): input must have 3 numbers

``` r
z_score("biny")
```

    ## Error in z_score("biny"): Input must be numeric

``` r
z_score(mtcars)
```

    ## Error in z_score(mtcars): Input must be numeric

``` r
z_score(c(T, T, F, T))
```

    ## Error in z_score(c(T, T, F, T)): Input must be numeric
