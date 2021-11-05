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

    ##  [1] -0.81533377  0.05313278 -1.03185651  0.48654477  1.53981479 -1.58509684
    ##  [7]  0.15994358  1.05011722 -1.65115031 -0.29054733  0.93571001 -0.53919176
    ## [13] -0.82134243  0.63171568  0.67352938  1.09496648 -0.20405276  0.58156450
    ## [19] -0.35096674  0.52769315  1.22153603 -1.53798843  1.09499358 -1.77998159
    ## [25]  0.55624652

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

    ##  [1] -0.81533377  0.05313278 -1.03185651  0.48654477  1.53981479 -1.58509684
    ##  [7]  0.15994358  1.05011722 -1.65115031 -0.29054733  0.93571001 -0.53919176
    ## [13] -0.82134243  0.63171568  0.67352938  1.09496648 -0.20405276  0.58156450
    ## [19] -0.35096674  0.52769315  1.22153603 -1.53798843  1.09499358 -1.77998159
    ## [25]  0.55624652

Looks like the same collection of z scores

``` r
y_vec = rnorm(40, mean = 12, sd = .3) 

z_scores(y_vec)
```

    ##  [1] -0.0431465259 -0.7193211774 -0.0149810520 -0.1953405024 -0.0139062276
    ##  [6] -0.6179059867 -1.4421715333 -1.5432660583 -0.6779323796  1.7001007187
    ## [11] -1.0845093244  0.1378674217  1.3553757364  0.0623859334  0.6276014985
    ## [16]  0.2325065528 -2.2125801059 -0.7104951961 -0.3193633421  0.6559647915
    ## [21] -0.0596347475  0.6838278101  0.7612652848  1.7917048686  0.6363379639
    ## [26] -0.4266382814 -0.0334676115 -0.3190850057  0.0159624816 -0.0006033037
    ## [31]  2.1277498248 -2.3933729395  1.2646794020 -0.5982924469  0.1362925131
    ## [36]  1.2206240572  0.0088753761  1.1996624954 -0.5287823765 -0.6639886064

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
