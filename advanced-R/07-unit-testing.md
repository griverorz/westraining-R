Unit testing
================
September 27, 2016

In non-trivial programming tasks, we want to be able to verify that changes to the code are not breaking some of the elementary functions. To help mitigate this problem, we may want to deploy a number of functions that validate that some functions produce the expected predetermined results. This is what "unit testing" accomplishes.

In `R` we have the `testthat` package:

``` r
library(testthat)
```

If we want to use it in a package, we will need a `tests/testthat directory`, add`testthat` to the `Suggests` field in the `DESCRIPTION` and create a `tests/testthat.R` that runs the tests when `R CMD check` runs. With that, we can then test the code using the `test()` function.

Imagine that our package provides the following function that pulls the numbers from a string.

``` r
extract_numbers <- function(x) {
    if (is.factor(x)) {
        warning("Input is factor")
        x <- as.character(x)
    }
    if (!is.character(x)) {
        stop("Input must be character")
    }
    str <- gsub("[^0-9]", "", x)
    return(as.numeric(str))
}
```

We want to test several things. First that given a string it actually extracts all the digits and only the digits:

``` r
context("Extract numbers")
test_that("extract_numbers extracts the digits", {
    expect_equal(extract_numbers("a1"), 1)
    expect_equal(extract_numbers("1"), 1)
})
```

But we also want to make sure that the function behaves correctly if the input does nto contain any digits:

``` r
test_that("extract numbers of a string with no digits produces NA", {
    expect_equal(extract_numbers("a"), NA_integer_)
})
```

Or when the input is unexpected. In particular, we want to make sure the function outputs correct warnings and errors:

``` r
test_that("extract numbers of factor is a warning", {
    expect_warning(extract_numbers(factor("a")), "Input is factor")
})


test_that("extract numbers of number is error", {
    expect_error(extract_numbers(1), "Input must be character")
})
```

The `testthat` package offers a few more options but they all have a similar structure: we test the behavior of the function against what we expect to happen.

Which tests to run is largely a matter of ingenuity in deciding which are the most important functions in our code and how they can fail. However, there are tools that can help us.
