writing_functions
================
2025-10-23

``` r
library(tidyverse)
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
set.seed(1)
```

## Start Small

The best way to build up a function is to start with code you’ve written
outside a function. Simple example: the chunk below takes a sample from
a normal distribution and then computes the vector of Z scores for the
sample.

``` r
x_vec = rnorm(20, mean = 10, sd = 3.5)

#computing z scores
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.894579096 -0.007534108 -1.123622566  1.538189109  0.152185414
    ##  [6] -1.107022329  0.325106999  0.599834216  0.421851526 -0.543016965
    ## [11]  1.446758183  0.218251901 -0.888870682 -2.633686248  1.023162590
    ## [16] -0.257822640 -0.226349080  0.824866429  0.690604708  0.441692638

Write a function to compute z scores

``` r
#x is a list of numbers
z_scores = function(x) {
 
   z = (x - mean(x)) / sd(x) #compute for x and then call it z
  
   z #often the last line of the function is the output 
}

#z is not being created and stored in the environment. it only exists within this function and when this function is running. 
```

Let’s try our function …

``` r
z_scores( x = x_vec)
```

    ##  [1] -0.894579096 -0.007534108 -1.123622566  1.538189109  0.152185414
    ##  [6] -1.107022329  0.325106999  0.599834216  0.421851526 -0.543016965
    ## [11]  1.446758183  0.218251901 -0.888870682 -2.633686248  1.023162590
    ## [16] -0.257822640 -0.226349080  0.824866429  0.690604708  0.441692638

``` r
num_vec = rnorm(123, mean = 14, sd = 0.4)

z_scores(x = num_vec)
```

    ##   [1]  1.01992219  0.86446496  0.06063410 -2.28406230  0.68007333 -0.08783948
    ##   [7] -0.20106513 -1.69491160 -0.56727349  0.45072438  1.51944241 -0.14084606
    ##  [13]  0.41633644 -0.08519966 -1.58847260 -0.49552619 -0.47200488 -0.09145738
    ##  [19]  1.22560036  0.84292498 -0.21098061 -0.31190442  0.76770493  0.60831801
    ##  [25] -0.80653038 -0.82781920  0.39010562  0.84901095 -0.15170490  0.97690068
    ##  [31]  0.42819021 -0.71936266  0.36345149 -1.30707897  1.60390050  2.22574295
    ##  [37] -0.44125395 -1.21025584  0.62315066 -0.17750259  2.70426421 -0.06865316
    ##  [43]  0.75949814  0.00773677 -0.86846458  0.19040115 -2.07458399  1.64085727
    ##  [49]  0.15002739  2.44410363  0.51612402 -0.83060395  0.66973603 -1.08524917
    ##  [55] -1.44825528  0.30702035 -0.52767313 -0.02281914  0.06038002 -0.69379552
    ##  [61] -0.67010653 -0.17764347  1.31428165 -1.75491101  0.65067306  0.35417080
    ##  [67]  1.18365139 -0.36964063  0.39628211  0.27936063 -0.64040050  1.34811391
    ##  [73]  1.29419145  0.77139738  1.77863487  0.61038927 -1.47433746 -0.67532855
    ##  [79] -1.41528645 -0.56187795 -0.72883757  0.02377054 -1.05892028  0.15545249
    ##  [85] -0.76771063  1.98363801  0.79013506  1.00992145  0.41237591  1.88694811
    ##  [91] -0.74629829 -0.54852275  1.60305817 -0.76329337 -0.25966814 -0.47032124
    ##  [97] -0.38760026 -0.34115935  0.53734392 -0.22552977 -0.59886388  1.50167389
    ## [103] -0.26784612 -0.22805865 -0.13789577  0.78554412 -0.10764713 -0.06682887
    ## [109] -0.79846992 -0.39245957  0.04426993 -0.69308384  0.57972723 -1.74903458
    ## [115]  0.32418780 -1.76954667 -0.36599644 -0.62422312 -0.76488204 -0.08871200
    ## [121] -2.19886791  1.31257340 -1.91555378

Let’s break our function

``` r
z_scores(3)
```

    ## [1] NA

``` r
#breaks because you need at least a couple values to calculate SD

z_scores("my name is jeff")
```

    ## Error in x - mean(x): non-numeric argument to binary operator

``` r
#breaks becuase you need to use a number to calculate a mean 
```

Update function to let people know when it is breaking…

``` r
z_scores = function(x) {
 
  if (!is.numeric(x)) {
     stop("The input x should be numeric")
  }
  
  if (length(x) < 5) {
     stop("only compute scores where x > 5")
  }
  
  z = (x - mean(x)) / sd(x) #compute for x and then call it z
  
  z #often the last line of the function is the output 
}
```

## let’s compute some stuff

What if we want to return multiple values (mean and sd of a numeric
vector)

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } 
  
  else if (length(x) < 5) {
    stop("Only compute mean and sd when the input has 5 or more numbers")
  }
  
  mean_x = mean(x, na.rm = TRUE)
  sd_x = sd(x, na.rm = TRUE)

  c(mean_x, sd_x)
}

mean_and_sd(x_vec)
```

    ## [1] 10.666834  3.196388

A better output format would be a df

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } 
  
  else if (length(x) < 5) {
    stop("Only compute mean and sd when the input has 5 or more numbers")
  }
  
  mean_x = mean(x, na.rm = TRUE)
  sd_x = sd(x, na.rm = TRUE)

  tibble(
  mean = mean_x, 
       sd = sd_x
  )
  
}

mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.7  3.20
