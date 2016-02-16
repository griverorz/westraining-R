### Challenge 1: Types and data structures

1. A problem that appears often in R is about the creation of sequences. Do not
   input the sequences manually. 
   a. Create a vector that contains a sequence of odd numbers from up to 31. Do not
   input it manually. Take a look at the documentation for `seq`.
   b. Create a
   sequence of type `logical` that takes value TRUE for odd numbers. Hint: you
   can define odd numbers using the reminder of the division by 2 through the
   modulus operator. Think about 3 mod 2 vs. 4 mod 2.
   c. Create a sequence of
   multiples of 3 and a sequence of multiples of 7 up to 49 and return the
   number of elements in the intersection.

2. We will see how to perform data analysis in R, but it is probably a good idea
	to take a look at how to run a simple model manually:
	a. Input the following matrices into R:
$$
X=\\left(
\\begin{array}{cc}
2.1 & -0.3 \\\\
3.2 &  \text{na}  \\\\
4.7 & -2.9	\\\\
\\end{array}
\\right),\quad
y=\\left(\\begin{array}{c}
1.1 \\\\
1.6 \\\\
2.5 \\\\
\\end{array}\\right)
$$
	where "na" stands for a missing value. Be careful with the dimensions of y.
	b. Impute the missing value in $X$ with the column mean. 
	c. Calculate the OLS estimator of $\beta$ for the model $y = \beta X + \varepsilon$
	in matrix form. Remember that the OLS estimator is:
	$$\hat \beta = (X^{T}X)^{-1}X^{T}y$$
	Take a look at the `crossprod` function.

3. Load the Iris dataset into R using `data(iris)`.
   a. Get the proportion of observations of each species.
   b. Calculate the mean and standard deviation of the length of the petal for
   each species.
   c. Calculate correlation matrix of the numerical variables. Take a look at
   the function `corr`.
