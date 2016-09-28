Functions in `R`
================
September 27, 2016

A quick reminder: functions in `R` are declared with the keyword `function` and assigning the declaration to a name:

``` r
hello_world <- function(x) print("Hello world!")
```

Functions can take named as well as positional arguments, with the first ones taking precedence. As expected, arguments can take default values. Either way, arguments are lazily evaluated (*i.e.*, they are evaluated only when they are called). For instance, compare:

``` r
g <- function(x) {
  return(1)
}
g(stop("Error!"))
```

    ## [1] 1

to

``` r
g <- function(x) {
  force(x)
  return(1)
}
g(stop("Error!"))
```

    ## Error in force(x): Error!

Notice that `force` is just a way of simply calling `x`.

A common special argument in `R` is the `dot-dot-dot` argument (`...`) that appears often in the documentation and that is intended to match any arguments not otherwise matched. It is useful for collecting and passing arguments to functions that are called within your function, and in cases in which you want to be flexible and not preespecify names that will come to the function.

``` r
f <- function(...) names(list(...))
f("a"=1, "b"=2)
```

    ## [1] "a" "b"

`R` implements *lexical scoping*, which means that if a name is not found within the function, it will look for it "one level up". This is very handy sometimes but mostly can be very dangerous if one is not entirely specific about which value this is referring to. More importantly, the code is not self-contained (it is not *pure*) and therefore harder to follow:

``` r
x <- 1
f <- function() {
  return(x + 1)
}
f()
```

    ## [1] 2

``` r
x <- 2
f()
```

    ## [1] 3

Also, note that default arguments are evaluated inside the function. This means that if the expression depends on the current environment the results will differ depending on whether you use the default value or explicitly provide one.

``` r
f <- function(x = ls()) {
  a <- 1
  return(x)
}
f()
```

    ## [1] "a" "x"

``` r
f(ls())
```

    ## [1] "f" "g" "x"

It is important to keep in mind that `R` looks for values when the function is evaluated, not when it’s created:

``` r
y <- 1
f <- function() y
y <- 15
f()
```

    ## [1] 15
