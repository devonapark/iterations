Iterations and List Columns
================
2025-11-06

#### Always add at the beginning

#### Load key packages.

``` r
library(tidyverse)
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

|  |
|:---|
| UNIMPORTANT |
| When “observations” that I want to store become more complex than single values (for example, each row many contain a few scalar observations as well a complete data set) For this we can use list columns are an appropriate column type. Map functions provide a way to interact with those columns. |
| GOAL: Use map functions and iterate over listcolumns in data frames. |
| What are `LISTS`? - Note: vectors are limited to a single data class (aka they can only be ALL characters, numeric or all logic) |
| \`\`\` r \#Trying to join the following vectors will result in coersion, as would creating vectors of mixed types. |
| vec_numeric = 5:8 vec_char = c(“My”, “name”, “is”, “Jeff”) vec_logical = c(TRUE, TRUE, TRUE, FALSE) \`\`\` |
| ^If we want to combine these, we would use a list. |
| Lists contain indexed elements. Indexed elements themselves can be scalars, vectors, or other things entirely. |

## Make a list

``` r
l = 
  list(
    vec_numeric = 1:23,
    char_vec = c("Jeff"), #already with these first two lines you could not put this in a DF because vec_numeric has length 23 and char_vec has length 1. 
    mat = matrix(1:8, nrow = 2, ncol = 4), 
    summary = summary(rnorm(1000, mean = 4)) #if you give me a sample (normally distributed) of 1000 observations with mean 4, this will output min, max, median and 1st and 3rd quartile
  )

l
```

    ## $vec_numeric
    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
    ## 
    ## $char_vec
    ## [1] "Jeff"
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   1.232   3.255   3.961   3.969   4.655   7.165

``` r
#Lists can be accessed using names or indices, and the things in lists can be accessed in the way you would usually access an object of that type.

#all get the vec_numeric portion
l$vec_numeric #dollar sign extracts things from a list (takes it out of a list) (HE GENERALLY AVOIDS) (leave it in a list if youve put it in a list)
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23

``` r
l[[1]] ## of variable in list
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23

``` r
l[["vec_numeric"]]
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23

``` r
#gets the first three observations of variable in list
l[[1]][1:3]
```

    ## [1] 1 2 3

``` r
l[[2]]
```

    ## [1] "Jeff"

``` r
l$mat
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8

## Make a different list

`for` loops

``` r
list_normals = 
  list(
    a = rnorm(30, mean = 3, sd = 1),
    b = rnorm(30, mean = 30, sd = 1),
    c = rnorm(30, mean = 3, sd = 10),
    d = rnorm(30, mean = -3, sd = 4)
  )

#check that it is a list
is.list(list_normals)
```

    ## [1] TRUE

### A couple ways to use mean_and_sd function from last time

Path 1: copy and paste the function from last time

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
```

Path 2:get it this way from source folder and script

``` r
#CHECK HOW THIS WORKS

source("source/mean_and_sd.R")
```

### Apply the mean_and_sd function to each element of list_norms using the lines below

