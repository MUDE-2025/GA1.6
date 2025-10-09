# Group Assignment 1.6: Ice Thickness Model

*[CEGM1000 MUDE](http://mude.citg.tudelft.nl/)* 

*Written by: `Lotfi Massarweh`*

*Due: `Friday 10th October, 2025`.*


*Most of the questions require you to finish `Analysis.ipynb` first, but depending on how you allocate tasks between group members, it is possible to work on this in parallel. Make sure you save time for peer reviewing each others work before completing the assignment!*


**Question 1**

*What is the expected value and standard deviation of the ice thickness after 3 days ($\mu_H$ and $\sigma_H$)? There should be two sets of results.*

% solution_start

Mean and standard deviation of linearized function:

- $\mu_h = 0.278$ m  
- $\sigma_h = 0.034$ m

Mean and standard deviation of simulated distribution:

- $\mu_h = 0.278$ m  
- $\sigma_h = 0.035$ m

% solution_end

**Question 2**

*Explain whether we should use the expected value for our prediction, or whether we should also account for the variability of the thickness estimate in the subsequent phases of our analysis?*

% solution_start

It depends! $\sigma_H$ = 0.034 m is approximately 10% of $\mu_H=0.278$ m; whether or not this is a large variability depends on the sensitivity of our (unknown) model for predicting break-up day as a function of ice thickness. If that model is not very sensitive to ice thickness, it may not be necessary to account for the uncertainty in subsequent phases of the analysis and we can use the expected value directly. If it is very sensitive to this parameter, we should include the uncertainty, perhaps with another round of uncertainty propagation (or numerical simulation).

% solution_end

**Question 3**

*How do we obtain the "true" distribution of $H_{ice}$, and what does it look like?*

% solution_start

To find the "true" distribution of $H_{ice}$ we must propagate the uncertainty through simulations. That is, we know the distribution functions of the inputs $\Delta T$ and $H_{ice,0}$ and we know that they are independent, so we can generate random samples of them directly from the distribution functions, plug them into the function and use the resulting realizations of $H_{ice}$ to approximate the "true" distribution. We can see that the PDF of the Normal distribution matches the histogram quite closely, however, looking at the quantile plot indicates that the extreme low values of the distribution (the lower, or left tail) are not approximated well by the Normal distribution.

% solution_end

**Question 4**

*Are the propagated and simulated $\mu_H$ and $\sigma_H$ equivalent?*

% solution_start

They are not exactly equivalent, but within 1 mm of each other, which is close enough for our purposes of estimation.

% solution_end

**Question 5**

*Is the Normal distribution a reasonable model for $H_{ice}$?*

% solution_start

Yes, since the central moments seem to be properly estimated the Normal distribution would be a reasonable model, with one exception: we can see that the simulated values deviate from the line in the tails. Thus, if you are interested in estimating values with low non-exceedance probabilities this is a bad model: we can see that below 3 standard deviations the difference is a few cm, or around 10% of the expected value. This would be a concern if your break-up date model is sensitive to accurately predicting the likelihood of exceptionally small ice thicknesses given the uncertainty in our estimates of the future temperature and past measurements of the true ice thickness.

% solution_end

**Question 6**

*Using the loop in Task 3.1, explore the effect of sample size on the results. Describe the observations you make and explain why they are happening.*

% solution_start

The results are clearly dependent on sample size; this is logical given that the set of samples from a Monte Carlo method asymptotically converges to the true distribution as sample size increases. It is obvious that the empirical distribution is not good for $N=5$ and $N=50$, but it is interesting how *skewed* the distribution is for $N=5000$. Conclusion? Always be mindful that the number of samples matters!

% solution_end

**Question 7**

*Why is the sampling distribution not the "true" distribution?*

% solution_start

There are two primary reasons. The first is captured by the previous question and answer: the result from a Monte Carlo Simulation (an empirical distribution) depends on the number of samples. As this increases, the "true" distribution is better approximated (asymptotically).

The second reason is that the Monte Carlo Simulation is only as good as our assumptions for the input distributions and the function itself. If these are a poor representation of reality, then the MCS result will be poor as well. For example, if we assume Normal distributions for the input parameters, but the "true" distributions are skewed, the results may be inaccurate. In reality we would like to measure this, but we can't, so we assume distributions of input. This is an assumption, and may not be a true representation of reality; it could be very far off. The function itself is also a model and introduces error.

In short, the "sample" (our model) is only "true" if our model(s) are perfect, which in reality is never the case. Jammer!

% solution_end

**Question 8**

*Describe the values of the function of random variables (the output) for which we might expect the model to be inaccurate. Quantify this inaccuracy by comparing the probability calculated with the assumed distribution with the frequency of similar values observed. Use the empty cell in Task 3 in the `Analysis.ipynb` file for computations.*

*Hint: you can count the number of values in an array that conform to a specific boolean condition using* `sum(MY_ARRAY <= MY_VALUE)`. *It may also be useful to find the length of an array with* `len(MY_ARRAY)`.

% solution_start

It looks like the Normal distribution and sampled distribution deviate for values of $q<2$, or $H_{ice}<0.2$ m (values where the dots deviate from the line). A short `for` loop in the code computes the probability of being in the lower tails of the theoretical and empirical distribution, which is around 1% and 2%, respectively. This is not a huge difference, however, we see that the estimated probability values differ *significantly* as the value of ice thickness decreases. For example, at 0.13 m the theoretical distribution underpredicts the probability by a factor of 10! This means the uncertainty propagation law (combined with an assumption of the Normal distribution) will underpredict the chance that the ice is thin, which could have significant impact on the accuracy of our ice model and our bets. Conclusion: we better study more probability to account for the tails of the distributions better! (We will do this later in MUDE).

Note that in this case the use of the terms *probability* and *frequency* are terms used for the distribution and empirical sample, respectively; in both cases they quantify the uncertainty associated with observing specific values of ice thickness after a few days.


% solution_end

> By Lotfi Massarweh, Delft University of Technology. CC BY 4.0, more info on the Credits page of Workbook. 
> 
**End of file.**