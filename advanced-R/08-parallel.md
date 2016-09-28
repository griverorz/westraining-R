Parallel computing
================
September 27, 2016

We saw that one one to speed up our code was to rely on C++ for some heavylifting. Another alternative is to take advantage of some of the parallelization tools in `R`.

Some problems have a very natural parallel structure. Think for instance about bootstrapping: we want to take R independent samples. Of course, we can perform bootstrap by first drawing the first sample and calculating the statistic of interest (f.i., the standard deviation), then moving to the second sample and calculating the statistic again and so forth. Then, at the end we aggregate all the intermediate outcomes (f.i., taking the mean) to produce the final result. However, each of the intermediate operations can be handled independently: we can think about having a number of workers, each of them drawing a bootstrap sample and calculating of the statistic of interest separately. Then, they all send their results to a central manager to combine all the intermediate steps. That is the essence of parallel computing. Instead of performing a task *sequentially*, we can split a task into different *subtasks* that can be performed separately.

Notice that sometimes the overhead of splitting and distributing the tasks between the workers can be more expensive than performing the tasks sequentially in a single thread. Just think about how sometimes coordinating a team might be more difficult than just doing some things yourself!

There are some limitations to what `R` can do because it is single threaded. But let's take a look at a very simple example of an *embarrassingly parallel* task. Say we want to calculate a linear model on 10,000 bootstrap samples. We will first do this with a loop (although keep in mind that loops are generally inefficient in `R`):

We first set the parameters and the data:

``` r
N <- 1E4; M <- 1E3
x <- runif(M, 1, 20)
y <- rnorm(3.1 + 4*x, 0, 0.3)
dta <- data.frame(x, y)
```

This will be our main function:

``` r
boot <- function(x) {
  idx <- sample(1:nrow(x), nrow(x), replace=TRUE)
  res <- lm(y ~ x, data=x[idx, ])
  return(coefficients(res))
} 
```

And now we preallocate some space for the coefficients in `M` and run the loop:

``` r
ptime <- system.time({
M <- vector("list", N)
for (i in 1:N) {
    M[[i]] <- boot(dta)
}
})
ptime[3]
```

    ## elapsed 
    ##    20.5

What's the bottleneck? That we are waiting for one operation to finish before starting the next one. However, we have some spare cores in our computer (or nodes in a cluster) that can be used to run some of the inverses regardless of what happens in the other nodes.

`R` comes with some utilities for parallel computing in the `parallel` package, but I usually find `doParallel` a bit nicer.

We first attach the package:

``` r
library(doParallel)
```

    ## Loading required package: foreach

    ## Loading required package: iterators

    ## Loading required package: parallel

and the we use it to detect the number of cores in our machine and make a "cluster" with them.

``` r
no_cores <- detectCores() 
print(no_cores)
```

    ## [1] 4

``` r
registerDoParallel(cores=no_cores)
```

We can now run our calculations using the `%dopar%` infix function:

``` r
ptime <- system.time({
  boots <- foreach (i=1:N, .export=c("boot")) %dopar% {
    boot(dta)
}
})
ptime[3]
```

    ## elapsed 
    ##    11.3

We can see that, even in this simple example, in a machine with 4 cores, there are already very significant improvements in performance. However, notice that the improvement is not 4x because there is considerable overhead in moving data around and manipulating results.
