notebook
================

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax
for authoring HTML, PDF, and MS Word documents. For more details on
using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that
includes both content as well as the output of any embedded R code
chunks within the document. You can embed an R code chunk like this:

``` r
summary(cars)
```

    ##      speed           dist       
    ##  Min.   : 4.0   Min.   :  2.00  
    ##  1st Qu.:12.0   1st Qu.: 26.00  
    ##  Median :15.0   Median : 36.00  
    ##  Mean   :15.4   Mean   : 42.98  
    ##  3rd Qu.:19.0   3rd Qu.: 56.00  
    ##  Max.   :25.0   Max.   :120.00

## Including Plots

You can also embed plots, for example:

![](notebook_files/figure-gfm/pressure-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.

``` r
knitr::kable(head(iris))
```

| Sepal.Length | Sepal.Width | Petal.Length | Petal.Width | Species |
| -----------: | ----------: | -----------: | ----------: | :------ |
|          5.1 |         3.5 |          1.4 |         0.2 | setosa  |
|          4.9 |         3.0 |          1.4 |         0.2 | setosa  |
|          4.7 |         3.2 |          1.3 |         0.2 | setosa  |
|          4.6 |         3.1 |          1.5 |         0.2 | setosa  |
|          5.0 |         3.6 |          1.4 |         0.2 | setosa  |
|          5.4 |         3.9 |          1.7 |         0.4 | setosa  |

-----

``` r
library("ggplot2")
library("dplyr")
```

``` r
smaller <- diamonds %>%
  filter(carat <= 2.5)
```

## Size and Cut, Color, and Clarity

Diamonds with lower quality cuts (cuts are ranked from “Ideal” to
“Fair”) tend to be be larger.

``` r
ggplot(diamonds, aes(y = carat, x = cut)) +
  geom_boxplot()
```

![](notebook_files/figure-gfm/unnamed-chunk-5-1.png)<!-- --> Likewise,
diamonds with worse color (diamond colors are ranked from J (worst) to D
(best)) tend to be larger:

``` r
ggplot(diamonds, aes(y = carat, x = color)) +
  geom_boxplot()
```

![](notebook_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

The pattern present in cut and color is also present in clarity.
Diamonds with worse clarity (I1 (worst), SI1, SI2, VS1, VS2, VVS1, VVS2,
IF (best)) tend to be larger:

``` r
ggplot(diamonds, aes(y = carat, x = clarity)) +
  geom_boxplot()
```

![](notebook_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

These patterns are consistent with there being a profitability threshold
for retail diamonds that is a function of carat, clarity, color, cut and
other characteristics. A diamond may be profitable to sell if a poor
value of one feature, for example, poor clarity, color, or cut, is be
offset by a good value of another feature, such as a large size. This
can be considered an example of [Berkson’s
paradox](https://en.wikipedia.org/wiki/Berkson%27s_paradox).

## Largest Diamonds

We have data about 53,940 diamonds. Only 126 (0.2%) are larger than 2.5
carats. The distribution of the remainder is shown below:

``` r
smaller %>%
  ggplot(aes(carat)) +
  geom_freqpoly(binwidth = 0.01)
```

![](notebook_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

The frequency distribution of diamond sizes is marked by spikes at
whole-number and half-carat values, as well as several other carat
values corresponding to fractions.

The largest twenty diamonds (by carat) in the datasets are,

``` r
diamonds %>%
  arrange(desc(carat)) %>%
  slice(1:20) %>%
  select(carat, cut, color, clarity) %>%
  knitr::kable(
    caption = "The largest 20 diamonds in the `diamonds` dataset."
  )
```

| carat | cut       | color | clarity |
| ----: | :-------- | :---- | :------ |
|  5.01 | Fair      | J     | I1      |
|  4.50 | Fair      | J     | I1      |
|  4.13 | Fair      | H     | I1      |
|  4.01 | Premium   | I     | I1      |
|  4.01 | Premium   | J     | I1      |
|  4.00 | Very Good | I     | I1      |
|  3.67 | Premium   | I     | I1      |
|  3.65 | Fair      | H     | I1      |
|  3.51 | Premium   | J     | VS2     |
|  3.50 | Ideal     | H     | I1      |
|  3.40 | Fair      | D     | I1      |
|  3.24 | Premium   | H     | I1      |
|  3.22 | Ideal     | I     | I1      |
|  3.11 | Fair      | J     | I1      |
|  3.05 | Premium   | E     | I1      |
|  3.04 | Very Good | I     | SI2     |
|  3.04 | Premium   | I     | SI2     |
|  3.02 | Fair      | I     | I1      |
|  3.01 | Premium   | I     | I1      |
|  3.01 | Premium   | F     | I1      |

The largest 20 diamonds in the `diamonds` dataset.

Most of the twenty largest datasets are in the lowest clarity category
(“I1”), with one being in the second best category (“VVS2”) The top
twenty diamonds have colors ranging from the worst, “J”, to best,
“D”,categories, though most are in the lower categories “J” and “I”.
The top twenty diamonds are more evenly distributed among the cut
categories, from “Fair” to “Ideal”, although the worst category (Fair)
is the most common.
