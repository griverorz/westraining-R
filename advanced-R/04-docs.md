Documentation using Roxygen
================
September 21, 2016

R documentation is great and, more importantly, it is easy to create. Obviously, you can just add comments to the code or external documentation, but it is a lot better to just be able to pull the documentation when you type `?function` in your console.

It is possible to manually write the documentation separately but it is a lot easier to use roxygen

The Roxygen syntax is very easy and consists of two types of entries:

    Special comment #'
    Variables marked by #' @

The documentation should have at least the following components:

1.  **Title:** Cross Tabulation and Table Creation.
2.  **Description:** `table` uses the cross-classifying factors...
3.  **Arguments:** `...` one or more objects which can be intepreted as factors
4.  **Value:** `table()` returns a contingency table

It can include details, links or examples.

Take a look at

``` r
?table
```

And let's build the documentation

``` r
#' Cross Tabulation and Table Creation
#'
#' \\code{table} uses the cross-classifying factors to build a contingency table of the counts at each combination of factor levels.
#' @param `...` one or more objects which can be interpreted as factors 
#' @value \\code{table()} returns a contingency table
table <- function(...) {
  # Your code here
}
```

The documentation for methods and classes is a bit different and needs to define the `@slot`s:

``` r
#' An S4 class to work with NPS data
#'
#' @slot values A numeric vector with the answer values to the NPS question
#' @slot top A numeric vector with the values in the "Promoters" category
#' @slot bottom A numeric vector with the values in the "Detractors" category
setClass("nps",
         slots = list(values="numeric", top="numeric", bottom="numeric"),
         validity=check_nps)
```

It is very handy to sometimes link and group several document pages together. We can acomplish that with the variable `@rdname`.

`roxygen2` also allows us to control the `NAMESPACE`

Exports
-------

To export an object, put `@export` in its roxygen block. For example:

``` r
#' @export
foo <- function(x, y, z) {
  ...
}
```

This will then generate `export()`, `exportMethods()`, `exportClass()` or `S3method()` depending on the type of the object.

If you want others to be able to extend your class, `@export` it. If you want others to create instances of your class but not to extend it, `@export` the constructor function, not the class.

``` r
# Can extend and create with new("A", ...)
#' @export
setClass("A")
```

S4 generics: `@export` if you want the generic to be publicly usable.

S4 methods: you only need to `@export` methods for generics that you did not define.

Imports
-------

The Imports field really has nothing to do with functions imported into the namespace: it just makes sure the package is installed when your package is. You need to import functions in exactly the same way regardless of whether or not the package is attached.

It’s common for packages to be listed in Imports in `DESCRIPTION`, but not in `NAMESPACE`.

If you are using just a few functions from another package, my recommendation is to note the package name in the `Imports:` field of the DESCRIPTION file and call the function(s) explicitly using `::`, e.g., `pkg::fun()`. If you are repeatedly using many functions from another package, you can import all of them using `@import package`

Since S4 is implemented in the `methods` package, you need to make sure it’s available.

To use classes defined in another package, place `@importClassesFrom package ClassA ClassB` ... next to the classes that inherit from the imported classes, or next to the methods that implement a generic for the imported classes.

To use generics defined in another package, place `@importMethodsFrom package GenericA GenericB` ... next to the methods that use the imported generics.
