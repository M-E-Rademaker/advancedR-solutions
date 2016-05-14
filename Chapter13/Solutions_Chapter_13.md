Solutions to Chapter 13
================
05 Mai, 2016

-   [13.1.1 Exercises](#exercises)

13.1.1 Exercises
----------------

> **Question 1**: One important feature of `deparse()` to be aware of when programming is that it can return multiple strings if the input is too long. For example, the following call produces a vector of length two:

``` r
g(a + b + c + d + e + f + g + h + i + j + k + l + m + n + o + p + q + r + s + t
  + u + v + w + x + y + z)
```

> \[where `g()` was defined as: `function(x) deparse(substitute(x))`\] Why does this happen? Carefully read the documentation. Can you write a wrapper around `deparse()` so that it always returns a single string?

The documentation tells us that the default behaviour of deparse is to try and break the expression to deparse into seperate strings if the output is longer than `width.cutoff = 60` **bytes**. In the above case:

``` r
g <- function(x) deparse(substitute(x))

g(a + b + c + d + e + f + g + h + i + j + k + l + m + n + o + p + q + r + s + t
  + u + v + w + x + y + z)
```

    ## [1] "a + b + c + d + e + f + g + h + i + j + k + l + m + n + o + p + "
    ## [2] "    q + r + s + t + u + v + w + x + y + z"

To find out the number of bytes a given expression contains we use (see [this](http://stackoverflow.com/questions/36969133/what-does-ncharsubstitutea-b-c-actually-count/36969146?noredirect=1#comment61494207_36969146) StackOverflow question)

``` r
sum(nchar(substitute(a + b + c + d + e + f + g + h + i + j + k + l + m + n + o 
                     + p + q + r + s + t + u + v + w + x + y + z), type = "bytes"))
```

    ## [1] 99

So if we set `width.cutoff = 100` the output wont be split

``` r
deparse(substitute(a + b + c + d + e + f + g + h + i + j + k + l + m + n + o 
                     + p + q + r + s + t + u + v + w + x + y + z), width.cutoff = 100)
```

    ## [1] "a + b + c + d + e + f + g + h + i + j + k + l + m + n + o + p + q + r + s + t + u + v + w + x + y + z"

A general wrapper could use this idea

``` r
deparse2 <- function(x) {
  l <- sum(nchar(x))
  deparse(x, width.cutoff = l + 1)
}

deparse2(substitute(a + b + c + d + e + f + g + h + i + j + k + l + m + n + o 
                     + p + q + r + s + t + u + v + w + x + y + z ))
```

    ## [1] "a + b + c + d + e + f + g + h + i + j + k + l + m + n + o + p + q + r + s + t + u + v + w + x + y + z"

------------------------------------------------------------------------

> **Question 2**: Why does `as.Date.default()` use `substitute()` and `deparse()`? Why does `pairwise.t.test()` use them?´Read the source code.

``` r
as.Date.default
```

    ## function (x, ...) 
    ## {
    ##     if (inherits(x, "Date")) 
    ##         return(x)
    ##     if (is.logical(x) && all(is.na(x))) 
    ##         return(structure(as.numeric(x), class = "Date"))
    ##     stop(gettextf("do not know how to convert '%s' to class %s", 
    ##         deparse(substitute(x)), dQuote("Date")), domain = NA)
    ## }
    ## <bytecode: 0x0000000015ce52c8>
    ## <environment: namespace:base>

The function uses `substitute()` and `deparse()` to print a meaningful error message

``` r
as.Date.default("abc")
```

    ## Error in as.Date.default("abc"): do not know how to convert '"abc"' to class "Date"

The function `pairwise.t.test()` uses `substitute()` and `deparse()` to generate labels for the resulting output.

``` r
attach(airquality)
Month <- factor(Month, labels = month.abb[5:9])
pairwise.t.test(Ozone, Month)
```

    ## 
    ##  Pairwise comparisons using t tests with pooled SD 
    ## 
    ## data:  Ozone and Month 
    ## 
    ##     May     Jun     Jul     Aug    
    ## Jun 1.00000 -       -       -      
    ## Jul 0.00026 0.05113 -       -      
    ## Aug 0.00019 0.04987 1.00000 -      
    ## Sep 1.00000 1.00000 0.00488 0.00388
    ## 
    ## P value adjustment method: holm

------------------------------------------------------------------------

> **Question 3**: `pairwise.t.test()` assumes that `deparse()` always returns a length one character vector. Can you construct an input that violates this expectation? What happens?

To be done

> **Question 4**: `f()`, defined above \[`f <- function(x) substitute(x)`\] just calls `substitute()` Why can't we use it to define `g()`. In other words, what will the following code return? First make a prediction. Then run the code and think about the results.

``` r
f <- function(x, y) substitute(x)
g <- function(x) deparse(f(x))
g(1:10) 
```

    ## [1] "x"

``` r
g(x) 
```

    ## [1] "x"

``` r
g(x + y^2 / z + exp(a * sin(b))) 
```

    ## [1] "x"
