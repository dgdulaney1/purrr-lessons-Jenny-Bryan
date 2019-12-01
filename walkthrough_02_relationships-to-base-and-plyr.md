Relationships to base and plyr
================

  - [lapply() vs purrr:map()](#lapply-vs-purrrmap)
  - [vapply() vs map\_\*()](#vapply-vs-map_)
  - [map\_dfr()](#map_dfr)
  - [mapply() vs map2(), pmap()](#mapply-vs-map2-pmap)
  - [aggregate() vs dplyr::summarise()](#aggregate-vs-dplyrsummarise)
  - [by() vs tidyr::nest()](#by-vs-tidyrnest)

``` r
library(tidyverse)
```

    ## -- Attaching packages ------------------------------------------------------------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.2.1     v purrr   0.3.3
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   1.0.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## -- Conflicts ---------------------------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(repurrrsive)
library(gapminder)

theme_set(theme_light())

knitr::opts_chunk$set(
  cache = FALSE,
  message = FALSE, 
  warning = FALSE, 
  fig.width = 10, 
  fig.height = 7
  )
```

# lapply() vs purrr:map()

These are list in, list out functions.

  - **Base**

<!-- end list -->

``` r
got_chars[1:3] %>% 
  lapply(function(x) x[["name"]])
```

    ## [[1]]
    ## [1] "Theon Greyjoy"
    ## 
    ## [[2]]
    ## [1] "Tyrion Lannister"
    ## 
    ## [[3]]
    ## [1] "Victarion Greyjoy"

  - **purrr**

<!-- end list -->

``` r
a <- got_chars[1:3] %>% 
  map("name")
```

So purrr knows that “name” is a named element in the list
got\_chars\[1:3\]. It returns a list length 3 with each character’s
name.

-----

# vapply() vs map\_\*()

vapply() requires a template for the return value, while purrr has
map\_\*() functions like map\_chr()

  - **Base**

<!-- end list -->

``` r
got_chars[1:3] %>% 
  vapply(function(x) x[["name"]],
         character(1))
```

    ## [1] "Theon Greyjoy"     "Tyrion Lannister"  "Victarion Greyjoy"

  - \*\*purrr\*

<!-- end list -->

``` r
got_chars[1:3] %>% 
  map_chr("name")
```

    ## [1] "Theon Greyjoy"     "Tyrion Lannister"  "Victarion Greyjoy"

So again, purrr knows that “name” is a named element in the list
got\_chars\[1:3\]; however, this time it returns a character vector with
the names because map\_chr() is used instead of just map().

-----

# map\_dfr()

This function is list in, data frame out

  - **Base**

<!-- end list -->

``` r
l <- got_chars[23:25] %>% 
  lapply("[", c("name", "playedBy"))

mat <- do.call(rbind, l)

(df <- as.data.frame(mat, stringsAsFactors = FALSE))
```

    ##              name      playedBy
    ## 1        Jon Snow Kit Harington
    ## 2   Aeron Greyjoy Michael Feast
    ## 3 Kevan Lannister    Ian Gelder

  - **purrr**

<!-- end list -->

``` r
got_chars[23:25] %>% 
  map_dfr(`[`, c("name", "playedBy"))
```

    ## # A tibble: 3 x 2
    ##   name            playedBy     
    ##   <chr>           <chr>        
    ## 1 Jon Snow        Kit Harington
    ## 2 Aeron Greyjoy   Michael Feast
    ## 3 Kevan Lannister Ian Gelder

-----

# mapply() vs map2(), pmap()

Functions used for mapping over 2+ vectors/lists.

  - **Base**

<!-- end list -->

``` r
nms <- vapply(
  got_chars[16:18],
  `[[`,
  character(1),
  "name"
  )

birth <- vapply(
  got_chars[16:18],
  `[[`,
  character(1),
  "born"
  )

mapply(
  function(x, y) paste(x, "was born", y),
  nms, birth
  )
```

    ##                                     Brandon Stark 
    ## "Brandon Stark was born In 290 AC, at Winterfell" 
    ##                                  Brienne of Tarth 
    ##             "Brienne of Tarth was born In 280 AC" 
    ##                                     Catelyn Stark 
    ##   "Catelyn Stark was born In 264 AC, at Riverrun"

  - **purrr**

<!-- end list -->

``` r
nms <- got_chars[16:18] %>% 
  map_chr("name")

birth <- got_chars[16:18] %>% 
  map_chr("born")

map2_chr(nms, birth, ~ paste(.x, "was born", .y))
```

    ## [1] "Brandon Stark was born In 290 AC, at Winterfell"
    ## [2] "Brienne of Tarth was born In 280 AC"            
    ## [3] "Catelyn Stark was born In 264 AC, at Riverrun"

-----

# aggregate() vs dplyr::summarise()

``` r
(mini_gap <- gapminder %>% 
  filter(country %in% c("Canada", "Germany"), year > 2000) %>% 
  droplevels())
```

    ## # A tibble: 4 x 6
    ##   country continent  year lifeExp      pop gdpPercap
    ##   <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
    ## 1 Canada  Americas   2002    79.8 31902268    33329.
    ## 2 Canada  Americas   2007    80.7 33390141    36319.
    ## 3 Germany Europe     2002    78.7 82350671    30036.
    ## 4 Germany Europe     2007    79.4 82400996    32170.

Single variable summary:

  - **Base**

<!-- end list -->

``` r
aggregate(lifeExp ~ country, mini_gap, mean)
```

    ##   country lifeExp
    ## 1  Canada 80.2115
    ## 2 Germany 79.0380

  - **tidyverse**

<!-- end list -->

``` r
mini_gap %>% 
  group_by(country) %>% 
  summarise(lifeExp = mean(lifeExp))
```

    ## # A tibble: 2 x 2
    ##   country lifeExp
    ##   <fct>     <dbl>
    ## 1 Canada     80.2
    ## 2 Germany    79.0

Multiple variable summary:

  - **Base**

<!-- end list -->

``` r
aggregate(cbind(lifeExp, gdpPercap) ~ country, mini_gap, mean)
```

    ##   country lifeExp gdpPercap
    ## 1  Canada 80.2115  34824.10
    ## 2 Germany 79.0380  31103.09

  - **tidyverse**

<!-- end list -->

``` r
mini_gap %>% 
  group_by(country) %>% 
  summarise_at(vars(lifeExp, gdpPercap), mean)
```

    ## # A tibble: 2 x 3
    ##   country lifeExp gdpPercap
    ##   <fct>     <dbl>     <dbl>
    ## 1 Canada     80.2    34824.
    ## 2 Germany    79.0    31103.

Bivariate summary of two variables for each country:

  - **Base**

<!-- end list -->

``` r
by_obj <- by(gapminder, gapminder$country, function(df) cor(df$lifeExp, df$year))

head(by_obj)
```

    ## gapminder$country
    ## Afghanistan     Albania     Algeria      Angola   Argentina   Australia 
    ##   0.9735051   0.9542420   0.9925307   0.9422392   0.9977816   0.9897716

  - **tidyverse**

<!-- end list -->

``` r
gapminder %>% 
  group_by(country) %>% 
  summarise(cor = cor(lifeExp, year))
```

    ## # A tibble: 142 x 2
    ##    country       cor
    ##    <fct>       <dbl>
    ##  1 Afghanistan 0.974
    ##  2 Albania     0.954
    ##  3 Algeria     0.993
    ##  4 Angola      0.942
    ##  5 Argentina   0.998
    ##  6 Australia   0.990
    ##  7 Austria     0.996
    ##  8 Bahrain     0.983
    ##  9 Bangladesh  0.995
    ## 10 Belgium     0.997
    ## # ... with 132 more rows

-----

# by() vs tidyr::nest()

Goal: fit a linear model of life expectancy vs year

  - **Base**

<!-- end list -->

``` r
by_obj <- by(gapminder,
             gapminder$country,
             function(df) lm(lifeExp ~ year, data = df)
             )

str(by_obj[1:2], max.level = 1)
```

    ## List of 2
    ##  $ Afghanistan:List of 12
    ##   ..- attr(*, "class")= chr "lm"
    ##  $ Albania    :List of 12
    ##   ..- attr(*, "class")= chr "lm"
    ##  - attr(*, "dim")= int 2
    ##  - attr(*, "dimnames")=List of 1

``` r
by_obj$Afghanistan
```

    ## 
    ## Call:
    ## lm(formula = lifeExp ~ year, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         year  
    ##   -507.5343       0.2753

Country information lives inside by\_obj, a nested list, and finding
information about the country like its continent is now quite difficult.

``` r
o_countries <- as.character(unique(gapminder$country[gapminder$continent == "Oceania"]))

by_obj[names(by_obj) %in% o_countries]
```

    ## $Australia
    ## 
    ## Call:
    ## lm(formula = lifeExp ~ year, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         year  
    ##   -376.1163       0.2277  
    ## 
    ## 
    ## $`New Zealand`
    ## 
    ## Call:
    ## lm(formula = lifeExp ~ year, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         year  
    ##   -307.6996       0.1928

In the tidyverse, we create a nested data frame where each row is a
country and the last column contains the linear model data for that
country

  - **tidyverse**

<!-- end list -->

``` r
nested_df <- gapminder %>% 
  group_by(country, continent) %>% 
  nest() %>% 
  mutate(fit = map(data, ~ lm(lifeExp ~ year, data = .x)))

str(nested_df$fit[1:2], max.level = 1)
```

    ## List of 2
    ##  $ :List of 12
    ##   ..- attr(*, "class")= chr "lm"
    ##  $ :List of 12
    ##   ..- attr(*, "class")= chr "lm"

The values in the data column are nested data frames

``` r
nested_df$data[1]
```

    ## <list_of<
    ##   tbl_df<
    ##     year     : integer
    ##     lifeExp  : double
    ##     pop      : integer
    ##     gdpPercap: double
    ##   >
    ## >[1]>
    ## [[1]]
    ## # A tibble: 12 x 4
    ##     year lifeExp      pop gdpPercap
    ##    <int>   <dbl>    <int>     <dbl>
    ##  1  1952    28.8  8425333      779.
    ##  2  1957    30.3  9240934      821.
    ##  3  1962    32.0 10267083      853.
    ##  4  1967    34.0 11537966      836.
    ##  5  1972    36.1 13079460      740.
    ##  6  1977    38.4 14880372      786.
    ##  7  1982    39.9 12881816      978.
    ##  8  1987    40.8 13867957      852.
    ##  9  1992    41.7 16317921      649.
    ## 10  1997    41.8 22227415      635.
    ## 11  2002    42.1 25268405      727.
    ## 12  2007    43.8 31889923      975.

And the values in fit are linear model objects

``` r
nested_df$fit[1]
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = lifeExp ~ year, data = .x)
    ## 
    ## Coefficients:
    ## (Intercept)         year  
    ##   -507.5343       0.2753

Having the data and fit objects makes a few things much easier:

  - **Filtering**

<!-- end list -->

``` r
nested_df %>% 
  filter(continent == "Oceania") %>% 
  .$fit
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = lifeExp ~ year, data = .x)
    ## 
    ## Coefficients:
    ## (Intercept)         year  
    ##   -376.1163       0.2277  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = lifeExp ~ year, data = .x)
    ## 
    ## Coefficients:
    ## (Intercept)         year  
    ##   -307.6996       0.1928

  - **Extracting and including fit values**

<!-- end list -->

``` r
nested_df %>% 
  mutate(
    coefs = map(fit, coef),
    intercept = map_dbl(coefs, 1),
    slope = map_dbl(coefs, 2)
    ) %>% 
  select(country, continent, intercept, slope)
```

    ## # A tibble: 142 x 4
    ## # Groups:   country, continent [710]
    ##    country     continent intercept slope
    ##    <fct>       <fct>         <dbl> <dbl>
    ##  1 Afghanistan Asia          -508. 0.275
    ##  2 Albania     Europe        -594. 0.335
    ##  3 Algeria     Africa       -1068. 0.569
    ##  4 Angola      Africa        -377. 0.209
    ##  5 Argentina   Americas      -390. 0.232
    ##  6 Australia   Oceania       -376. 0.228
    ##  7 Austria     Europe        -406. 0.242
    ##  8 Bahrain     Asia          -860. 0.468
    ##  9 Bangladesh  Asia          -936. 0.498
    ## 10 Belgium     Europe        -340. 0.209
    ## # ... with 132 more rows

coef() is a function that grabs coefficients from a model object. So the
columns are created with maps that grab the coefficients from the fit
column.

Below it’s broken down into a mapping of the fits to get coefficients,
then a mapping of that returned list to get the numeric values.

  - **Get a list of coefficients**

<!-- end list -->

``` r
coef(nested_df$fit[[1]])
```

    ##  (Intercept)         year 
    ## -507.5342716    0.2753287

``` r
map(nested_df$fit, coef) %>% head(3)
```

    ## [[1]]
    ##  (Intercept)         year 
    ## -507.5342716    0.2753287 
    ## 
    ## [[2]]
    ##  (Intercept)         year 
    ## -594.0725110    0.3346832 
    ## 
    ## [[3]]
    ##   (Intercept)          year 
    ## -1067.8590396     0.5692797

  - **Get each intercept**

<!-- end list -->

``` r
map(nested_df$fit, coef) %>% head(3) %>% map_dbl(., 1)
```

    ## [1]  -507.5343  -594.0725 -1067.8590

  - **Get each slope**

<!-- end list -->

``` r
map(nested_df$fit, coef) %>% head(3) %>% map_dbl(., 2)
```

    ## [1] 0.2753287 0.3346832 0.5692797

-----
