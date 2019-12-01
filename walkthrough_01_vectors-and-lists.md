Vectors and lists
================

  - [Atomic vectors](#atomic-vectors)
      - [Excercises](#excercises)
      - [Exercises](#exercises)
  - [Coercion](#coercion)
      - [Exercises](#exercises-1)
  - [Lists](#lists)
      - [Exercises](#exercises-2)
  - [List indexing](#list-indexing)
      - [Exercises](#exercises-3)
  - [Vectorized operations](#vectorized-operations)

Working through [this
section](https://jennybc.github.io/purrr-tutorial/bk00_vectors-and-lists.html)
of the purrr tutorials

``` r
library(tidyverse)
```

    ## -- Attaching packages ----------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.2.1     v purrr   0.3.3
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   1.0.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## -- Conflicts -------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

-----

Walking through section

## Atomic vectors

Atomic vectors are homogenous

``` r
v_log <- c(TRUE, FALSE, FALSE, TRUE)
v_int <- 1:4
v_doub <- 1:4 * 1.2
v_char <- letters[1:4]
```

### Excercises

1.  Define the vectors above or similar. Use the family of is.\_()
    functions to confirm vector type, e.g. is.logical(). You will need
    to guess or look some of them up. Long-term, you may wish to explore
    the rlang::is\_() family of functions.

<!-- end list -->

``` r
is.character(v_char)
```

    ## [1] TRUE

``` r
is.numeric(v_doub)
```

    ## [1] TRUE

``` r
is.integer(v_int)
```

    ## [1] TRUE

``` r
is.logical(v_log)
```

    ## [1] TRUE

2.  What do is.numeric(), is.integer(), and is.double() return for the
    vectors that hold floating point number versus integers?

<!-- end list -->

``` r
is.numeric(v_doub)
```

    ## [1] TRUE

``` r
is.numeric(v_int)
```

    ## [1] TRUE

``` r
is.integer(v_doub) # FALSE
```

    ## [1] FALSE

``` r
is.integer(v_int)
```

    ## [1] TRUE

``` r
is.double(v_doub)
```

    ## [1] TRUE

``` r
is.double(v_int) # FALSE
```

    ## [1] FALSE

Can index with a logical vector or with integers

``` r
v_char[c(FALSE, FALSE, TRUE, TRUE)]
```

    ## [1] "c" "d"

``` r
v_char[v_log]
```

    ## [1] "a" "d"

``` r
v_char[2:3]
```

    ## [1] "b" "c"

``` r
v_char[-4]
```

    ## [1] "a" "b" "c"

### Exercises

1.  What happens when you request the zero-th element of one of our
    vectors?

<!-- end list -->

``` r
v_char[0]
```

    ## character(0)

An atomic vector of length 0.

2.  What happens when you ask for an element that is past the end of the
    vector, i.e. request x\[k\] when the length of x is less than k?

<!-- end list -->

``` r
v_doub[50]
```

    ## [1] NA

3.  We indexed a vector x with a vector of positive integers that is
    shorter than x. What happens if the indexing vector is longer than
    x?

<!-- end list -->

``` r
v_char[1:50]
```

    ##  [1] "a" "b" "c" "d" NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA 
    ## [18] NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA 
    ## [35] NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA

4.  We indexed x with a logical vector of the same length. What happen
    if the indexing vector is shorter than x?

<!-- end list -->

``` r
v_doub[c(TRUE, FALSE)]
```

    ## [1] 1.2 3.6

-----

## Coercion

Use as.\_() for explicit coerction

``` r
as.integer(v_log)
```

    ## [1] 1 0 0 1

``` r
as.numeric(v_int)
```

    ## [1] 1 2 3 4

``` r
as.character(v_doub)
```

    ## [1] "1.2" "2.4" "3.6" "4.8"

Implicit coercion can be triggered from other actions as well.

``` r
v_doub_copy <- v_doub

v_doub_copy[3] <- "uhoh"
str(v_doub_copy)
```

    ##  chr [1:4] "1.2" "2.4" "uhoh" "4.8"

Be intentional about type\!

``` r
big_plans <- rep(NA_integer_, 4)
str(big_plans)
```

    ##  int [1:4] NA NA NA NA

``` r
big_plans[3] <- 5L # L implies double instead of int
str(big_plans)
```

    ##  int [1:4] NA NA 5 NA

### Exercises

1.  Recall the hieararchy of the most common atomic vector types:
    logical \< integer \< numeric \< character. Try to use the as.\*()
    functions to go the wrong way. Call as.logical(), as.integer(), and
    as.numeric() on a character vector, such as letters. What happens?

<!-- end list -->

``` r
as.logical(letters[1:4])
```

    ## [1] NA NA NA NA

``` r
as.integer(letters[1:4])
```

    ## Warning: NAs introduced by coercion

    ## [1] NA NA NA NA

``` r
as.numeric(letters[1:4])
```

    ## Warning: NAs introduced by coercion

    ## [1] NA NA NA NA

So can make anything a character vector, can make anything logical &
integer a numeric vector, but (mostly) can’t make a character vector
logical, integer, or numeric.

-----

## Lists

Lists are useful if you want:

  - An individual atom to be longer than length 1 (i.e. a sublist)
  - Individual atoms of different type

<!-- end list -->

``` r
x <- list(1:3, c("four", "five"))
x
```

    ## [[1]]
    ## [1] 1 2 3
    ## 
    ## [[2]]
    ## [1] "four" "five"

``` r
y <- list(letters[26:22], transcendental = c(pi, exp(1)), f = function(x) x^2)
y
```

    ## [[1]]
    ## [1] "z" "y" "x" "w" "v"
    ## 
    ## $transcendental
    ## [1] 3.141593 2.718282
    ## 
    ## $f
    ## function(x) x^2

### Exercises

1.  Make the lists x, y, and z as shown above. Use is.\_() functions to
    get to know these objects. Try to get some positive and negative
    results, i.e. establish a few things that x is and is NOT. Make sure
    to try is.list(), is.vector(), is.atomic(), and is.recursive().
    Long-term, you may wish to explore the rlang::is.\_() family of
    functions.

<!-- end list -->

``` r
x
```

    ## [[1]]
    ## [1] 1 2 3
    ## 
    ## [[2]]
    ## [1] "four" "five"

``` r
is.integer(x)
```

    ## [1] FALSE

``` r
is.integer(x[[1]])
```

    ## [1] TRUE

``` r
is.list(x)
```

    ## [1] TRUE

``` r
is.vector(x)
```

    ## [1] TRUE

``` r
is.atomic(x)
```

    ## [1] FALSE

``` r
is.recursive(x)
```

    ## [1] TRUE

-----

## List indexing

3 ways to index a list:

  - With a single bracket

<!-- end list -->

``` r
x
```

    ## [[1]]
    ## [1] 1 2 3
    ## 
    ## [[2]]
    ## [1] "four" "five"

``` r
x[1]
```

    ## [[1]]
    ## [1] 1 2 3

``` r
x[2]
```

    ## [[1]]
    ## [1] "four" "five"

This will return the specified element, but will always be a list

  - With double square brackets

<!-- end list -->

``` r
x[[1]]
```

    ## [1] 1 2 3

``` r
y[["transcendental"]]
```

    ## [1] 3.141593 2.718282

This returns the “naked” element.

  - With the $

<!-- end list -->

``` r
y$transcendental
```

    ## [1] 3.141593 2.718282

Must specify the component by name with $.

### Exercises

1.  Use \[, \[\[, and $ to access the second component of the list z,
    which bears the name “transcendental”. Use the length() and the
    is.\*() functions explored elsewhere to study the result. Which
    methods of indexing yield the same result vs. different?

<!-- end list -->

``` r
y
```

    ## [[1]]
    ## [1] "z" "y" "x" "w" "v"
    ## 
    ## $transcendental
    ## [1] 3.141593 2.718282
    ## 
    ## $f
    ## function(x) x^2

``` r
single <- y[2]
double <- y[[2]]
dollar <- y$transcendental

length(single)
```

    ## [1] 1

``` r
length(double)
```

    ## [1] 2

``` r
length(dollar)
```

    ## [1] 2

``` r
is.list(single)
```

    ## [1] TRUE

``` r
is.list(double)
```

    ## [1] FALSE

``` r
is.atomic(dollar)
```

    ## [1] TRUE

2.  Put the same data into an atomic vector and a list:

<!-- end list -->

``` r
my_vec <- c(a = 1, b = 2, c = 3)
my_list <- list(a = 1, b = 2, c = 3)
```

Use \[ and \[\[ to attempt to retrieve elements 2 and 3 from my\_vec and
my\_list. What succeeds vs. fails? What if you try to retrieve element 2
alone? Does \[\[ even work on atomic vectors? Compare and contrast the
results from the various combinations of indexing method and input
object

``` r
my_vec[c(2, 3)]
```

    ## b c 
    ## 2 3

``` r
my_vec[[c(2, 3)]]
```

    ## Error in my_vec[[c(2, 3)]]: attempt to select more than one element in vectorIndex

-----

## Vectorized operations

Brute force way to square elements in a vector:

``` r
n <- 5
res <- rep(NA_integer_, n)

for(i in seq_len(n)){
  res[i] <- i ^ 2
}

res
```

    ## [1]  1  4  9 16 25

The R way:

``` r
seq_len(n) ^ 2
```

    ## [1]  1  4  9 16 25

But this is not possible with lists

``` r
l_doub <- as.list(v_doub)

exp(l_doub)
```

    ## Error in exp(l_doub): non-numeric argument to mathematical function

So how do we apply a function to each element of a list? By using
purrr:map()\!

``` r
map(l_doub, exp)
```

    ## [[1]]
    ## [1] 3.320117
    ## 
    ## [[2]]
    ## [1] 11.02318
    ## 
    ## [[3]]
    ## [1] 36.59823
    ## 
    ## [[4]]
    ## [1] 121.5104
