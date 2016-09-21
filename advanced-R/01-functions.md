Functions
================
September 21, 2016

A quick reminder: functions in R are declared with the keyword `function` and assigning the declaration to a name:

``` r
hello_world <- function(x) print("Hello world!")
```

Functions can take named as well as positional arguments, with the first ones taking precedence. Arguments can take default values,

``` r
f <- function(first=x, y) {
  
}
```

Either way, arguments are lazily evaluated (*i.e.*, they are evaluated only when they are called).

``` r
g <- function(x) {
  return(1)
}
g(warning("Error!"))
```

    ## [1] 1

A common special argument in R is the dot-dot-dot argument (`...`) which will match any arguments not otherwise matched. It is useful for collecting and passing arguments to functions that are called within your function. Also, for cases in which you want to be flexible and not preespecify names that will come to the function.

``` r
f <- function(...) names(list(...))
f("a"=1, "b"=2)
```

    ## [1] "a" "b"

R implements lexical scoping, which means that if a name is not found within the function, it will look for it one level up. This is very handy sometimes but mostly can be very dangerous if one is not entirely specific about which value this is referring to:

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

# ls() evaluated inside f:
f()
```

    ## [1] "a" "x"

``` r
# ls() evaluated in global environment:
f(ls())
```

    ## [1] "f" "g" "x"

Also, R looks for values when the function is run, not when it’s created:

``` r
y <- 1
f <- function() y
y <- 15
f()
```

    ## [1] 15
