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

    ##  [1] -0.94888646 -1.51293054  0.99777482 -1.76461067  0.87579555 -0.85554892
    ##  [7]  1.26446366 -1.51449073 -0.72548705 -0.72674208  1.84621045  0.83368493
    ## [13]  0.57110141  0.81125358  0.38298954 -0.06963830 -1.06597372  0.07227511
    ## [19] -0.58875600  0.30462310  0.97092935  0.52493559 -1.02804582  1.02973841
    ## [25]  0.31533480

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

    ##  [1] -0.94888646 -1.51293054  0.99777482 -1.76461067  0.87579555 -0.85554892
    ##  [7]  1.26446366 -1.51449073 -0.72548705 -0.72674208  1.84621045  0.83368493
    ## [13]  0.57110141  0.81125358  0.38298954 -0.06963830 -1.06597372  0.07227511
    ## [19] -0.58875600  0.30462310  0.97092935  0.52493559 -1.02804582  1.02973841
    ## [25]  0.31533480

Looks like the same collection of z scores

``` r
y_vec = rnorm(40, mean = 12, sd = .3) 

z_scores(y_vec)
```

    ##  [1]  0.08463627 -0.14786558  1.18454157 -1.93796392  0.37088159  3.34708301
    ##  [7]  0.21012443 -1.32973701 -0.14256904 -0.47608497  0.94359339 -1.58146960
    ## [13] -0.27099481  1.07761765 -0.01375457 -0.81682201 -0.59493532  0.61883909
    ## [19]  2.22477052 -0.23420282  0.84102258 -0.76206426  0.11782954 -0.03679170
    ## [25]  0.44994588 -0.40877155 -0.36880824 -0.31306612  0.93151283 -0.26362218
    ## [31] -0.80418365  0.50782506 -0.59632933  0.84134220  1.08398742 -0.40209547
    ## [37] -1.00349578 -1.02071346 -0.25293733 -1.05627430

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
    ## 1  12.0 0.269

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.49  4.23

Defined x as y\_vec then you can work things through without doing the
full function just using the environment, but you have to be careful of
this

Every once in a while, you should restart your session or do rm(x) from
the environment to make sure things still work
