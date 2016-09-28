Functions and object oriented programming in R
================
September 28, 2016

R supports three object-oriented systems: S3, S4, and RC. The first two implement generic-function OO, which is the most usual in R. The last one, RC, is a message-passing OO which may be familiar to Python or C++ programmers. S4 is more formal and may be an overkill in most cases but I find it clearer.

Functions can return only a single object. But this is not a limitation because you can return a list containing any number of objects.

### S4 programming

In S4, "messages (methods) are sent to objects and the object determines which function to call."

To define a new class we need a name and slots. For instance, in the `nps` package, I define a new class for objects that will contain information from NPS. An object will be defined by the values and the top and bottom categories that were used for the NPS.

``` r
setClass("nps",
         slots = list(values="numeric", top="numeric", bottom="numeric"))
```

I could then create instances of `nps` by using the `new` function.

``` r
new("nps", values=sample(0:10, size=10, replace=TRUE), top=9:10, bottom=0:6)
```

    ## An object of class "nps"
    ## Slot "values":
    ##  [1] 5 1 0 2 8 6 5 5 9 8
    ## 
    ## Slot "top":
    ## [1]  9 10
    ## 
    ## Slot "bottom":
    ## [1] 0 1 2 3 4 5 6

I can also define a function that validates, when the instance is created, whether it is a correct object. Now, we can redefine the class so that it evaluates during creation whether the new object passes the validity check:

``` r
check_nps <- function(object) {
    errors <- character()
    if (length(intersect(object@top, object@bottom)) > 0) {
        msg <- paste("Top and bottom must not overlap")
        errors <- c(errors, msg)
    }

    if (length(errors) == 0) TRUE else errors
}

setClass("nps",
         slots=list(values="numeric", top="numeric", bottom="numeric"),
         validity=check_nps)
```

If we try to create an instance that does not comply with the test:

``` r
new("nps", values=sample(0:10, size=10, replace=TRUE), top=6:10, bottom=0:6)
```

    ## Error in validObject(.Object): invalid class "nps" object: Top and bottom must not overlap

we get the user-defined error.

We can access the "slots", using the `@` operator or using `slot()`:

``` r
x <- new("nps", values=sample(0:10, size=100, replace=TRUE), top=9:10, bottom=0:6)
x@top == slot(x, "top")
```

    ## [1] TRUE TRUE

#### Methods

In order to create methods for a class, we use `setMethod` with the signature of the class it is going to be applied and the function that is going to be used.

``` r
setMethod("summary",
          signature("nps"),
          function(object) {
              return(table(factor(object@values)))
          })
```

    ## Creating a generic function for 'summary' from package 'base' in the global environment

    ## [1] "summary"

For instance, we can check how `R` dispatches appropriately based on the class of the object

``` r
x <- c(1, 2, 3)
class(x)
```

    ## [1] "numeric"

``` r
summary(x)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     1.0     1.5     2.0     2.0     2.5     3.0

while

``` r
x <- new("nps", values=sample(0:10, size=10, replace=TRUE), top=9:10, bottom=0:6)
class(x)
```

    ## [1] "nps"
    ## attr(,"package")
    ## [1] ".GlobalEnv"

``` r
summary(x)
```

    ## 
    ## 3 7 8 9 
    ## 1 3 5 1

We can list the methods associated with a class

``` r
methods(class="nps")
```

    ## [1] summary
    ## see '?methods' for accessing help and source code

and the methods associated with a generic function:

``` r
methods(summary)
```

    ##  [1] summary,ANY-method             summary,nps-method            
    ##  [3] summary.aov                    summary.aovlist*              
    ##  [5] summary.aspell*                summary.check_packages_in_dir*
    ##  [7] summary.connection             summary.data.frame            
    ##  [9] summary.Date                   summary.default               
    ## [11] summary.ecdf*                  summary.factor                
    ## [13] summary.glm                    summary.infl*                 
    ## [15] summary.lm                     summary.loess*                
    ## [17] summary.manova                 summary.matrix                
    ## [19] summary.mlm*                   summary.nls*                  
    ## [21] summary.packageStatus*         summary.PDF_Dictionary*       
    ## [23] summary.PDF_Stream*            summary.POSIXct               
    ## [25] summary.POSIXlt                summary.ppr*                  
    ## [27] summary.prcomp*                summary.princomp*             
    ## [29] summary.proc_time              summary.srcfile               
    ## [31] summary.srcref                 summary.stepfun               
    ## [33] summary.stl*                   summary.table                 
    ## [35] summary.tukeysmooth*          
    ## see '?methods' for accessing help and source code

Oftentimes, we will need to create our own generic `standardGeneric`:

``` r
setMethod("score",
          signature("nps"),
          function(object) {
              tab <- as.vector(summary(object)/length(object@values))
              score <- tab[1] - tab[3]
          })
```

    ## Error in setMethod("score", signature("nps"), function(object) {: no existing definition for function 'score'

If our method requires a new generic, we can define it through `setGeneric` and

``` r
setGeneric("score", function(object) standardGeneric("score"))
```

    ## [1] "score"

``` r
setMethod("score",
          signature("nps"),
          function(object) {
              tab <- as.vector(summary(object)/length(object@values))
              sc <- tab[1] - tab[3]
              return(sc)
          })
```

    ## [1] "score"

and now:

``` r
score(x)
```

    ## [1] -0.4

There are special methods for `[` and `foo<-` (replacement functions). Although they can created in a different way in `R`, I find this solution cleaner:

``` r
setGeneric("top<-", function(object, value) standardGeneric("top<-"))
```

    ## [1] "top<-"

``` r
setReplaceMethod("top", signature("nps"), function(object, value) {
    object@top <- value
    if (validObject(object)) {
        return(object)
    }
})
```

    ## [1] "top<-"

Notice how the method tests whether the instance after the transformation is a `validObject` of the class. We can now replace the contents of the `top` slot of the instance through a *setter* method:

``` r
top(x) <- c(8:10)
x
```

    ## An object of class "nps"
    ## Slot "values":
    ##  [1] 8 8 8 3 9 7 7 8 8 7
    ## 
    ## Slot "top":
    ## [1]  8  9 10
    ## 
    ## Slot "bottom":
    ## [1] 0 1 2 3 4 5 6

Similarly, we have *getter* methods:

``` r
setGeneric("top", function(object) standardGeneric("top"))
```

    ## [1] "top"

``` r
setMethod("top", signature("nps"), function(object) object@top)
```

    ## [1] "top"

``` r
top(x)
```

    ## [1]  8  9 10

We can also create a `[` method to perform subset and replicate this operation:

``` r
x@values[1:4]
```

    ## [1] 8 8 8 3

The new method takes at least two arguments `x` and `i`:

``` r
setMethod("[", signature("nps"), function(x, i) {
    new("nps", values=x@values[i], top=x@top, bottom=x@bottom)
})
```

    ## [1] "["

Now we can extract the 1st to 4th values of `x`:

``` r
x[1:4]
```

    ## An object of class "nps"
    ## Slot "values":
    ## [1] 8 8 8 3
    ## 
    ## Slot "top":
    ## [1]  8  9 10
    ## 
    ## Slot "bottom":
    ## [1] 0 1 2 3 4 5 6
