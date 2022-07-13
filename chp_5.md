---
title: "Learning R and R Markdown"
author: "Heena Jaffri"
date: '2022-07-12'
output: 
  html_document: 
    keep_md: yes
---




```r
library(gapminder)
```

```
## Warning: package 'gapminder' was built under R version 4.2.1
```

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
## ✔ tibble  3.1.7     ✔ dplyr   1.0.9
## ✔ tidyr   1.2.0     ✔ stringr 1.4.0
## ✔ readr   2.1.2     ✔ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```



```r
str(gapminder)
```

```
## tibble [1,704 × 6] (S3: tbl_df/tbl/data.frame)
##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
##  $ year     : int [1:1704] 1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
##  $ lifeExp  : num [1:1704] 28.8 30.3 32 34 36.1 ...
##  $ pop      : int [1:1704] 8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
##  $ gdpPercap: num [1:1704] 779 821 853 836 740 ...
```

```r
head(gapminder$lifeExp)
```

```
## [1] 28.801 30.332 31.997 34.020 36.088 38.438
```

```r
hist(gapminder$lifeExp)
```

![](chp_5_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
table(gapminder$continent)
```

```
## 
##   Africa Americas     Asia   Europe  Oceania 
##      624      300      396      360       24
```


```r
gapminder
```

```
## # A tibble: 1,704 × 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
## # … with 1,694 more rows
```

```r
gapminder %>% head()
```

```
## # A tibble: 6 × 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

```r
select(gapminder, year, lifeExp)
```

```
## # A tibble: 1,704 × 2
##     year lifeExp
##    <int>   <dbl>
##  1  1952    28.8
##  2  1957    30.3
##  3  1962    32.0
##  4  1967    34.0
##  5  1972    36.1
##  6  1977    38.4
##  7  1982    39.9
##  8  1987    40.8
##  9  1992    41.7
## 10  1997    41.8
## # … with 1,694 more rows
```

```r
#with pipe

gapminder %>% 
  select(year, lifeExp) %>% 
  head(4)
```

```
## # A tibble: 4 × 2
##    year lifeExp
##   <int>   <dbl>
## 1  1952    28.8
## 2  1957    30.3
## 3  1962    32.0
## 4  1967    34.0
```
CH 7: Single Table dplyr Functions


```r
(my_gap <- gapminder)
```

```
## # A tibble: 1,704 × 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
## # … with 1,694 more rows
```

```r
my_gap %>% filter(country=="Canada")
```

```
## # A tibble: 12 × 6
##    country continent  year lifeExp      pop gdpPercap
##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Canada  Americas   1952    68.8 14785584    11367.
##  2 Canada  Americas   1957    70.0 17010154    12490.
##  3 Canada  Americas   1962    71.3 18985849    13462.
##  4 Canada  Americas   1967    72.1 20819767    16077.
##  5 Canada  Americas   1972    72.9 22284500    18971.
##  6 Canada  Americas   1977    74.2 23796400    22091.
##  7 Canada  Americas   1982    75.8 25201900    22899.
##  8 Canada  Americas   1987    76.9 26549700    26627.
##  9 Canada  Americas   1992    78.0 28523502    26343.
## 10 Canada  Americas   1997    78.6 30305843    28955.
## 11 Canada  Americas   2002    79.8 31902268    33329.
## 12 Canada  Americas   2007    80.7 33390141    36319.
```

```r
my_precious <- my_gap %>% filter(country=="Canada")
```




```r
my_gap %>% mutate(gdp = pop * gdpPercap)
```

```
## # A tibble: 1,704 × 7
##    country     continent  year lifeExp      pop gdpPercap          gdp
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>        <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.  6567086330.
##  2 Afghanistan Asia       1957    30.3  9240934      821.  7585448670.
##  3 Afghanistan Asia       1962    32.0 10267083      853.  8758855797.
##  4 Afghanistan Asia       1967    34.0 11537966      836.  9648014150.
##  5 Afghanistan Asia       1972    36.1 13079460      740.  9678553274.
##  6 Afghanistan Asia       1977    38.4 14880372      786. 11697659231.
##  7 Afghanistan Asia       1982    39.9 12881816      978. 12598563401.
##  8 Afghanistan Asia       1987    40.8 13867957      852. 11820990309.
##  9 Afghanistan Asia       1992    41.7 16317921      649. 10595901589.
## 10 Afghanistan Asia       1997    41.8 22227415      635. 14121995875.
## # … with 1,694 more rows
```

```r
ctib <- my_gap %>% 
  filter(country=="Canada")

my_gap <- my_gap %>% 
  mutate(tmp=rep(ctib$gdpPercap, nlevels(country)), #mutate builds things sequentially so you can refer to earlier ones. setting a variable to null gets rid of it
 gdpPercapRel = gdpPercap/tmp,
 tmp=NULL)
```

```r
my_gap %>% 
  filter(country=="Canada") %>% 
  select(country,year,gdpPercapRel)
```

```
## # A tibble: 12 × 3
##    country  year gdpPercapRel
##    <fct>   <int>        <dbl>
##  1 Canada   1952            1
##  2 Canada   1957            1
##  3 Canada   1962            1
##  4 Canada   1967            1
##  5 Canada   1972            1
##  6 Canada   1977            1
##  7 Canada   1982            1
##  8 Canada   1987            1
##  9 Canada   1992            1
## 10 Canada   1997            1
## 11 Canada   2002            1
## 12 Canada   2007            1
```

```r
summary(my_gap$gdpPercapRel)
```

```
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
## 0.007236 0.061648 0.171521 0.326659 0.446564 9.534690
```


```r
my_gap %>% 
  arrange(year,country)
```

```
## # A tibble: 1,704 × 7
##    country     continent  year lifeExp      pop gdpPercap gdpPercapRel
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>        <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.       0.0686
##  2 Albania     Europe     1952    55.2  1282697     1601.       0.141 
##  3 Algeria     Africa     1952    43.1  9279525     2449.       0.215 
##  4 Angola      Africa     1952    30.0  4232095     3521.       0.310 
##  5 Argentina   Americas   1952    62.5 17876956     5911.       0.520 
##  6 Australia   Oceania    1952    69.1  8691212    10040.       0.883 
##  7 Austria     Europe     1952    66.8  6927772     6137.       0.540 
##  8 Bahrain     Asia       1952    50.9   120447     9867.       0.868 
##  9 Bangladesh  Asia       1952    37.5 46886859      684.       0.0602
## 10 Belgium     Europe     1952    68    8730405     8343.       0.734 
## # … with 1,694 more rows
```

```r
my_gap %>% 
  filter(year==2007) %>% 
  arrange(desc(lifeExp))
```

```
## # A tibble: 142 × 7
##    country          continent  year lifeExp       pop gdpPercap gdpPercapRel
##    <fct>            <fct>     <int>   <dbl>     <int>     <dbl>        <dbl>
##  1 Japan            Asia       2007    82.6 127467972    31656.        0.872
##  2 Hong Kong, China Asia       2007    82.2   6980412    39725.        1.09 
##  3 Iceland          Europe     2007    81.8    301931    36181.        0.996
##  4 Switzerland      Europe     2007    81.7   7554661    37506.        1.03 
##  5 Australia        Oceania    2007    81.2  20434176    34435.        0.948
##  6 Spain            Europe     2007    80.9  40448191    28821.        0.794
##  7 Sweden           Europe     2007    80.9   9031088    33860.        0.932
##  8 Israel           Asia       2007    80.7   6426679    25523.        0.703
##  9 France           Europe     2007    80.7  61083916    30470.        0.839
## 10 Canada           Americas   2007    80.7  33390141    36319.        1    
## # … with 132 more rows
```

```r
today()
```

```
## [1] "2022-07-13"
```



