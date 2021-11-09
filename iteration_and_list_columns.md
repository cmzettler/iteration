iteration and list columns
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
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
knitr::opts_chunk$set(
  fig.width = 6, 
  fig.asp = .6, 
  out.width = "90%"
)
```

iteration = doing the same thing over and over - doing the same thing
with the same function over and over –&gt; iterating

for loops - loops are the easiest place to start - loops have an output
object, a sequence to iterate over, the loop body, and (optionally) an
input object - basic structure:

input = list(*) output = list(*)

for (i in 1:n) { output\[\[i\]\] = f(input\[\[i\]\])

}

Loop functions - loop process (supply input vector/ list; apply a
function to each element; save the result to a vector/ list) is really
common - for loops can get tedious/ opaque - loop functions are popular
for cleaning up loops - we focus on purr::map() - base R has lapply()
and similar things - DONT USE THESE

map - goal of map is to clarify the loop process - basic structure:
(input is the list I need to iterate over) ouput = map(input, f) -
benefit is clarity

map variants - map takes one input and returns a list (by default) - you
can use specific variants to prevent errors (know the form that you will
output) map\_dbl, map\_lgl, map\_ df - if you need to iterate over 2
inputs - you can use map variants to give 2 inputs

Process for iteration: - write a single example, embed example into a
loop, abstract loop body to a function, re-write using a map statement -
make sure something works and then try to iterate however feels most
natural

LISTS: - dataframes provide a nice data rectangle (UNIFORM) = special
list - ouputs that are less uniform/ nicely structured = LIST - you can
put anything in the list (including a list) - list columns –&gt; df

## Lists

``` r
l = list(
  vec_numeric = 5:8, 
  vec_logical = c(TRUE, FALSE), 
  summary = summary(rnorm(1000, mean = 5, sd = 3))
)

l[[3]]
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  -4.301   3.093   4.906   4.930   6.831  13.636

``` r
l[["summary"]]
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  -4.301   3.093   4.906   4.930   6.831  13.636

``` r
l$summary
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  -4.301   3.093   4.906   4.930   6.831  13.636

Looks nothing like a dataframe

## list of normals

``` r
list_norms = 
  list(
    a = rnorm(50, mean = 2, sd = 1), 
    b = rnorm(50, mean = 5, sd = 3), 
    c = rnorm(50, mean = 20, sd = 1.2), 
    d = rnorm(50, mean = -12, sd = 0.5) 
  )
```

what if we want to take the mean and sd of this list?

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

