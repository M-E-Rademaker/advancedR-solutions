Solutions to Chapter 2
================
Manuel Steiner

-   [2.1.3 Exercise](#exercise)

2.1.3 Exercise
--------------

> **Question 1**: What are the six types of atomic vector? How does a list differ from an atomic vector?

The four common types (ordered from least flexible to most):

1.  logical
2.  integer
3.  double
4.  character

And two rather special types

1.  raw
2.  complex

Atomic vectors can only contain one type of input. Lists are vectors too:

``` r
l <- list("a" = 1:10, "b" = c("a1", "b1"))
is.vector(l)
```

    ## [1] TRUE

The difference between atomic vectors and list is that the latter may contain different input types.

> **Question 2**: What makes `is.vector()` and `is.numeric()` fundamentally different to `is.list()` and `is.character()`?
