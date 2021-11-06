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

    ##  [1] -0.09572689  0.67399096 -0.99920839  0.33863256  0.75257328  1.95016139
    ##  [7]  0.19348458  0.08252495 -2.15560172  0.02723626 -1.04292104  0.79587594
    ## [13] -0.70743943 -0.70625483  0.33584198  1.35047546  0.32271769  0.71725218
    ## [19]  0.01952805 -1.24065576 -1.20637254 -0.79186555 -0.91901687  1.84674563
    ## [25]  0.45802210

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

    ##  [1] -0.09572689  0.67399096 -0.99920839  0.33863256  0.75257328  1.95016139
    ##  [7]  0.19348458  0.08252495 -2.15560172  0.02723626 -1.04292104  0.79587594
    ## [13] -0.70743943 -0.70625483  0.33584198  1.35047546  0.32271769  0.71725218
    ## [19]  0.01952805 -1.24065576 -1.20637254 -0.79186555 -0.91901687  1.84674563
    ## [25]  0.45802210

Looks like the same collection of z scores

``` r
y_vec = rnorm(40, mean = 12, sd = .3) 

z_scores(y_vec)
```

    ##  [1] -0.09169816 -0.34785766 -0.50845610 -1.19890989  0.84467731 -0.26174750
    ##  [7]  1.10710172 -0.51278118 -0.53389041  0.89508294 -1.02031200 -0.24435627
    ## [13]  0.13647690 -1.63674403  0.84468389  2.15732758  0.10708854 -0.80660652
    ## [19]  0.42531002  0.74381958 -1.96019112 -0.68143539  1.51144268 -1.32341020
    ## [25] -0.60079759  0.66951662  1.52515460 -1.60258069  1.08335459 -0.23718969
    ## [31] -0.74529240  2.12685056  0.99250095  0.03357858 -0.68089961  0.08989877
    ## [37]  0.36310888  0.01923711 -1.07493546  0.39388002

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
    ## 1  12.1 0.261

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.86  3.54

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
    ## 1  1.60  3.30

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
    ## 1  3.17  2.86

## Revisit Napolean Dynamite

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_elements(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_elements("#cm_cr-review_list .review-rating") %>%
  html_text()

review_text = 
  dynamite_html %>%
  html_elements(".review-text-content span") %>%
  html_text()

reviews = 
  tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
)
```

Okay but there are a lot of pages of reviews

Could copy and paste everything above to page number 2, 3, 4, 5, and
update the reviews df to 2, 3, 4, etc.

Instead, write a function that gets reviews based on page url

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

page_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

get_page_reviews(page_url)
```

    ## # A tibble: 10 × 3
    ##    title                                                 stars   text           
    ##    <chr>                                                 <chr>   <chr>          
    ##  1 this film is so good!                                 5.0 ou… "\n  VOTE FOR …
    ##  2 Good movie                                            5.0 ou… "\n  Weird sto…
    ##  3 I Just everyone to know this....                      5.0 ou… "\n  VOTE FOR …
    ##  4 the cobweb in his hair during the bike ramp scene lol 5.0 ou… "\n  5 stars f…
    ##  5 Best quirky movie ever                                5.0 ou… "\n  You all k…
    ##  6 Classic Film                                          5.0 ou… "\n  Had to or…
    ##  7 hehehehe                                              5.0 ou… "\n  goodjobbo…
    ##  8 Painful                                               1.0 ou… "\n  I think I…
    ##  9 GRAND                                                 5.0 ou… "\n  GRAND\n"  
    ## 10 Hello, 90s                                            5.0 ou… "\n  So nostal…

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=2"

get_page_reviews(url)
```

    ## # A tibble: 10 × 3
    ##    title                                       stars              text          
    ##    <chr>                                       <chr>              <chr>         
    ##  1 Cult Classic                                5.0 out of 5 stars "\n  Watched …
    ##  2 Format was inaccurate                       4.0 out of 5 stars "\n  There wa…
    ##  3 Good funny                                  3.0 out of 5 stars "\n  Would re…
    ##  4 Not available w/in 48 hour window           1.0 out of 5 stars "\n  I couldn…
    ##  5 Your mom went to college.                   5.0 out of 5 stars "\n  Classic …
    ##  6 Very funny movie                            5.0 out of 5 stars "\n  I watch …
    ##  7 Watch it twice! Trust me!                   5.0 out of 5 stars "\n  Nothing …
    ##  8 A classic                                   5.0 out of 5 stars "\n  If you d…
    ##  9 Can't say how many times I've seen          5.0 out of 5 stars "\n  Such a g…
    ## 10 I pity the fool who doesn’t own this movie. 5.0 out of 5 stars "\n  I love t…

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=3"

get_page_reviews(url)
```

    ## # A tibble: 10 × 3
    ##    title                                         stars              text        
    ##    <chr>                                         <chr>              <chr>       
    ##  1 I don’t know why it’s so popular!             2.0 out of 5 stars "\n  My gir…
    ##  2 Okay                                          3.0 out of 5 stars "\n  Okay\n"
    ##  3 A WHOLESOME comedic journey                   5.0 out of 5 stars "\n  Not a …
    ##  4 Hilarious                                     5.0 out of 5 stars "\n  Funny\…
    ##  5 Love it                                       5.0 out of 5 stars "\n  What o…
    ##  6 WORTH IT!                                     5.0 out of 5 stars "\n  It's t…
    ##  7 Funny movie.                                  5.0 out of 5 stars "\n  Great …
    ##  8 Best movie ever!                              5.0 out of 5 stars "\n  Got th…
    ##  9 I was stuck in the oil patch back in the day. 5.0 out of 5 stars "\n  I watc…
    ## 10 Funny Dork humor                              5.0 out of 5 stars "\n  Humor …

``` r
base_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

urls = str_c(base_url, 1:5)

bind_rows(
  get_page_reviews(urls[1]),
  get_page_reviews(urls[2]),
  get_page_reviews(urls[3]),
  get_page_reviews(urls[4]),
  get_page_reviews(urls[5]))
```

    ## # A tibble: 50 × 3
    ##    title                                                 stars   text           
    ##    <chr>                                                 <chr>   <chr>          
    ##  1 this film is so good!                                 5.0 ou… "\n  VOTE FOR …
    ##  2 Good movie                                            5.0 ou… "\n  Weird sto…
    ##  3 I Just everyone to know this....                      5.0 ou… "\n  VOTE FOR …
    ##  4 the cobweb in his hair during the bike ramp scene lol 5.0 ou… "\n  5 stars f…
    ##  5 Best quirky movie ever                                5.0 ou… "\n  You all k…
    ##  6 Classic Film                                          5.0 ou… "\n  Had to or…
    ##  7 hehehehe                                              5.0 ou… "\n  goodjobbo…
    ##  8 Painful                                               1.0 ou… "\n  I think I…
    ##  9 GRAND                                                 5.0 ou… "\n  GRAND\n"  
    ## 10 Hello, 90s                                            5.0 ou… "\n  So nostal…
    ## # … with 40 more rows

Supposed I want to change something else - you can also add that into
the function (i.e., get rid of the at the beginning of the function)

You can pass in a summary function into your function (i.e., summarize
this vector using the mean, median, standard deviation, etc.) - useful
in factor reorder according to the median

EXAMPLE OF SCOPING

``` r
f = function(x) {
  z = x + y 
  z
}

x = 1
y = 2 

f(x = y) 
```

    ## [1] 4
