### Challenge 3: Programming

1. Here is a classic: Write a program that prints the numbers from 1 to 100. But
   for multiples of three print "Fizz" instead of the number and for the
   multiples of five print "Buzz". For numbers which are multiples of both three
   and five print "FizzBuzz."

2. Consider the following dataset, in which `house_id` indicates the household,
   `person_id` the person within the household, `age` the age of `person_id`.
   
   ```r
   dta <- data.frame("house_id"=rep(1:25, each=5),
		          "person_id"=1:5,
				  "age"=rbinom(125 , 100, .4))
   ```

	a. Get the age of the youngest individual in each household.

	b. Get the person_id for the youngest individual in each household.

	c. Imagine that now we get two additional vectors:

	```r
	dta$weight <- runif(125)
	dta$attr1 <- ifelse(rbinom(125, 1, 0.85), rpois(1, .5), NA)
	```

	Write a function that calculates a weighted average and apply it to the dataset to get the weighted average of `attr1` for each household.
