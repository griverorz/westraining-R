Using C++ code
================
September 25, 2016

R is able to connect to Fortran, C or C++ source code. This is very helpful for some functions that may be slow in R. That way we can do some manipulations in R, send the data to C++ for the heavylifting and then return the output back to R. The `Rcpp` package makes the connection to C++ very easy.

The source code is expected to be located in a `src/` directory in the package. Also, we need to import and the package and ensure that R knows it is source code.

In `DESCRIPTION` add

    LinkingTo: Rcpp
    Imports: Rcpp

and then `NAMESPACE` add:

    useDynLib(mypackage)
    importFrom(Rcpp, sourceCpp)

For simple functions, we can rely on inline C++ code using `cppFunction()`:

``` r
library(Rcpp)
cppFunction('int add(int x, int y, int z) {
  int sum = x + y + z;
  return sum;
}')
add
```

    ## function (x, y, z) 
    ## .Primitive(".Call")(<pointer: 0x10a201f30>, x, y, z)
    ## <environment: 0x7fc0e8f05918>

``` r
add(1, 2, 3)
```

    ## [1] 6

However, it will usually be easier to use C++ stand-alone files and source them using `sourceCpp()`. These functions have to start with

    #include <Rcpp.h>
    using namespace Rcpp;

and to ensure that they are available to R, they have to be exported with:

    // [[Rcpp::export]]

For instance,

``` cpp
#include <Rcpp.h>
using namespace Rcpp;

// Below is a simple example of exporting a C++ function to R.
// [[Rcpp::export]]
int timesTwo(int x) {
   return x * 2;
}
```

Each exported C++ functiongets a wrapper function in `R/RcppExports.R` that will look like:

``` r
timesTwo <- function(x) {
  .Call('timesTwo', PACKAGE = 'mypackage', x)
}
```

and we will need to `@export` it to make it available in the `NAMESPACE`.

`Rcpp` provides sugar functions to make C++ and R look more similar. It is possible to access attributes of R object passed to C++ and you can even pass lists and data frames using the `List` and `DataFrame` class in `Rcpp`. You can even put R functions inside your C++ code through the type `Function`!

Missing values a bit tricky.
