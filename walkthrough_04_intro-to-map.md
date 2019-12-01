Intro to map() walkthrough
================

  - [Setup](#setup)
  - [Name and position shortcuts](#name-and-position-shortcuts)
      - [Exercises](#exercises)
  - [Type-specific map](#type-specific-map)
      - [Exercises](#exercises-1)
  - [Extract multiple values](#extract-multiple-values)
      - [Exercises](#exercises-2)
  - [Data frame output](#data-frame-output)
      - [Exercises](#exercises-3)

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
library(repurrrsive)
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
theme_set(theme_light())

knitr::opts_chunk$set(
  message = FALSE, 
  warning = FALSE, 
  fig.width = 10, 
  fig.height = 7
  )
```

-----

`purrr:map()` is used to apply a function to each element of a list

``` r
map(1:5, sqrt)
```

    ## [[1]]
    ## [1] 1
    ## 
    ## [[2]]
    ## [1] 1.414214
    ## 
    ## [[3]]
    ## [1] 1.732051
    ## 
    ## [[4]]
    ## [1] 2
    ## 
    ## [[5]]
    ## [1] 2.236068

So for each element 1, 2, …, 5, the `sqrt()` function is applied and a
list of each `sqrt(.)` is returned.

# Name and position shortcuts

`purrr:map` functions have shortcuts that allow the extraction of
elemennts based on name or position.

``` r
str(got_chars[1:4], list.len = 3)
```

    ## List of 4
    ##  $ :List of 18
    ##   ..$ url        : chr "https://www.anapioficeandfire.com/api/characters/1022"
    ##   ..$ id         : int 1022
    ##   ..$ name       : chr "Theon Greyjoy"
    ##   .. [list output truncated]
    ##  $ :List of 18
    ##   ..$ url        : chr "https://www.anapioficeandfire.com/api/characters/1052"
    ##   ..$ id         : int 1052
    ##   ..$ name       : chr "Tyrion Lannister"
    ##   .. [list output truncated]
    ##  $ :List of 18
    ##   ..$ url        : chr "https://www.anapioficeandfire.com/api/characters/1074"
    ##   ..$ id         : int 1074
    ##   ..$ name       : chr "Victarion Greyjoy"
    ##   .. [list output truncated]
    ##   [list output truncated]

For each sublist in `got_chars`, there is a `$name` element

``` r
map(got_chars[1:4], "name")
```

    ## [[1]]
    ## [1] "Theon Greyjoy"
    ## 
    ## [[2]]
    ## [1] "Tyrion Lannister"
    ## 
    ## [[3]]
    ## [1] "Victarion Greyjoy"
    ## 
    ## [[4]]
    ## [1] "Will"

An integer can be provided instead of a name

``` r
map(got_chars[1:4], 3)
```

    ## [[1]]
    ## [1] "Theon Greyjoy"
    ## 
    ## [[2]]
    ## [1] "Tyrion Lannister"
    ## 
    ## [[3]]
    ## [1] "Victarion Greyjoy"
    ## 
    ## [[4]]
    ## [1] "Will"

And it’s easy to use this functionality in a pipe

``` r
got_chars[1:4] %>% 
  map("name")
```

    ## [[1]]
    ## [1] "Theon Greyjoy"
    ## 
    ## [[2]]
    ## [1] "Tyrion Lannister"
    ## 
    ## [[3]]
    ## [1] "Victarion Greyjoy"
    ## 
    ## [[4]]
    ## [1] "Will"

## Exercises

1.  Use names() to inspect the names of the list elements associated
    with a single character. What is the index or position of the
    playedBy element? Use the character and position shortcuts to
    extract the playedBy elements for all characters.

<!-- end list -->

``` r
names(got_chars[[1]])
```

    ##  [1] "url"         "id"          "name"        "gender"      "culture"    
    ##  [6] "born"        "died"        "alive"       "titles"      "aliases"    
    ## [11] "father"      "mother"      "spouse"      "allegiances" "books"      
    ## [16] "povBooks"    "tvSeries"    "playedBy"

playedBy is the 18th element.

``` r
got_chars %>% 
  map("playedBy")
```

    ## [[1]]
    ## [1] "Alfie Allen"
    ## 
    ## [[2]]
    ## [1] "Peter Dinklage"
    ## 
    ## [[3]]
    ## [1] ""
    ## 
    ## [[4]]
    ## [1] "Bronson Webb"
    ## 
    ## [[5]]
    ## [1] "DeObia Oparei"
    ## 
    ## [[6]]
    ## [1] ""
    ## 
    ## [[7]]
    ## [1] "Oliver Ford"
    ## 
    ## [[8]]
    ## [1] ""
    ## 
    ## [[9]]
    ## [1] "Emilia Clarke"
    ## 
    ## [[10]]
    ## [1] "Liam Cunningham"
    ## 
    ## [[11]]
    ## [1] "Maisie Williams"
    ## 
    ## [[12]]
    ## [1] ""
    ## 
    ## [[13]]
    ## [1] "Gemma Whelan"
    ## 
    ## [[14]]
    ## [1] "Ian McElhinney"
    ## 
    ## [[15]]
    ## [1] ""
    ## 
    ## [[16]]
    ## [1] "Isaac Hempstead-Wright"
    ## 
    ## [[17]]
    ## [1] "Gwendoline Christie"
    ## 
    ## [[18]]
    ## [1] "Michelle Fairley"
    ## 
    ## [[19]]
    ## [1] "Lena Headey"
    ## 
    ## [[20]]
    ## [1] "Sean Bean"       "Sebastian Croft" "Robert Aramayo" 
    ## 
    ## [[21]]
    ## [1] "Nikolaj Coster-Waldau"
    ## 
    ## [[22]]
    ## [1] ""
    ## 
    ## [[23]]
    ## [1] "Kit Harington"
    ## 
    ## [[24]]
    ## [1] "Michael Feast"
    ## 
    ## [[25]]
    ## [1] "Ian Gelder"
    ## 
    ## [[26]]
    ## [1] "Carice van Houten"
    ## 
    ## [[27]]
    ## [1] ""
    ## 
    ## [[28]]
    ## [1] ""
    ## 
    ## [[29]]
    ## [1] "John Bradley-West"
    ## 
    ## [[30]]
    ## [1] "Sophie Turner"

``` r
got_chars %>% 
  map(18)
```

    ## [[1]]
    ## [1] "Alfie Allen"
    ## 
    ## [[2]]
    ## [1] "Peter Dinklage"
    ## 
    ## [[3]]
    ## [1] ""
    ## 
    ## [[4]]
    ## [1] "Bronson Webb"
    ## 
    ## [[5]]
    ## [1] "DeObia Oparei"
    ## 
    ## [[6]]
    ## [1] ""
    ## 
    ## [[7]]
    ## [1] "Oliver Ford"
    ## 
    ## [[8]]
    ## [1] ""
    ## 
    ## [[9]]
    ## [1] "Emilia Clarke"
    ## 
    ## [[10]]
    ## [1] "Liam Cunningham"
    ## 
    ## [[11]]
    ## [1] "Maisie Williams"
    ## 
    ## [[12]]
    ## [1] ""
    ## 
    ## [[13]]
    ## [1] "Gemma Whelan"
    ## 
    ## [[14]]
    ## [1] "Ian McElhinney"
    ## 
    ## [[15]]
    ## [1] ""
    ## 
    ## [[16]]
    ## [1] "Isaac Hempstead-Wright"
    ## 
    ## [[17]]
    ## [1] "Gwendoline Christie"
    ## 
    ## [[18]]
    ## [1] "Michelle Fairley"
    ## 
    ## [[19]]
    ## [1] "Lena Headey"
    ## 
    ## [[20]]
    ## [1] "Sean Bean"       "Sebastian Croft" "Robert Aramayo" 
    ## 
    ## [[21]]
    ## [1] "Nikolaj Coster-Waldau"
    ## 
    ## [[22]]
    ## [1] ""
    ## 
    ## [[23]]
    ## [1] "Kit Harington"
    ## 
    ## [[24]]
    ## [1] "Michael Feast"
    ## 
    ## [[25]]
    ## [1] "Ian Gelder"
    ## 
    ## [[26]]
    ## [1] "Carice van Houten"
    ## 
    ## [[27]]
    ## [1] ""
    ## 
    ## [[28]]
    ## [1] ""
    ## 
    ## [[29]]
    ## [1] "John Bradley-West"
    ## 
    ## [[30]]
    ## [1] "Sophie Turner"

2.  What happens if you use the character shortcut with a string that
    does not appear in the lists’ names?

<!-- end list -->

``` r
got_chars %>% 
  map(";alskdjf")
```

    ## [[1]]
    ## NULL
    ## 
    ## [[2]]
    ## NULL
    ## 
    ## [[3]]
    ## NULL
    ## 
    ## [[4]]
    ## NULL
    ## 
    ## [[5]]
    ## NULL
    ## 
    ## [[6]]
    ## NULL
    ## 
    ## [[7]]
    ## NULL
    ## 
    ## [[8]]
    ## NULL
    ## 
    ## [[9]]
    ## NULL
    ## 
    ## [[10]]
    ## NULL
    ## 
    ## [[11]]
    ## NULL
    ## 
    ## [[12]]
    ## NULL
    ## 
    ## [[13]]
    ## NULL
    ## 
    ## [[14]]
    ## NULL
    ## 
    ## [[15]]
    ## NULL
    ## 
    ## [[16]]
    ## NULL
    ## 
    ## [[17]]
    ## NULL
    ## 
    ## [[18]]
    ## NULL
    ## 
    ## [[19]]
    ## NULL
    ## 
    ## [[20]]
    ## NULL
    ## 
    ## [[21]]
    ## NULL
    ## 
    ## [[22]]
    ## NULL
    ## 
    ## [[23]]
    ## NULL
    ## 
    ## [[24]]
    ## NULL
    ## 
    ## [[25]]
    ## NULL
    ## 
    ## [[26]]
    ## NULL
    ## 
    ## [[27]]
    ## NULL
    ## 
    ## [[28]]
    ## NULL
    ## 
    ## [[29]]
    ## NULL
    ## 
    ## [[30]]
    ## NULL

A NULLED list of equal length to the outermost list is returned.

3.  What happens if you use the position shortcut with a number greater
    than the length of the lists?

<!-- end list -->

``` r
got_chars %>% 
  map(50)
```

    ## [[1]]
    ## NULL
    ## 
    ## [[2]]
    ## NULL
    ## 
    ## [[3]]
    ## NULL
    ## 
    ## [[4]]
    ## NULL
    ## 
    ## [[5]]
    ## NULL
    ## 
    ## [[6]]
    ## NULL
    ## 
    ## [[7]]
    ## NULL
    ## 
    ## [[8]]
    ## NULL
    ## 
    ## [[9]]
    ## NULL
    ## 
    ## [[10]]
    ## NULL
    ## 
    ## [[11]]
    ## NULL
    ## 
    ## [[12]]
    ## NULL
    ## 
    ## [[13]]
    ## NULL
    ## 
    ## [[14]]
    ## NULL
    ## 
    ## [[15]]
    ## NULL
    ## 
    ## [[16]]
    ## NULL
    ## 
    ## [[17]]
    ## NULL
    ## 
    ## [[18]]
    ## NULL
    ## 
    ## [[19]]
    ## NULL
    ## 
    ## [[20]]
    ## NULL
    ## 
    ## [[21]]
    ## NULL
    ## 
    ## [[22]]
    ## NULL
    ## 
    ## [[23]]
    ## NULL
    ## 
    ## [[24]]
    ## NULL
    ## 
    ## [[25]]
    ## NULL
    ## 
    ## [[26]]
    ## NULL
    ## 
    ## [[27]]
    ## NULL
    ## 
    ## [[28]]
    ## NULL
    ## 
    ## [[29]]
    ## NULL
    ## 
    ## [[30]]
    ## NULL

Same as with the non-name.

4.  What if these shortcuts did not exist? Write a function that takes a
    list and a string as input and returns the list element that bears
    the name in the string. Apply this to got\_chars via map(). Do you
    get the same result as with the shortcut? Reflect on code length and
    readability.

<!-- end list -->

``` r
map_knockoff_str <- function(l, s) l[[s]]
  
sapply(got_chars[1:4], map_knockoff_str, s = "name")
```

    ## [1] "Theon Greyjoy"     "Tyrion Lannister"  "Victarion Greyjoy"
    ## [4] "Will"

Less readable\! Redundant if it’s such a common use case.

5.  Write another function that takes a list and an integer as input and
    returns the list element at that position. Apply this to got\_chars
    via map(). How does this result and process compare with the
    shortcut?

<!-- end list -->

``` r
map_knockoff_int <- function(l, i) l[[i]]
  
sapply(got_chars[1:4], map_knockoff_int, i = 3)
```

    ## [1] "Theon Greyjoy"     "Tyrion Lannister"  "Victarion Greyjoy"
    ## [4] "Will"

-----

# Type-specific map

Variants of map() like map\_chr() will return from a map call the exact
type of object you want.

``` r
got_chars %>% 
  map_chr("name")
```

    ##  [1] "Theon Greyjoy"      "Tyrion Lannister"   "Victarion Greyjoy" 
    ##  [4] "Will"               "Areo Hotah"         "Chett"             
    ##  [7] "Cressen"            "Arianne Martell"    "Daenerys Targaryen"
    ## [10] "Davos Seaworth"     "Arya Stark"         "Arys Oakheart"     
    ## [13] "Asha Greyjoy"       "Barristan Selmy"    "Varamyr"           
    ## [16] "Brandon Stark"      "Brienne of Tarth"   "Catelyn Stark"     
    ## [19] "Cersei Lannister"   "Eddard Stark"       "Jaime Lannister"   
    ## [22] "Jon Connington"     "Jon Snow"           "Aeron Greyjoy"     
    ## [25] "Kevan Lannister"    "Melisandre"         "Merrett Frey"      
    ## [28] "Quentyn Martell"    "Samwell Tarly"      "Sansa Stark"

A character vector is returned instead of a list

## Exercises

1.  For each character, the second element is named “id”. This is the
    character’s id in the API Of Ice And Fire. Use a type-specific form
    of map() and an extraction shortcut to extract these ids into an
    integer vector.

<!-- end list -->

``` r
got_chars %>% 
  map_int("id")
```

    ##  [1] 1022 1052 1074 1109 1166 1267 1295  130 1303 1319  148  149  150  168 2066
    ## [16]  208  216  232  238  339  529  576  583   60  605  743  751  844  954  957

2.  Use your list inspection strategies to find the list element that is
    logical. There is one\! Use a type-specific form of map() and an
    extraction shortcut to extract these values for all characters into
    a logical vector.

<!-- end list -->

``` r
got_chars %>% 
  map_lgl("alive")
```

    ##  [1]  TRUE  TRUE  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE
    ## [13]  TRUE  TRUE FALSE  TRUE  TRUE FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE
    ## [25] FALSE  TRUE FALSE FALSE  TRUE  TRUE

Like how `map_chr()` returns a character vector, `map_lgl()` returns a
logical vector.

-----

# Extract multiple values

Sometimes instead of just retrieving a single value from each list
element (e.g. “name”) we want multiple values (e.g. “name”, “culture”,
and “gender”).

For a single character, we can use

``` r
got_chars[[3]][c("name", "culture", "gender")]
```

    ## $name
    ## [1] "Victarion Greyjoy"
    ## 
    ## $culture
    ## [1] "Ironborn"
    ## 
    ## $gender
    ## [1] "Male"

And with `map()`, we have a few options for multiple value extraction:

  - **Using `[`**

<!-- end list -->

``` r
got_chars[1:4] %>% 
  map(`[`, c("name", "culture", "gender"))
```

    ## [[1]]
    ## [[1]]$name
    ## [1] "Theon Greyjoy"
    ## 
    ## [[1]]$culture
    ## [1] "Ironborn"
    ## 
    ## [[1]]$gender
    ## [1] "Male"
    ## 
    ## 
    ## [[2]]
    ## [[2]]$name
    ## [1] "Tyrion Lannister"
    ## 
    ## [[2]]$culture
    ## [1] ""
    ## 
    ## [[2]]$gender
    ## [1] "Male"
    ## 
    ## 
    ## [[3]]
    ## [[3]]$name
    ## [1] "Victarion Greyjoy"
    ## 
    ## [[3]]$culture
    ## [1] "Ironborn"
    ## 
    ## [[3]]$gender
    ## [1] "Male"
    ## 
    ## 
    ## [[4]]
    ## [[4]]$name
    ## [1] "Will"
    ## 
    ## [[4]]$culture
    ## [1] ""
    ## 
    ## [[4]]$gender
    ## [1] "Male"

  - **Using `extract()`**

<!-- end list -->

``` r
got_chars[1:4] %>% 
  map(extract, c("name", "culture", "gender"))
```

    ## [[1]]
    ## [[1]]$name
    ## [1] "Theon Greyjoy"
    ## 
    ## [[1]]$culture
    ## [1] "Ironborn"
    ## 
    ## [[1]]$gender
    ## [1] "Male"
    ## 
    ## 
    ## [[2]]
    ## [[2]]$name
    ## [1] "Tyrion Lannister"
    ## 
    ## [[2]]$culture
    ## [1] ""
    ## 
    ## [[2]]$gender
    ## [1] "Male"
    ## 
    ## 
    ## [[3]]
    ## [[3]]$name
    ## [1] "Victarion Greyjoy"
    ## 
    ## [[3]]$culture
    ## [1] "Ironborn"
    ## 
    ## [[3]]$gender
    ## [1] "Male"
    ## 
    ## 
    ## [[4]]
    ## [[4]]$name
    ## [1] "Will"
    ## 
    ## [[4]]$culture
    ## [1] ""
    ## 
    ## [[4]]$gender
    ## [1] "Male"

## Exercises

1.  Use your list inspection skills to determine the position of the
    elements named “name”, “gender”, “culture”, “born”, and “died”. Map
    `[` or `magrittr::extract()` over users, requesting these elements
    by position instead of name.

<!-- end list -->

``` r
got_chars[[1]] %>% 
  names(.)
```

    ##  [1] "url"         "id"          "name"        "gender"      "culture"    
    ##  [6] "born"        "died"        "alive"       "titles"      "aliases"    
    ## [11] "father"      "mother"      "spouse"      "allegiances" "books"      
    ## [16] "povBooks"    "tvSeries"    "playedBy"

``` r
got_chars[1:4] %>% 
  map(extract, c(3, 4, 5, 6, 7))
```

    ## [[1]]
    ## [[1]]$name
    ## [1] "Theon Greyjoy"
    ## 
    ## [[1]]$gender
    ## [1] "Male"
    ## 
    ## [[1]]$culture
    ## [1] "Ironborn"
    ## 
    ## [[1]]$born
    ## [1] "In 278 AC or 279 AC, at Pyke"
    ## 
    ## [[1]]$died
    ## [1] ""
    ## 
    ## 
    ## [[2]]
    ## [[2]]$name
    ## [1] "Tyrion Lannister"
    ## 
    ## [[2]]$gender
    ## [1] "Male"
    ## 
    ## [[2]]$culture
    ## [1] ""
    ## 
    ## [[2]]$born
    ## [1] "In 273 AC, at Casterly Rock"
    ## 
    ## [[2]]$died
    ## [1] ""
    ## 
    ## 
    ## [[3]]
    ## [[3]]$name
    ## [1] "Victarion Greyjoy"
    ## 
    ## [[3]]$gender
    ## [1] "Male"
    ## 
    ## [[3]]$culture
    ## [1] "Ironborn"
    ## 
    ## [[3]]$born
    ## [1] "In 268 AC or before, at Pyke"
    ## 
    ## [[3]]$died
    ## [1] ""
    ## 
    ## 
    ## [[4]]
    ## [[4]]$name
    ## [1] "Will"
    ## 
    ## [[4]]$gender
    ## [1] "Male"
    ## 
    ## [[4]]$culture
    ## [1] ""
    ## 
    ## [[4]]$born
    ## [1] ""
    ## 
    ## [[4]]$died
    ## [1] "In 297 AC, at Haunted Forest"

-----

# Data frame output

`map_dfr()` allows us to extract multiple values *and* stack the results
in a data frame instead of keeping it as a nested list.

``` r
got_chars[1:4] %>% 
  map_dfr(extract, c("name", "culture", "gender"))
```

    ## # A tibble: 4 x 3
    ##   name              culture  gender
    ##   <chr>             <chr>    <chr> 
    ## 1 Theon Greyjoy     Ironborn Male  
    ## 2 Tyrion Lannister  ""       Male  
    ## 3 Victarion Greyjoy Ironborn Male  
    ## 4 Will              ""       Male

The variable types in the resulting df are automatically type converted.
This can be problematic sometimes, so it’s good practice to explicitly
type variables.

``` r
got_chars %>% {
  
  tibble(
    name = map_chr(., "name"),
    culture = map_chr(., "culture"),
    gender = map_chr(., "gender"),
    id = map_int(., "id"),
    born = map_chr(., "born"),
    alive = map_lgl(., "alive")
  )
}
```

    ## # A tibble: 30 x 6
    ##    name             culture  gender    id born                             alive
    ##    <chr>            <chr>    <chr>  <int> <chr>                            <lgl>
    ##  1 Theon Greyjoy    Ironborn Male    1022 In 278 AC or 279 AC, at Pyke     TRUE 
    ##  2 Tyrion Lannister ""       Male    1052 In 273 AC, at Casterly Rock      TRUE 
    ##  3 Victarion Greyj~ Ironborn Male    1074 In 268 AC or before, at Pyke     TRUE 
    ##  4 Will             ""       Male    1109 ""                               FALSE
    ##  5 Areo Hotah       Norvoshi Male    1166 In 257 AC or before, at Norvos   TRUE 
    ##  6 Chett            ""       Male    1267 At Hag's Mire                    FALSE
    ##  7 Cressen          ""       Male    1295 In 219 AC or 220 AC              FALSE
    ##  8 Arianne Martell  Dornish  Female   130 In 276 AC, at Sunspear           TRUE 
    ##  9 Daenerys Targar~ Valyrian Female  1303 In 284 AC, at Dragonstone        TRUE 
    ## 10 Davos Seaworth   Westeros Male    1319 In 260 AC or before, at King's ~ TRUE 
    ## # ... with 20 more rows

The curly brackets `{}` call prevents `got_chars` from being passed in
as the first argument of `tibble()`.

## Exercises

1.  Use `map_dfr()` to create the same data frame as above, but indexing
    with a vector of positive integers instead of names.

<!-- end list -->

``` r
got_chars[1:4] %>%
  map_dfr(extract, c(3, 5, 4, 2, 6, 8))
```

    ## # A tibble: 4 x 6
    ##   name              culture  gender    id born                         alive
    ##   <chr>             <chr>    <chr>  <int> <chr>                        <lgl>
    ## 1 Theon Greyjoy     Ironborn Male    1022 In 278 AC or 279 AC, at Pyke TRUE 
    ## 2 Tyrion Lannister  ""       Male    1052 In 273 AC, at Casterly Rock  TRUE 
    ## 3 Victarion Greyjoy Ironborn Male    1074 In 268 AC or before, at Pyke TRUE 
    ## 4 Will              ""       Male    1109 ""                           FALSE

-----