mean_and_sd(list_norms[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.96  1.12

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.24  2.87

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  19.9  1.15

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -12.1 0.567

Why use numbers instead of letters (use element \#x instead of
remembering the names each time)

## for loop

Let’s use a for loop to iterate over my list of normals.

``` r
output = vector("list", length = 4)

for(i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norms[[i]])
  
}
```

let’s use map instead …

``` r
output = map(list_norms, mean_and_sd) 

output = map(list_norms, median) 

output = map(list_norms, summary) 

output = map(list_norms, IQR) 
```

default outputs as a list

``` r
output = map_dbl(list_norms, median)
```

## LIST COLUMNS!!

``` r
list_col_df = 
  tibble(
    name = c("a", "b", "c", "d"), 
    norms = list_norms
  )
```

it has lists in one of the columns but we can do normal things to
dataframes

``` r
list_col_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 × 2
    ##   name  norms       
    ##   <chr> <named list>
    ## 1 a     <dbl [50]>

``` r
list_col_df %>% 
  pull(name) 
```

    ## [1] "a" "b" "c" "d"

``` r
list_col_df %>% 
  pull(norms) 
```

    ## $a
    ##  [1]  3.9705016  2.5252311  2.1458872  2.2519902  2.6618673  2.9182781
    ##  [7]  2.4629941  1.1296093  0.5903122  0.2628728  0.5247520  1.9964688
    ## [13]  3.2801921  1.6188002  1.9601045  3.4723155  2.1959557  0.4355779
    ## [19]  2.7692951  2.7540386  0.3748014  1.2290479  2.3028134  2.3706756
    ## [25]  2.5745317  1.3519972  0.8332585  0.2932868  2.0620608  3.5402373
    ## [31]  0.2758835  2.9086491  2.8093401  0.0373101  2.5533016  3.0940533
    ## [37]  3.4537862  2.0989652  3.9301051  1.7877567  0.6928137  1.7127018
    ## [43]  1.1271763  0.9223215 -0.2205157  2.6312269  3.0446265  2.1363165
    ## [49]  3.5087756  0.6817882
    ## 
    ## $b
    ##  [1]  5.7082275  5.8790288  6.1358043  7.0987836  5.1376414 -1.4373817
    ##  [7]  5.9582640  5.1056678  4.9423460  7.4981582  9.0785406  2.4343803
    ## [13]  4.6417801  4.9285800  6.0449121 10.9596637  6.8143330  5.4802758
    ## [19]  5.3468205  0.5879641  3.8789163 11.7051481  2.5359015  1.3976072
    ## [25]  1.4428135  4.0649934  7.5979978  3.5306887  3.5432670  4.5706793
    ## [31]  4.1629312  6.8538031  1.2505792  8.7298034  6.6661720 10.4680522
    ## [37]  5.9346383  5.4465619 11.3378817  3.6499349  6.1214941  4.4922586
    ## [43]  8.9882738  0.5757569  1.7088209  5.1395515  4.6374694  3.7602859
    ## [49]  1.7219826  7.9427799
    ## 
    ## $c
    ##  [1] 20.85857 20.23221 17.12122 19.40826 21.23631 19.35491 18.16119 21.44041
    ##  [9] 19.61571 20.12851 20.25850 19.82311 19.68834 20.88972 21.29818 19.44443
    ## [17] 19.38147 17.58377 22.44601 20.36092 19.57931 18.69313 20.65174 21.29881
    ## [25] 20.41548 21.83667 18.68583 20.58668 19.41682 20.93572 18.47481 18.69835
    ## [33] 18.54350 20.00356 21.05689 21.98011 20.89175 19.94877 20.77342 18.74783
    ## [41] 19.48190 18.21174 18.58788 18.95788 20.81990 19.55723 19.52704 19.67913
    ## [49] 19.91652 19.90991
    ## 
    ## $d
    ##  [1] -12.65663 -11.64141 -12.51539 -12.17288 -12.85353 -10.51136 -12.57778
    ##  [8] -11.85557 -11.90726 -11.83225 -12.58423 -11.31003 -12.95632 -11.70780
    ## [15] -12.47904 -11.64228 -11.59740 -12.18068 -13.35464 -11.40037 -13.04183
    ## [22] -12.01234 -12.44892 -12.80999 -12.11105 -11.91633 -13.10916 -11.94643
    ## [29] -12.67624 -12.26875 -12.41409 -12.14065 -11.23229 -11.81306 -11.68102
    ## [36] -12.49201 -12.14427 -11.63440 -11.61435 -11.60719 -11.40879 -11.34228
    ## [43] -12.18717 -11.63335 -11.56734 -11.47968 -12.38159 -12.32434 -12.52410
    ## [50] -12.03010

``` r
list_col_df$norms[[1]]
```

    ##  [1]  3.9705016  2.5252311  2.1458872  2.2519902  2.6618673  2.9182781
    ##  [7]  2.4629941  1.1296093  0.5903122  0.2628728  0.5247520  1.9964688
    ## [13]  3.2801921  1.6188002  1.9601045  3.4723155  2.1959557  0.4355779
    ## [19]  2.7692951  2.7540386  0.3748014  1.2290479  2.3028134  2.3706756
    ## [25]  2.5745317  1.3519972  0.8332585  0.2932868  2.0620608  3.5402373
    ## [31]  0.2758835  2.9086491  2.8093401  0.0373101  2.5533016  3.0940533
    ## [37]  3.4537862  2.0989652  3.9301051  1.7877567  0.6928137  1.7127018
    ## [43]  1.1271763  0.9223215 -0.2205157  2.6312269  3.0446265  2.1363165
    ## [49]  3.5087756  0.6817882

``` r
mean_and_sd(list_col_df$norms[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.96  1.12

``` r
mean_and_sd(list_col_df$norms[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.24  2.87

``` r
mean_and_sd(list_col_df$norms[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  19.9  1.15

``` r
mean_and_sd(list_col_df$norms[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -12.1 0.567

``` r
list_col_df %>% 
  mutate(summaries = map(norms, mean_and_sd)) 
```

    ## # A tibble: 4 × 3
    ##   name  norms        summaries       
    ##   <chr> <named list> <named list>    
    ## 1 a     <dbl [50]>   <tibble [1 × 2]>
    ## 2 b     <dbl [50]>   <tibble [1 × 2]>
    ## 3 c     <dbl [50]>   <tibble [1 × 2]>
    ## 4 d     <dbl [50]>   <tibble [1 × 2]>

``` r
list_col_df %>% 
  mutate(summaries = map(norms, mean_and_sd)) %>% 
  pull(summaries)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.96  1.12
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.24  2.87
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  19.9  1.15
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -12.1 0.567

playing with various ways of extracting list elements from a list column

## Nested data

When would you atually end up with a dataframe inside of a dataframe?

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2021-10-05 10:31:07 (7.602)

    ## file min/max dates: 1869-01-01 / 2021-10-31

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2021-10-05 10:31:12 (1.697)

    ## file min/max dates: 1965-01-01 / 2020-02-29

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2021-10-05 10:31:14 (0.912)

    ## file min/max dates: 1999-09-01 / 2021-09-30

We have a lot of information nested into one study item (Central Park,
Waterhole, Waikiki)

Nest data within location

``` r
weather_nested = nest(weather_df, data = date:tmin)

weather_nested %>% 
  filter(name == "CentralPark_NY") %>% 
  pull(data) 
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows

``` r
weather_nested %>% 
  pull(data) 
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

Let’s do something with linear regression - let’s say we want to see how
tmax is related to tmin

``` r
weather_nested$data[[1]]
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows

``` r
weather_nested$data[[2]]
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows

``` r
weather_nested$data[[3]]
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

``` r
lm(tmax ~tmin, data = weather_nested$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nested$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
lm(tmax ~tmin, data = weather_nested$data[[2]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nested$data[[2]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509

``` r
lm(tmax ~tmin, data = weather_nested$data[[3]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nested$data[[3]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

``` r
weather_lm = function(df) {
  
  lm(tmax ~tmin, data = df)

}

weather_lm(weather_nested$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
lm(tmax ~tmin, data = weather_nested$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nested$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

bottom two things are the same (using function vs. not)

I want to have another column inside of my dataset that is the result of
the linear model fit

``` r
map(weather_nested$data, weather_lm) 
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

``` r
weather_nested %>% 
  mutate(lm_results = map(data, weather_lm))
```

    ## # A tibble: 3 × 4
    ##   name           id          data               lm_results
    ##   <chr>          <chr>       <list>             <list>    
    ## 1 CentralPark_NY USW00094728 <tibble [365 × 4]> <lm>      
    ## 2 Waikiki_HA     USC00519397 <tibble [365 × 4]> <lm>      
    ## 3 Waterhole_WA   USS0023B17S <tibble [365 × 4]> <lm>

You can also unnest

``` r
unnest(weather_nested, data)
```

    ## # A tibble: 1,095 × 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2017-01-01     0   8.9   4.4
    ##  2 CentralPark_NY USW00094728 2017-01-02    53   5     2.8
    ##  3 CentralPark_NY USW00094728 2017-01-03   147   6.1   3.9
    ##  4 CentralPark_NY USW00094728 2017-01-04     0  11.1   1.1
    ##  5 CentralPark_NY USW00094728 2017-01-05     0   1.1  -2.7
    ##  6 CentralPark_NY USW00094728 2017-01-06    13   0.6  -3.8
    ##  7 CentralPark_NY USW00094728 2017-01-07    81  -3.2  -6.6
    ##  8 CentralPark_NY USW00094728 2017-01-08     0  -3.8  -8.8
    ##  9 CentralPark_NY USW00094728 2017-01-09     0  -4.9  -9.9
    ## 10 CentralPark_NY USW00094728 2017-01-10     0   7.8  -6  
    ## # … with 1,085 more rows

``` r
weather_nested %>% 
  unnest(data)
```

    ## # A tibble: 1,095 × 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2017-01-01     0   8.9   4.4
    ##  2 CentralPark_NY USW00094728 2017-01-02    53   5     2.8
    ##  3 CentralPark_NY USW00094728 2017-01-03   147   6.1   3.9
    ##  4 CentralPark_NY USW00094728 2017-01-04     0  11.1   1.1
    ##  5 CentralPark_NY USW00094728 2017-01-05     0   1.1  -2.7
    ##  6 CentralPark_NY USW00094728 2017-01-06    13   0.6  -3.8
    ##  7 CentralPark_NY USW00094728 2017-01-07    81  -3.2  -6.6
    ##  8 CentralPark_NY USW00094728 2017-01-08     0  -3.8  -8.8
    ##  9 CentralPark_NY USW00094728 2017-01-09     0  -4.9  -9.9
    ## 10 CentralPark_NY USW00094728 2017-01-10     0   7.8  -6  
    ## # … with 1,085 more rows

Can also use pipes

## Revist Napoleon

Function to get reviews/ stars

``` r
get_page_reviews = function(page_url) {
  
  page_html = read_html(page_url)

  review_titles = 
    page_html %>%
    html_elements(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    page_html %>%
    html_elements("#cm_cr-review_list .review-rating") %>%
    html_text()
  
  review_text = 
    page_html %>%
    html_elements(".review-text-content span") %>%
    html_text()
  
  reviews = 
    tibble(
      title = review_titles,
      stars = review_stars,
      text = review_text
  )
  
  return(reviews)
  
}


base_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

urls = str_c(base_url, 1:5)

map(urls, get_page_reviews)
```

    ## [[1]]
    ## # A tibble: 10 × 3
    ##    title                                                 stars   text           
    ##    <chr>                                                 <chr>   <chr>          
    ##  1 Vintage                                               5.0 ou… "\n  Easy to o…
    ##  2 too many commercials                                  1.0 ou… "\n  5 minutes…
    ##  3 this film is so good!                                 5.0 ou… "\n  VOTE FOR …
    ##  4 Good movie                                            5.0 ou… "\n  Weird sto…
    ##  5 I Just everyone to know this....                      5.0 ou… "\n  VOTE FOR …
    ##  6 the cobweb in his hair during the bike ramp scene lol 5.0 ou… "\n  5 stars f…
    ##  7 Best quirky movie ever                                5.0 ou… "\n  You all k…
    ##  8 Classic Film                                          5.0 ou… "\n  Had to or…
    ##  9 hehehehe                                              5.0 ou… "\n  goodjobbo…
    ## 10 Painful                                               1.0 ou… "\n  I think I…
    ## 
    ## [[2]]
    ## # A tibble: 10 × 3
    ##    title                             stars              text                    
    ##    <chr>                             <chr>              <chr>                   
    ##  1 GRAND                             5.0 out of 5 stars "\n  GRAND\n"           
    ##  2 Hello, 90s                        5.0 out of 5 stars "\n  So nostalgic movie…
    ##  3 Cult Classic                      5.0 out of 5 stars "\n  Watched it with my…
    ##  4 Format was inaccurate             4.0 out of 5 stars "\n  There was an optio…
    ##  5 Good funny                        3.0 out of 5 stars "\n  Would recommend\n" 
    ##  6 Not available w/in 48 hour window 1.0 out of 5 stars "\n  I couldn't watch i…
    ##  7 Your mom went to college.         5.0 out of 5 stars "\n  Classic funny movi…
    ##  8 Very funny movie                  5.0 out of 5 stars "\n  I watch this movie…
    ##  9 Watch it twice! Trust me!         5.0 out of 5 stars "\n  Nothing to dislike…
    ## 10 A classic                         5.0 out of 5 stars "\n  If you don’t enjoy…
    ## 
    ## [[3]]
    ## # A tibble: 10 × 3
    ##    title                                       stars              text          
    ##    <chr>                                       <chr>              <chr>         
    ##  1 Can't say how many times I've seen          5.0 out of 5 stars "\n  Such a g…
    ##  2 I pity the fool who doesn’t own this movie. 5.0 out of 5 stars "\n  I love t…
    ##  3 I don’t know why it’s so popular!           2.0 out of 5 stars "\n  My girlf…
    ##  4 Okay                                        3.0 out of 5 stars "\n  Okay\n"  
    ##  5 A WHOLESOME comedic journey                 5.0 out of 5 stars "\n  Not a mo…
    ##  6 Hilarious                                   5.0 out of 5 stars "\n  Funny\n" 
    ##  7 Love it                                     5.0 out of 5 stars "\n  What of …
    ##  8 WORTH IT!                                   5.0 out of 5 stars "\n  It's the…
    ##  9 Funny movie.                                5.0 out of 5 stars "\n  Great co…
    ## 10 Best movie ever!                            5.0 out of 5 stars "\n  Got this…
    ## 
    ## [[4]]
    ## # A tibble: 10 × 3
    ##    title                                         stars              text        
    ##    <chr>                                         <chr>              <chr>       
    ##  1 I was stuck in the oil patch back in the day. 5.0 out of 5 stars "\n  I watc…
    ##  2 Funny Dork humor                              5.0 out of 5 stars "\n  Humor …
    ##  3 Still funny!                                  5.0 out of 5 stars "\n  Still …
    ##  4 Love it!! 💜                                  5.0 out of 5 stars "\n  Love i…
    ##  5 LOVE it                                       5.0 out of 5 stars "\n  cult c…
    ##  6 Perfect                                       5.0 out of 5 stars "\n  Exactl…
    ##  7 Love this movie!                              5.0 out of 5 stars "\n  Great …
    ##  8 Love it                                       5.0 out of 5 stars "\n  Love t…
    ##  9 As described                                  3.0 out of 5 stars "\n  Book i…
    ## 10 GOSH!!!                                       5.0 out of 5 stars "\n  Just w…
    ## 
    ## [[5]]
    ## # A tibble: 10 × 3
    ##    title                             stars              text                    
    ##    <chr>                             <chr>              <chr>                   
    ##  1 Watch it right now                5.0 out of 5 stars "\n  You need to watch …
    ##  2 At this point it’s an addiction   5.0 out of 5 stars "\n  I watch this movie…
    ##  3 💕                                5.0 out of 5 stars "\n  Hands down, one of…
    ##  4 Good dumb movie                   5.0 out of 5 stars "\n  I really wanted to…
    ##  5 funny                             5.0 out of 5 stars "\n  so funny and inven…
    ##  6 Best Movie- Try to prove me wrong 5.0 out of 5 stars "\n  Best movie ever\n" 
    ##  7 Vote For Pedro!!                  5.0 out of 5 stars "\n  What is NOT to lik…
    ##  8 So Funny                          5.0 out of 5 stars "\n  This is such a goo…
    ##  9 Best movie ever                   5.0 out of 5 stars "\n  It's napoleon dyna…
    ## 10 Funny                             5.0 out of 5 stars "\n  Classic\n"

``` r
napoleon_df = 
  tibble(
    urls = urls
)

napoleon_df %>% 
  mutate(reviews = map(urls, get_page_reviews)) %>% 
  select(reviews) %>% 
  unnest()
```

    ## Warning: `cols` is now required when using unnest().
    ## Please use `cols = c(reviews)`

    ## # A tibble: 50 × 3
    ##    title                                                 stars   text           
    ##    <chr>                                                 <chr>   <chr>          
    ##  1 Vintage                                               5.0 ou… "\n  Easy to o…
    ##  2 too many commercials                                  1.0 ou… "\n  5 minutes…
    ##  3 this film is so good!                                 5.0 ou… "\n  VOTE FOR …
    ##  4 Good movie                                            5.0 ou… "\n  Weird sto…
    ##  5 I Just everyone to know this....                      5.0 ou… "\n  VOTE FOR …
    ##  6 the cobweb in his hair during the bike ramp scene lol 5.0 ou… "\n  5 stars f…
    ##  7 Best quirky movie ever                                5.0 ou… "\n  You all k…
    ##  8 Classic Film                                          5.0 ou… "\n  Had to or…
    ##  9 hehehehe                                              5.0 ou… "\n  goodjobbo…
    ## 10 Painful                                               1.0 ou… "\n  I think I…
    ## # … with 40 more rows
