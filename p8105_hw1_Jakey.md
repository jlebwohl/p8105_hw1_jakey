Homework 1
================
Jakey Lebwohl
July 5 2023

# Problem 1

Before getting started, we will need the *tidyverse* library.

``` r
library(tidyverse)
```

The following code chunk loads the penguins dataset.

``` r
data("penguins", package = "palmerpenguins")
```

This dataset has 8 variables: for each of the 344 penguins, the dataset
includes the species, island, bill length (in mm), bill depth (mm),
flipper length (mm), body mass (g), sex, and year.

In order to find the mean flipper length of the penguins in this data
set, we can use the *mean* function, excluding penguins without
flippers:

``` r
flipper_mean = mean(penguins$flipper_length_mm, na.rm=TRUE)
round(flipper_mean, digits = 2)
```

    ## [1] 200.92

We can make a scatterplot too. The following code chunk creates a
scatterplot with flipper length (mm) as the y variable and bill length
(mm) as the x variable.

``` r
ggplot(penguins, aes(x = bill_length_mm, y = flipper_length_mm, color = species)) + geom_point() # make scatterplot
```

    ## Warning: Removed 2 rows containing missing values (`geom_point()`).

![](p8105_hw1_Jakey_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
ggsave("flippers_vs_bills.pdf") # save scatterplot as pdf
```

    ## Saving 7 x 5 in image

    ## Warning: Removed 2 rows containing missing values (`geom_point()`).

# Problem 2

We will now create a random sample of size 10 from a normal distribution
and assign certain variables to each number.

``` r
sample_df = tibble(
  value = rnorm(10),
  value_pos = value > 0,
  value_word = c("2", "3", "5", "7", "11", "13", "17", "19", "23", "29"),
  value_factor = factor(c("one", "prime", "prime", "composite", "prime", "composite", "prime", "composite", "composite", "composite"))
)
```

We *can* take the mean of the set of 10 numerical values: the mean is
0.183. This mean should be close to 0: there is a 50% probability the
mean will be between -0.213 and 0.213. In this case, it **is**.

We *can* take the mean of the set of logical values. This is because
TRUE and FALSE are interchanged with 0 and 1:

``` r
pull(sample_df, value_pos)
```

    ##  [1] FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE FALSE  TRUE FALSE

``` r
mean(pull(sample_df, value_pos))
```

    ## [1] 0.5

We *cannot* take the mean of the set of characters:

``` r
mean(pull(sample_df, value_word))
```

    ## Warning in mean.default(pull(sample_df, value_word)): argument is not numeric
    ## or logical: returning NA

    ## [1] NA

We *cannot* take the mean of the set of factors:

``` r
mean(pull(sample_df, value_factor))
```

    ## Warning in mean.default(pull(sample_df, value_factor)): argument is not numeric
    ## or logical: returning NA

    ## [1] NA

Using *as.numeric*, we can convert many types of variables to numeric
values:

``` r
x = as.numeric(pull(sample_df, value_pos))
y = as.numeric(pull(sample_df, value_word))
z = as.numeric(pull(sample_df, value_factor))
```

When this function is applied to a logical value, TRUE gets converted to
1 and FALSE gets converted to 0.

When this function is applied to a character value, any numbers in the
character value get converted to numerical values. Every other character
value is converted to NA.

When this function is applied to a factor, it gives each factor a
different natural number (apparently dependent on its position among all
other factors alphabetically).
