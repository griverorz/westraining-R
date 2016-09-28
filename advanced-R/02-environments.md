The basics of environments
================
September 28, 2016

Environments are important to understand some of the *quirks* of the `R` language. Environments box names associated to values or as a bag that keeps some names together.

You can use them in your workflow and they are actually similar to lists with some differences, but among other things, environments are "ordered": all environments have a parent.

``` r
e <- new.env()
e
```

    ## <environment: 0x7fba1e2eee28>

``` r
parent.env(e)
```

    ## <environment: 0x7fba1bc3bb78>

There are four special environments:

1.  The `globalenv()` is the interactive workspace.

2.  The `baseenv()` is the environment of the base package. Its parent is the empty environment.

3.  The `emptyenv()` is the ultimate ancestor of all environments, and the only environment without a parent.

4.  The `environment()` is the current environment.

We can list the parents of the `globalenv()` with the `search()` function:

``` r
search()
```

    ##  [1] ".GlobalEnv"        "package:stats"     "package:graphics" 
    ##  [4] "package:grDevices" "package:utils"     "package:datasets" 
    ##  [7] "package:setwidth"  "package:colorout"  "package:methods"  
    ## [10] "Autoloads"         "package:base"

Environments are relevant because they are used by functions. Functions are associated with four types of environments:

1.  The *enclosing* environment on which the function was created.
2.  The *binding* environment to which the name of the function is bound.
3.  The *execution* environment that is created when the function is evaluated.
4.  The *calling* environment on which the function was called.

The *enclosing environment* holds a reference to the environment in which it was created. It is used for *lexical scoping* and allows `R` to find values that the function needs. The *binding environment* allows `R` to find the function.

Namespaces (more on this later) are implemented with environments and its what ensures that we cannot overwrite functions provided by the base package:

For instance, the function `sd` calls the function `var` provided by the `stats` package:

    x <- 1:10
    sd(x)
    var <- function(x, na.rm = TRUE) 100
    sd(x)

This works because every package has two environments associated with it: the *package* environment and the *namespace* environment. Every exported function in a package is bound into the package environment, but enclosed by the namespace environment. When we type `var` into the console, it’s found first in the global environment. When `sd()` looks for `var()` it finds it first in its namespace environment so never looks in the `globalenv()`.

The other thing that is usually surprising to users from other languages is the *execution environment* which is created wen the function is called and thrown away after the function has completed:

``` r
g <- function(x) {
  if (!exists("a", inherits = FALSE)) {
    message("Defining a")
    a <- 1
  } else {
    a <- a + 1
  }
  a
}
g(10)
```

    ## Defining a

    ## [1] 1

``` r
g(10)
```

    ## Defining a

    ## [1] 1
