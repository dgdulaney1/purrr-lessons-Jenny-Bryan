Intro to map() self-learn
================

  - [Setup](#setup)
  - [Use position and integer map()
    shortcuts](#use-position-and-integer-map-shortcuts)
  - [Use a type-specific map](#use-a-type-specific-map)
  - [Extract multiple values](#extract-multiple-values)
  - [Use `map_dfr()` to return a df instead of a nested
    list](#use-map_dfr-to-return-a-df-instead-of-a-nested-list)

# Setup

``` r
library(tidyverse)
```

    ## -- Attaching packages ------------------------ tidyverse 1.2.1 --

    ## v ggplot2 3.2.1     v purrr   0.3.3
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   1.0.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## -- Conflicts --------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(magrittr)
```

    ## 
    ## Attaching package: 'magrittr'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     set_names

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     extract

``` r
library(repurrrsive)

theme_set(theme_light())

knitr::opts_chunk$set(
  cache = TRUE,
  message = FALSE, 
  warning = FALSE, 
  fig.width = 10, 
  fig.height = 7
  )
```

Create list that can be manipulated throughout this.

``` r
div_list <- list(list(div = "NLE", teams = c("ATL", "WAS")),
                 list(div = "NLC", teams = c("PIT", "CHC")),
                 list(div = "NLW", teams = c("LAD", "SD"))
                  )
```

# Use position and integer map() shortcuts

``` r
div_list %>% 
  map("div")
```

    ## [[1]]
    ## [1] "NLE"
    ## 
    ## [[2]]
    ## [1] "NLC"
    ## 
    ## [[3]]
    ## [1] "NLW"

``` r
div_list %>% 
  map(2)
```

    ## [[1]]
    ## [1] "ATL" "WAS"
    ## 
    ## [[2]]
    ## [1] "PIT" "CHC"
    ## 
    ## [[3]]
    ## [1] "LAD" "SD"

# Use a type-specific map

``` r
div_list %>% 
  map_chr("div")
```

    ## [1] "NLE" "NLC" "NLW"

# Extract multiple values

``` r
div_list %>% 
  map(extract, c("div", "teams"))
```

    ## [[1]]
    ## [[1]]$div
    ## [1] "NLE"
    ## 
    ## [[1]]$teams
    ## [1] "ATL" "WAS"
    ## 
    ## 
    ## [[2]]
    ## [[2]]$div
    ## [1] "NLC"
    ## 
    ## [[2]]$teams
    ## [1] "PIT" "CHC"
    ## 
    ## 
    ## [[3]]
    ## [[3]]$div
    ## [1] "NLW"
    ## 
    ## [[3]]$teams
    ## [1] "LAD" "SD"

# Use `map_dfr()` to return a df instead of a nested list

``` r
div_list %>% 
  map_dfr(extract, c(1, 2))
```

    ## Error: Argument 2 must be length 1, not 2

For some reason, that call isn’t working? It seems the same as this, no?

``` r
got_chars %>% 
  map_dfr(extract, c(1, 2, 3, 4, 5))
```

    ## # A tibble: 30 x 5
    ##    url                                         id name            gender culture
    ##    <chr>                                    <int> <chr>           <chr>  <chr>  
    ##  1 https://www.anapioficeandfire.com/api/c~  1022 Theon Greyjoy   Male   Ironbo~
    ##  2 https://www.anapioficeandfire.com/api/c~  1052 Tyrion Lannist~ Male   ""     
    ##  3 https://www.anapioficeandfire.com/api/c~  1074 Victarion Grey~ Male   Ironbo~
    ##  4 https://www.anapioficeandfire.com/api/c~  1109 Will            Male   ""     
    ##  5 https://www.anapioficeandfire.com/api/c~  1166 Areo Hotah      Male   Norvos~
    ##  6 https://www.anapioficeandfire.com/api/c~  1267 Chett           Male   ""     
    ##  7 https://www.anapioficeandfire.com/api/c~  1295 Cressen         Male   ""     
    ##  8 https://www.anapioficeandfire.com/api/c~   130 Arianne Martell Female Dornish
    ##  9 https://www.anapioficeandfire.com/api/c~  1303 Daenerys Targa~ Female Valyri~
    ## 10 https://www.anapioficeandfire.com/api/c~  1319 Davos Seaworth  Male   Wester~
    ## # ... with 20 more rows
