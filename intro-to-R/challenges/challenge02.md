### Challenge 2: Data analysis and graphics

1. Let's do some simulations:

	a. Get the sampling distribution for the proportion of tails in a balanced
    coin, in an experiment that consist in flipping the coin 10 times. Take a
    look at the `rbinom` function.

	b. Create a histogram with the distribution obtained above. Calculate the
    probability of observing more than 8 of tails in 10 throws and mark that event
    on the graph.
	
	c. Imagine our experiment of flipping a coin 10 times produced 7 tails.
    Calculate the 99% confidence interval for the proportion of tails.
	
2. Now lets get some more information about the models that we plotted before:

	a. Read in the dataset on tobacco consumption, and estimate the parameters of the
	lin-log model between tobacco consumption and tax that we plotted before.

	b. Calculate the predicted values of the model in the previous sections with
	confidence intervals at the 99\% confidence level.

	c. Plot the predicted values you calculated above without using the `stat`
    and `formula` arguments.

	e. Estimate the parameters of the lin-log model above with fixed effects per
    state, and get the predictions using the average tax rate in that state.