``` r
mean_and_sd(list_normals[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.00 0.854

``` r
mean_and_sd(list_normals[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  29.8 0.773

``` r
mean_and_sd(list_normals[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.19  10.5

``` r
mean_and_sd(list_normals[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.76  4.06

However, now we are doing this multiple time, and if we wanted to save
these results it would get even more tedious, so…..

### Use a loop to iterate!!

``` r
output = vector("list", length = 4) 

#output is a vector of type list with length equal to 4 becuase we have 4 inputs (and subsequently 4 outputs) --> this is a way to define an empty list with four blank spots

#now create the for loop -- iterating from 1 to 4
#first time i is going to be 1 then 2 and so on until 4
for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_normals[[i]])
  
}

#list_normals[[i]] --> i=1 means a and so on

output
```

    ## [[1]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.00 0.854
    ## 
    ## [[2]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  29.8 0.773
    ## 
    ## [[3]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.19  10.5
    ## 
    ## [[4]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.76  4.06

^In this example, Jeff bypassed a common first step in writing loops
because he already had the function he wanted to repeat. Frequently,
however, he’ll start with repeated code segements, then abstract the
underlying process into a function, and then wrap things up in a for
loop.

# Use `map` to do the same thing.

`MAP` A criticism of for loops is that there’s a lot of overhead – you
have to define your output vector / list, there’s the for loop
bookkeeping to do, etc – that distracts from the purpose of the code. In
this case, we want to apply mean_and_sd to each element of list_norms,
but we have to scan inside the for loop to figure that out.

The map functions in purrr try to make the purpose of your code clear.
Compare the loop above to the line below.

``` r
output = map(list_normals, mean_and_sd)

# map says, "tell me the input list and what function you want me to apply to it each time"

output = map(list_normals, median)
```

### Check out some `map` variants

`MAP` focuses on the fucntion we want to apply by removing the
bookkeeping

``` r
#collapses this into a dataframe
map_dfr(list_normals, mean_and_sd)
```

    ## # A tibble: 4 × 2
    ##    mean     sd
    ##   <dbl>  <dbl>
    ## 1  3.00  0.854
    ## 2 29.8   0.773
    ## 3  5.19 10.5  
    ## 4 -2.76  4.06

``` r
#input list is named a,b,c,d. If i want to keep that ID varaible, i can include the argument ".id = "sample"" 
map_dfr(list_normals, mean_and_sd, .id = "sample")
```

    ## # A tibble: 4 × 3
    ##   sample  mean     sd
    ##   <chr>  <dbl>  <dbl>
    ## 1 a       3.00  0.854
    ## 2 b      29.8   0.773
    ## 3 c       5.19 10.5  
    ## 4 d      -2.76  4.06

``` r
#if we know our result is going to be a single number
#simplifies the results in a predictable way (not as a list)
map_dbl(list_normals, median) #we can define our output whichever way we want. 
```

    ##         a         b         c         d 
    ##  3.088785 29.752766  5.011984 -3.026142

``` r
#^ you dont need to set the length of your output becuase map will utomatically set it 


#The first argument to map is the list (or vector, or data frame) we want to iterate over, and the second argument is the function we want to apply to each element. The line above will produce the same output as the previous loop, but is clearer and easier to understand (once you’re used to map …).

#This code (using map) is why we pointed out in writing functions that functions can be passed as arguments to other functions. The second argument in map(list_norms, mean_and_sd) is a function we just wrote. To see how powerful this can be, suppose we wanted to apply a different function, say median, to each column of list_norms. The chunk below includes both the loop and the map approach.
```

## LIST COLUMNS

I have list_normals (but I prefer dataframes) so, let’s try to put my
list into a dataframe!!

``` r
#define dataframe 

listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    sample = list_normals
  )

#the reason this works is because "name" and "sample" BOTH have length 4. As long as they have the same length i can put them next to eachother in the same dataframe and call them whatever i want 

#so we have name (type character) with values a-d and sample (type dbl) with each cell having a list comprised of 30 numbers
```

### Did this really work??

``` r
pull(listcol_df, name)
```

    ## [1] "a" "b" "c" "d"

``` r
pull(listcol_df, sample)
```

    ## $a
    ##  [1] 2.962243 2.695585 4.000893 2.427436 2.848314 2.201049 3.052992 3.703348
    ##  [9] 4.307738 3.759010 1.117580 3.218375 2.449452 3.347139 2.834131 3.376006
    ## [17] 2.927198 1.227492 2.501071 3.573369 3.483637 3.584882 3.124578 3.563391
    ## [25] 4.371748 3.803289 1.189559 2.266123 2.293079 3.776913
    ## 
    ## $b
    ##  [1] 29.11678 30.25807 29.72752 29.77801 30.10652 29.95935 30.50731 29.61680
    ##  [9] 30.34357 28.54262 29.41455 29.71769 29.55586 28.32725 29.04593 30.72194
    ## [17] 29.50808 28.65607 30.00308 30.26586 29.41340 31.21069 29.43912 28.60292
    ## [25] 31.65798 30.28256 29.24831 30.27832 30.54203 30.23862
    ## 
    ## $c
    ##  [1]  21.128210   2.911274   8.895060  25.266525  20.177932  -7.954563
    ##  [7]   7.503893  13.304281  -4.541365   5.683536  -6.254584  13.951345
    ## [13]   8.958830  -3.539322 -17.303913   9.511732  12.728472   1.381342
    ## [19]   2.029896  21.544146  -8.676226   4.340432  16.410133  14.089019
    ## [25]  -8.861595   2.554036  -5.409085   6.838490   2.034510  -2.919120
    ## 
    ## $d
    ##  [1]  -2.88022646   7.35067783   1.15717579   0.89228091  -6.45759348
    ##  [6]  -7.64902053  -5.23267319  -4.22668421  -4.34597794  -2.00633104
    ## [11]  -4.19256446  -6.15876402   1.95842070   3.49292835  -0.47847471
    ## [16]  -3.17205778  -1.30624284  -7.75913071  -5.34112823  -2.10906136
    ## [21]  -6.62332344  -4.70983870   2.65029543   0.48160382  -1.28285992
    ## [26]  -5.89549525  -0.04376089 -12.12646606  -6.43222515  -0.40561299

### can I apply `mean_and_sd`??

``` r
pull(listcol_df, sample[[1]]) #gets all the numbers in row 1 or from a
```

    ## $a
    ##  [1] 2.962243 2.695585 4.000893 2.427436 2.848314 2.201049 3.052992 3.703348
    ##  [9] 4.307738 3.759010 1.117580 3.218375 2.449452 3.347139 2.834131 3.376006
    ## [17] 2.927198 1.227492 2.501071 3.573369 3.483637 3.584882 3.124578 3.563391
    ## [25] 4.371748 3.803289 1.189559 2.266123 2.293079 3.776913
    ## 
    ## $b
    ##  [1] 29.11678 30.25807 29.72752 29.77801 30.10652 29.95935 30.50731 29.61680
    ##  [9] 30.34357 28.54262 29.41455 29.71769 29.55586 28.32725 29.04593 30.72194
    ## [17] 29.50808 28.65607 30.00308 30.26586 29.41340 31.21069 29.43912 28.60292
    ## [25] 31.65798 30.28256 29.24831 30.27832 30.54203 30.23862
    ## 
    ## $c
    ##  [1]  21.128210   2.911274   8.895060  25.266525  20.177932  -7.954563
    ##  [7]   7.503893  13.304281  -4.541365   5.683536  -6.254584  13.951345
    ## [13]   8.958830  -3.539322 -17.303913   9.511732  12.728472   1.381342
    ## [19]   2.029896  21.544146  -8.676226   4.340432  16.410133  14.089019
    ## [25]  -8.861595   2.554036  -5.409085   6.838490   2.034510  -2.919120
    ## 
    ## $d
    ##  [1]  -2.88022646   7.35067783   1.15717579   0.89228091  -6.45759348
    ##  [6]  -7.64902053  -5.23267319  -4.22668421  -4.34597794  -2.00633104
    ## [11]  -4.19256446  -6.15876402   1.95842070   3.49292835  -0.47847471
    ## [16]  -3.17205778  -1.30624284  -7.75913071  -5.34112823  -2.10906136
    ## [21]  -6.62332344  -4.70983870   2.65029543   0.48160382  -1.28285992
    ## [26]  -5.89549525  -0.04376089 -12.12646606  -6.43222515  -0.40561299

``` r
#then pull for mean_and_sd function
mean_and_sd(pull(listcol_df, sample)[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.00 0.854

``` r
mean_and_sd(pull(listcol_df, sample)[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  29.8 0.773

``` r
mean_and_sd(pull(listcol_df, sample)[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.19  10.5

``` r
mean_and_sd(pull(listcol_df, sample)[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.76  4.06

### iterate using `map`

``` r
map(pull(listcol_df, sample), mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.00 0.854
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  29.8 0.773
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.19  10.5
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.76  4.06

``` r
#previously i had mapped across list_normals using mean_and_sd, but now it is a list in my df

#pull(listcol_df, sample) --> accesses the input list
  #so i call on the dataframe and the specify "saple" becuase this is the     colmn in the df that houses my list 
```

### adding a column …

Ideally I want the output I just got to be in the dataframe that I just
iterated over. So, I want to add a column to it.

(aka the values i just got (mean and sd), i want to put into listcol_df)

``` r
#like when we want to add any columns, use MUTATE

listcol_df = 
  listcol_df |> 
  mutate(
    summary = map(sample, mean_and_sd)  #map across the samples and apply this function 
  )



listcol_df |> pull(summary)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.00 0.854
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  29.8 0.773
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.19  10.5
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.76  4.06

``` r
pull(listcol_df, summary)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.00 0.854
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  29.8 0.773
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.19  10.5
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.76  4.06

``` r
#^both do the same thing
```

``` r
listcol_df |> 
  select(-sample) |>  # I may not care about the sample anymore, so i can remove this
  unnest(summary) 
```

    ## # A tibble: 4 × 3
    ##   name   mean     sd
    ##   <chr> <dbl>  <dbl>
    ## 1 a      3.00  0.854
    ## 2 b     29.8   0.773
    ## 3 c      5.19 10.5  
    ## 4 d     -2.76  4.06

``` r
#unnest allows me to unnest a particular column (takes the 1x2 tibble from mean_and_sd function output that is now in summary and makes it two columns)

#it takes the name of columns that were originally in the tibble

#technically you can unnest the same column as well 
```

# Let’s look at a couple more examples

## Revisit NSDUH

``` r
#Import data from online 
nsduh_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

#read in HTML
nsduh_html = read_html(nsduh_url)

#Function to extract certain table doing the following (specifics to data cleaning process is in previous class "writing_functions)
nsduh_import = function(html, table_num) {
  
  data = 
    html |> 
    html_table() |> 
    nth(table_num) |>
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
      percent = as.numeric(percent)) |>
    filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))
  
  data
  
}

nsduh_import(nsduh_html, table_num = 1)
```

    ## # A tibble: 510 × 4
    ##    State   age   year      percent
    ##    <chr>   <chr> <chr>       <dbl>
    ##  1 Alabama 12+   2013-2014    9.98
    ##  2 Alabama 12+   2014-2015    9.6 
    ##  3 Alabama 12-17 2013-2014    9.9 
    ##  4 Alabama 12-17 2014-2015    9.71
    ##  5 Alabama 18-25 2013-2014   27.0 
    ##  6 Alabama 18-25 2014-2015   26.1 
    ##  7 Alabama 26+   2013-2014    7.1 
    ##  8 Alabama 26+   2014-2015    6.81
    ##  9 Alabama 18+   2013-2014    9.99
    ## 10 Alabama 18+   2014-2015    9.59
    ## # ℹ 500 more rows

``` r
nsduh_import(nsduh_html, table_num = 2)
```

    ## # A tibble: 510 × 4
    ##    State   age   year      percent
    ##    <chr>   <chr> <chr>       <dbl>
    ##  1 Alabama 12+   2013-2014    5.57
    ##  2 Alabama 12+   2014-2015    5.35
    ##  3 Alabama 12-17 2013-2014    4.98
    ##  4 Alabama 12-17 2014-2015    5.16
    ##  5 Alabama 18-25 2013-2014   15.0 
    ##  6 Alabama 18-25 2014-2015   14.3 
    ##  7 Alabama 26+   2013-2014    4.03
    ##  8 Alabama 26+   2014-2015    3.86
    ##  9 Alabama 18+   2013-2014    5.63
    ## 10 Alabama 18+   2014-2015    5.37
    ## # ℹ 500 more rows

``` r
nsduh_import(nsduh_html, table_num = 3)
```

    ## # A tibble: 510 × 4
    ##    State   age   year      percent
    ##    <chr>   <chr> <chr>       <dbl>
    ##  1 Alabama 12+   2013-2014    1.42
    ##  2 Alabama 12+   2014-2015    1.49
    ##  3 Alabama 12-17 2013-2014    4.46
    ##  4 Alabama 12-17 2014-2015    4.36
    ##  5 Alabama 18-25 2013-2014    6.04
    ##  6 Alabama 18-25 2014-2015    6.39
    ##  7 Alabama 26+   2013-2014    0.15
    ##  8 Alabama 26+   2014-2015    0.2 
    ##  9 Alabama 18+   2013-2014    0.95
    ## 10 Alabama 18+   2014-2015    1.05
    ## # ℹ 500 more rows

### Try this with a `for` loop

``` r
#define blank list first with length three
output = vector("list", length = 3) 


for (i in 1:3) {
  
  output[[i]] = nsduh_import(html = nsduh_html, i)
  
}
```

### Do this with `map`

``` r
map(1:3, nsduh_import, html = nsduh_html)
```

    ## [[1]]
    ## # A tibble: 510 × 4
    ##    State   age   year      percent
    ##    <chr>   <chr> <chr>       <dbl>
    ##  1 Alabama 12+   2013-2014    9.98
    ##  2 Alabama 12+   2014-2015    9.6 
    ##  3 Alabama 12-17 2013-2014    9.9 
    ##  4 Alabama 12-17 2014-2015    9.71
    ##  5 Alabama 18-25 2013-2014   27.0 
    ##  6 Alabama 18-25 2014-2015   26.1 
    ##  7 Alabama 26+   2013-2014    7.1 
    ##  8 Alabama 26+   2014-2015    6.81
    ##  9 Alabama 18+   2013-2014    9.99
    ## 10 Alabama 18+   2014-2015    9.59
    ## # ℹ 500 more rows
    ## 
    ## [[2]]
    ## # A tibble: 510 × 4
    ##    State   age   year      percent
    ##    <chr>   <chr> <chr>       <dbl>
    ##  1 Alabama 12+   2013-2014    5.57
    ##  2 Alabama 12+   2014-2015    5.35
    ##  3 Alabama 12-17 2013-2014    4.98
    ##  4 Alabama 12-17 2014-2015    5.16
    ##  5 Alabama 18-25 2013-2014   15.0 
    ##  6 Alabama 18-25 2014-2015   14.3 
    ##  7 Alabama 26+   2013-2014    4.03
    ##  8 Alabama 26+   2014-2015    3.86
    ##  9 Alabama 18+   2013-2014    5.63
    ## 10 Alabama 18+   2014-2015    5.37
    ## # ℹ 500 more rows
    ## 
    ## [[3]]
    ## # A tibble: 510 × 4
    ##    State   age   year      percent
    ##    <chr>   <chr> <chr>       <dbl>
    ##  1 Alabama 12+   2013-2014    1.42
    ##  2 Alabama 12+   2014-2015    1.49
    ##  3 Alabama 12-17 2013-2014    4.46
    ##  4 Alabama 12-17 2014-2015    4.36
    ##  5 Alabama 18-25 2013-2014    6.04
    ##  6 Alabama 18-25 2014-2015    6.39
    ##  7 Alabama 26+   2013-2014    0.15
    ##  8 Alabama 26+   2014-2015    0.2 
    ##  9 Alabama 18+   2013-2014    0.95
    ## 10 Alabama 18+   2014-2015    1.05
    ## # ℹ 500 more rows

``` r
#input that i care about is just 1:3
#nsduh_import has two arguments (html and i), and in MAP, whatever things are not changing (fixed everywhere) are written after everything else 



#i is 1:3 in this map function
```

### Do this all in a dataframe.

We dont want import and output vectors just floating around, so we are
gonna tidy this up by putting it all in a dataframe

``` r
nsduh_df = 
  tibble(
    name = c("marj year", "marj month", "marj first"),
    number = 1:3
  ) |> 
  
  #creates a quick data frame with just name of table we will eventually     want to pull and then the number of the datatable 
  
  #eventually we want to add column with a list of the imported and tidy     datasets 
  
  mutate(
    table = map(number, nsduh_import, html = nsduh_html) 
    
    #map across table number that i am interested in (number column is         defined in section above) apply nsduh_import function which takes two      arguments html (which is not changing) and table number (which is          defined as number)
    
  ) |> 
  unnest(table)
```

## Look at weather data

``` r
library(p8105.datasets)
data("weather_df")
```

``` r
weather_df |> 
  filter(name == "CentralPark_NY") |> 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point() + 
  geom_smooth(method = "lm") #regression line that goes through the middle of this 
```

    ## `geom_smooth()` using formula = 'y ~ x'

<img src="iteration_and_listcols_files/figure-gfm/unnamed-chunk-23-1.png" width="90%" />

Let’s do a regression

``` r
weather_df |> 
  filter(name == "CentralPark_NY") |> 
  lm(tmax ~ tmin, data = _) #lm means linear model
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = filter(weather_df, name == "CentralPark_NY"))
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.514        1.034

``` r
  #data = _ becuase dataframe does not go first, so we need to use a place holder, "_"

weather_df |> 
  filter(name == "Molokai_HI") |> 
  lm(tmax ~ tmin, data = _)
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = filter(weather_df, name == "Molokai_HI"))
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     21.7547       0.3222

``` r
weather_df |> 
  filter(name == "Waterhole_WA") |> 
  lm(tmax ~ tmin, data = _)
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = filter(weather_df, name == "Waterhole_WA"))
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.532        1.137

### Let’s iterate differently…

(imagine if you had 100 weather stations)

``` r
weather_nest = 
  weather_df |> 
  nest(data = date:tmin)

#nest: into the data column I want to colapse everything from data to tmin 
```

``` r
lm(tmax ~ tmin, data = pull(weather_nest, data)[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = pull(weather_nest, data)[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.514        1.034

``` r
#run linear model of tmax against tmin, where the dataset is going to be taken out of the data column of weather_nest for only the first row 

#pull out data column from weather_nest and only take the first element (central park)

lm(tmax ~ tmin, data = pull(weather_nest, data)[[2]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = pull(weather_nest, data)[[2]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     21.7547       0.3222

``` r
lm(tmax ~ tmin, data = pull(weather_nest, data)[[3]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = pull(weather_nest, data)[[3]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.532        1.137

### Do this using `map` ..

``` r
#first step, create a new function
#function of a dataframe that runs a linear model of tmax against tmin using some input dataframe

weather_lm = function(df) {
  
  lm(tmax ~ tmin, data = df)
  
}
```

``` r
map(pull(weather_nest, data), weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.514        1.034  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     21.7547       0.3222  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.532        1.137

``` r
#pull(weather_nest, data) is my input list
#weather_lm is the function i want to run on my input list
```

``` r
weather_nest |> 
  mutate(
    lm_fits = map(data, weather_lm) #map across the data column using my lm function 
  ) 
```

    ## # A tibble: 3 × 4
    ##   name           id          data               lm_fits
    ##   <chr>          <chr>       <list>             <list> 
    ## 1 CentralPark_NY USW00094728 <tibble [730 × 4]> <lm>   
    ## 2 Molokai_HI     USW00022534 <tibble [730 × 4]> <lm>   
    ## 3 Waterhole_WA   USS0023B17S <tibble [730 × 4]> <lm>

# How can we edit lm_fits so we can put it into our dataframe (unnest it)

In someway this is an extreme version of groupby and summarize
