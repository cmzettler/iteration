iteration
================

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.5     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(knitr)

knitr::opts_chunk$set(
  fig.width = 6, 
  fig.asp = .6, 
  out.width = "90%"
)
```

When to write funtions - if you have to use the code more than twice

Parts to a function:

-   arguments (inputs) these get used by the cody in the body (i.e., x
    in mean(x)) - na.rm = FALSE is the default, but you’ll sometimes was
    to change things

-   body (code that does stuff) starts with data/ input checks using
    conditional execution (check that whatever input you provide is
    allowed in the function that you created) , performs operations,
    format output

-   return objects (what the function produces) implicit - last value
    produced by the function or explicit - return a specific thing can
    return single values or a collection of values (he prefers returning
    dataframes) named or unnamed

SCOPING - will help prevent/ identify errors - if you have x in your
function, it’ll look in the function and then if its not there then itll
look in your environment. - this will be a problem if you have x in the
function and also x in the workspace, it can cross over if there are
issues

CONDITIONAL EXECUTION - going through some checks at the beginning of
the body of the function Code: if (condition\_1) {thing\_1} else if
(condition\_2) {thing\_2} else {thing\_3}

HOW TO WRITE FUNCTIONS - start with the smallest possible, least
complicated version - if it works, then add complexity

## Start coding

## Z scores

subtract mean and divide by the standard deviation

``` r
x_vec = rnorm(25, mean = 5, sd = 4) 

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.536758266  0.042045724 -0.855749766  0.764748851 -0.133760118
    ##  [6]  0.878902168 -0.361043796 -0.902950985 -0.736838782  1.381483621
    ## [11]  0.367803840 -2.060614053  0.717360795 -0.050196047  1.108827566
    ## [16]  0.978078281 -2.132055585 -0.002847095  0.426777017 -1.401872034
    ## [21]  1.933213055 -0.505638940  0.372273341 -0.521256307  0.156550983

Decide we want to do this to a lot of different vectors

``` r
z_scores = function(x) {
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}
```

Function of an x input, body of function within curly brackets

I want to use argument x THEN return z

Now try to use it

``` r
z_scores(x = x_vec)
```

    ##  [1]  0.536758266  0.042045724 -0.855749766  0.764748851 -0.133760118
    ##  [6]  0.878902168 -0.361043796 -0.902950985 -0.736838782  1.381483621
    ## [11]  0.367803840 -2.060614053  0.717360795 -0.050196047  1.108827566
    ## [16]  0.978078281 -2.132055585 -0.002847095  0.426777017 -1.401872034
    ## [21]  1.933213055 -0.505638940  0.372273341 -0.521256307  0.156550983

Looks like the same collection of z scores

``` r
y_vec = rnorm(40, mean = 12, sd = .3) 

z_scores(y_vec)
```

    ##  [1]  1.414128866 -1.736208320 -0.693750209 -0.003416589 -0.963457430
    ##  [6] -0.337437092  0.464325231 -0.201563611 -1.634743116  0.118489004
    ## [11] -0.411236019 -0.520866202 -0.823430665 -0.225730170 -0.596811384
    ## [16]  0.064360517 -0.808886514 -0.074062950  1.717695520  0.290057038
    ## [21]  0.626767200  0.322534411 -1.941988236  1.835790592  0.593754327
    ## [26]  0.448854486  2.227581360  0.625982143  1.017464402 -0.907186461
    ## [31] -1.038585610 -0.532561008 -0.072077118 -0.332702266  0.400849851
    ## [36] -0.347571543  2.231009598 -0.389971059 -0.898450867  1.093049893

How great is this!

``` r
z_scores(3)
```

    ## [1] NA

``` r
z_scores(c("my", "name", "is", "christie"))
```

    ## Warning in mean.default(x): argument is not numeric or logical: returning NA

    ## Error in x - mean(x): non-numeric argument to binary operator

``` r
mtcars
```

    ##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
    ## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
    ## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    ## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
    ## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
    ## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
    ## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
    ## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
    ## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
    ## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
    ## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
    ## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
    ## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    ## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
    ## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
    ## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
    ## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
    ## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
    ## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
    ## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
    ## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2

``` r
z_scores(mtcars)
```

    ## Warning in mean.default(x): argument is not numeric or logical: returning NA

    ## Error in is.data.frame(x): 'list' object cannot be coerced to type 'double'

NA is probably not what i want to have happen

ERROR

ERROR

The things that are going wrong here, my function shouuld work if I give
it a collection of numbers that I can take the mean of

Update z\_scores function to be better

``` r
z_scores = function(x) {
  
  if(!is.numeric(x)) {
    stop("x needs to be numeric")
  }
  if(length(x) < 3) {
    stop("x should have at least 3 numbers")
  }
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}
```

If something is going to break, it should break early and informatively

## Multiple outputs

If I give you something things, I want the mean and standard deviation

``` r
mean_and_sd = function(x) {
  
  if(!is.numeric(x)) {
    stop("x needs to be numeric")
  }
  if(length(x) < 3) {
    stop("x should have at least 3 numbers")
  }
  mean_x = mean(x) 
  sd_x = sd(x)
  
  output_df = 
    tibble(
      mean = mean_x, 
      sd = sd_x
    )
  
  return(output_df)
  
}
```

``` r
mean_and_sd(y_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  12.1 0.331

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.32  5.69

Defined x as y\_vec then you can work things through without doing the
full function just using the environment, but you have to be careful of
this

Every once in a while, you should restart your session or do rm(x) from
the environment to make sure things still work

## Different sample sizes, means, and sds

``` r
sim_data = 
  tibble(
  x = rnorm(30, mean = 2, sd = 3) 
  )

sim_data %>% 
  summarize(
    mean = mean(x), 
    sd = sd(x)
  )
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.36  3.05

Let’s write a function that simulates data, computes the mean and sd.

mu and sigma are the true things

allows multiple function inputs which map to different things in my code

everything should be self contained

``` r
sim_mean_sd = function (n, mu, sigma) {
  
  # do checks on inputs 
  
  sim_data = 
    tibble(
    x = rnorm(n, mean = mu, sd = sigma) 
    )
  
  sim_data %>% 
    summarize(
      mean = mean(x), 
      sd = sd(x)
    )
  
}

sim_mean_sd(30, 4, 3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.41  2.78
