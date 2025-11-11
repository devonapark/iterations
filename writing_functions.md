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

Alternative Format

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

  list(mean = mean_x, 
       sd = sd_x
  )
  
}

mean_and_sd(x_vec)
```

    ## $mean
    ## [1] 10.66683
    ## 
    ## $sd
    ## [1] 3.196388

# Make up data….

Let’s *simulate* some data

``` r
sim_df = 
  tibble(
    x = rnorm(n = 300, mean = 5, sd = 2)
  )

sim_df |> 
  summarize(
    mu_hat = mean(x),
    sigma_hat = sd(x)
    
    )
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   5.06      2.02

What if i want to createa function that creates this dataframe rather
than running the code chunks indvidually multiple times

Write a function to do simulations (Originally we wrote this as code
chunk. Now, we moved code to a source file)

``` r
#now i import from source folder:
source("source/sim_mean_sd.R")

#source means execute entire file in backgorund
```

``` r
sim_mean_sd(n_subj = 30)
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   2.50      2.42

Let’s run this function

``` r
sim_mean_sd()
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   3.16      2.64

``` r
sim_mean_sd(n_subj = 3800)
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   2.99      2.08

``` r
sim_mean_sd(mu = 45)
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   44.9      2.19

``` r
#Let's say you dont define default value for n_subj
sim_mean_sd = function(n_subj, mu = 3, sigma = 2) { #these are default values if not defined when you actually run the function
  
              sim_df = 
              tibble(
                x = rnorm(n = n_subj, mean = mu, sd = sigma)
              )
          
            sim_df |> 
              summarize(
                mu_hat = mean(x),
                sigma_hat = sd(x)
                )
  
}

sim_mean_sd(50) #it will use 50 for the first non defined value 
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   3.34      1.56

``` r
sim_mean_sd(mu = 45, 50) #it will use 50 for the first non defined value 
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   45.0      1.86

Import LoTR data

``` r
#Warning lights should be going off!
fellowship_ring = 
readxl::read_excel("data/LotR_Words.xlsx", range = "B3:D6") |>
  mutate(movie = "fellowship_ring")

two_towers = readxl::read_excel("data/LotR_Words.xlsx", range = "F3:H6") |>
  mutate(movie = "two_towers")

return_king = readxl::read_excel("data/LotR_Words.xlsx", range = "J3:L6") |>
  mutate(movie = "return_king")

lotr_df = 
  bind_rows(fellowship_ring, two_towers, return_king) |>
  janitor::clean_names() |>
  pivot_longer(
    female:male,
    names_to = "sex",
    values_to = "words") |> 
  mutate(race = str_to_lower(race)) |> 
  select(movie, everything()) 
```

Try to import the datasets using a function!

``` r
#my version
import_lotr = function(path, cells, m) {
  
  df = 
    readxl::read_excel(path, range = cells) |>
    mutate(movie = m) 
  
  df
}
```

r = cells = “B3:D6” x = pathway =“data/LotR_Words.xlsx” m = title =
“fellowship_ring”

``` r
#jeff's version
import_lotr = function(cells, movie_title) {
  
  df = 
    readxl::read_excel("data/LotR_Words.xlsx", range = cells) |>
    mutate(movie = movie_title) 
  
  df
}

lotr_df = 
  bind_rows(
    fellowship = import_lotr(cells = "B3:D6", movie_title = "fellowship_ring"),
    two_towers = import_lotr(cells = "F3:H6", movie_title = "two_towers"),
    return_king = import_lotr(cells = "J3:L6", movie_title = "return_king")
  )
```

## Last Example

``` r
nsduh_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

nsduh_html = read_html(nsduh_url)

data_marj = 
  nsduh_html |> 
  html_table() |> 
  nth(1) |>                         #output only the first table
  slice(-1) |> 
  select(-contains("P Value")) |>
  pivot_longer(
    -State,                          #pivot everything except state
    names_to = "age_year", 
    values_to = "percent") |>
  separate(age_year, into = c("age", "year"), sep = "\\(") |>
  mutate(
    year = str_replace(year, "\\)", ""),
    percent = str_replace(percent, "[a-c]$", ""),
    percent = as.numeric(percent)) |>
  filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))
```

Let’s say you want to import multiple tables

Why dont we write an import function!!

``` r
nsduh_table = function(html, table_num, table_name) {
  
  table = 
    html |> #replaced with impput value from function 
    html_table() |> 
    nth(table_num) |> #replaced with impput value from function 
    slice(-1) |> 
    select(-contains("P Value")) |>
    pivot_longer(
      -State,
      names_to = "age_year", 
      values_to = "percent") |>
    separate(age_year, into = c("age", "year"), sep = "\\(") |>
    mutate(
      year = str_replace(year, "\\)", ""),
      percent = str_replace(percent, "[a-c]$", ""),
      percent = as.numeric(percent),
      name = table_name) |>                             #replaced with impput value from function 
    filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))
  
  table
  
}


nsduh_table(nsduh_html, table_num = 1, "Table 1")
```

    ## # A tibble: 510 × 5
    ##    State   age   year      percent name   
    ##    <chr>   <chr> <chr>       <dbl> <chr>  
    ##  1 Alabama 12+   2013-2014    9.98 Table 1
    ##  2 Alabama 12+   2014-2015    9.6  Table 1
    ##  3 Alabama 12-17 2013-2014    9.9  Table 1
    ##  4 Alabama 12-17 2014-2015    9.71 Table 1
    ##  5 Alabama 18-25 2013-2014   27.0  Table 1
    ##  6 Alabama 18-25 2014-2015   26.1  Table 1
    ##  7 Alabama 26+   2013-2014    7.1  Table 1
    ##  8 Alabama 26+   2014-2015    6.81 Table 1
    ##  9 Alabama 18+   2013-2014    9.99 Table 1
    ## 10 Alabama 18+   2014-2015    9.59 Table 1
    ## # ℹ 500 more rows

``` r
nsduh_table(nsduh_html, table_num = 2, "Table 2")
```

    ## # A tibble: 510 × 5
    ##    State   age   year      percent name   
    ##    <chr>   <chr> <chr>       <dbl> <chr>  
    ##  1 Alabama 12+   2013-2014    5.57 Table 2
    ##  2 Alabama 12+   2014-2015    5.35 Table 2
    ##  3 Alabama 12-17 2013-2014    4.98 Table 2
    ##  4 Alabama 12-17 2014-2015    5.16 Table 2
    ##  5 Alabama 18-25 2013-2014   15.0  Table 2
    ##  6 Alabama 18-25 2014-2015   14.3  Table 2
    ##  7 Alabama 26+   2013-2014    4.03 Table 2
    ##  8 Alabama 26+   2014-2015    3.86 Table 2
    ##  9 Alabama 18+   2013-2014    5.63 Table 2
    ## 10 Alabama 18+   2014-2015    5.37 Table 2
    ## # ℹ 500 more rows
