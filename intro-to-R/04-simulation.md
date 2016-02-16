Inference, probability and simulation
================
February 16, 2016

### Inference

Let's take a more careful look at the model we fit before:

``` r
affairs <- read.csv("http://koaning.io/theme/data/affairs.csv")
sample_model <- lm(nbaffairs ~ age, data=affairs)
```

We took a look at some values of interest, like the estimated coefficients or the confidence intervals around them. It may also be interesting to take a look at predictions.

``` r
yhat <- predict(sample_model)
head(yhat)
```

    ##    1    2    3    4    5    6 
    ## 1.61 1.27 1.44 2.28 1.10 1.44

The `predict` method takes a number of uselful arguments, like `newdata`, which applies the estimated coefficients to a new dataset.

``` r
predict(sample_model, newdata=data.frame("age"=54), interval="confidence")
```

    ##    fit  lwr  upr
    ## 1 2.18 1.52 2.85

### Simulation and probability

We saw in the previous script the somewhat mysterious function `qt`. It actually belongs to a very rich family of functions that are associated with probability distributions with a common name structure.

The first letter indicates the operation, while the rest of the name indicates the distribution. In particular, `r` indicate random generation, `d` density function, `q` quantile function, and `p` distribution function. Therefore, `rnorm` will make an extration out of a normal distribution, `pnorm` will give the probability associated with a quantile in a normal distribution and so on. R provides many distributions, Student's t, Snedecor's F, Gamma, Beta, ... and they all follow the same convention.

Thus, `qt` in the previous calculates the probability associated with a quantile in a t distribution with given dof. These functions are very useful. Let's see them in action:

``` r
rnorm(1)
qnorm(0.975, mean=1, sd=1)
rnorm(5, mean=2, sd=5)
rbeta(1, 1, 5)
```

For simulations it is probably a good idea to set up a seed for the RNG.

``` r
set.seed(20150211)
rnorm(1, 2, 1)
set.seed(20150211)
rnorm(1, 2, 1) 
```

The function `replicate` let's us repeat expressions a number of times:

``` r
x <- rnorm(25, 3.2, 1.7)
summary(lm(x ~ 1))
```

    ## 
    ## Call:
    ## lm(formula = x ~ 1)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.0663 -0.5488  0.0167  0.3609  2.7888 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)    3.665      0.237    15.5  5.5e-14
    ## 
    ## Residual standard error: 1.18 on 24 degrees of freedom

``` r
sd(replicate(9999, mean(sample(x, length(x), replace=TRUE))))
```

    ## [1] 0.229
