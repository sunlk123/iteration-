Iteration and List Columns
================
October 29, 2019

Creating a simple list

``` r
l = list(vec_numeric = 5:8,
         mat         = matrix(1:8, 2, 4),
         vec_logical = c(TRUE, FALSE),
         summary     = summary(rnorm(1000)))
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $vec_logical
    ## [1]  TRUE FALSE
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.00805 -0.69737 -0.03532 -0.01165  0.68843  3.81028

How to access things inside of lists

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l$summary
```

    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.00805 -0.69737 -0.03532 -0.01165  0.68843  3.81028

``` r
#use two [[]] to pull out second item in the list, i.e. the matrix
l[[2]]
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8

``` r
mean(l$vec_numeric)
```

    ## [1] 6.5

A dataframe is really just a list

``` r
df = list(
  a = rnorm(20, 3, 1),
  b = rnorm(20, 0, 5),
  c = rnorm(20, 10, .2),
  d = rnorm(20, -3, 1)
)

df$a
```

    ##  [1] 4.134965 4.111932 2.129222 3.210732 3.069396 1.337351 3.810840
    ##  [8] 1.087654 1.753247 3.998154 2.459127 2.783624 1.378063 1.549036
    ## [15] 3.350910 2.825453 2.408572 1.665973 1.902701 5.036104

``` r
df[[2]]
```

    ##  [1] -1.63244797  3.87002606  3.92503200  3.81623040  1.47404380
    ##  [6] -6.26177962 -5.04751876  3.75695597 -6.54176756  2.63770049
    ## [11] -2.66769787 -1.99188007 -3.94784725 -1.15070568  4.38592421
    ## [16]  2.26866589 -1.16232074  4.35002762  8.28001867 -0.03184464

Function from last class

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  tibble(
    mean = mean_x, 
    sd = sd_x
  )
}
```

Want to compute mean and sd of objects a, b, c and d from the list

``` r
mean_and_sd(df[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.70  1.12

``` r
mean_and_sd(df[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.416  4.08

``` r
mean_and_sd(df[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.191

``` r
mean_and_sd(df[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.43  1.18

We still have to copy and paste, and change the index of the dataframe;
we are going to try to iterate by writing our first for loop.

``` r
output = vector("list", length = 4)
```

Writing our first for loop

``` r
for (i in 1:4) {
  
  output[[i]] = mean_and_sd(df[[i]])
  
}

output
```

    ## [[1]]
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.70  1.12
    ## 
    ## [[2]]
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.416  4.08
    ## 
    ## [[3]]
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.191
    ## 
    ## [[4]]
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.43  1.18

``` r
# df is the input, for each line of input in the df, run the mean_and_sd function
output = map(df, mean_and_sd)

output_median = map(df, median)

output_summary = map(df, summary)

output_median = map_dbl(df, median)

#organizes output into dataframe 
output = map_dfr(df, mean_and_sd)
```
