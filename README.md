# Thomas Madeley Kraken Flex Dynamic Containment Submission

### Context. 

Hi there, thank you for taking the time to read my submission. I will briefly summarise my approach and attempt to explain
some of the decision making process and tradeoffs made. 


## Task

The full task summary can be found in the brief: `Data Science Interview Task 2021.pdf` in the main repo. 

In this brief, we were tasked with modeling a proposed Dynamic Containment service, analyzing the performance of a given grid battery over time. 

We have been provided with a subsample of grid frequency data at 1Hz intervals, a formula to describe the dynamic containment strategy and the specifications of a grid battery.  with which to do this

The goal was to understand the battery's state of charge (SOC) behavior, the longest expected service duration without hitting constraints, and to provide recommendations on the most suitable service type.

# Instructions


## Set up and installation

We have created a `Makefile` to house some helpful commands for ease of use. You must have some form of Conda installed for this to work. 

#### Create Environment and install the package in editable mode
Run the command below from the root directory, and answer `y` when asked about installation of dependencies.

This will automatically create a conda environment and ipykernel kernel for use in a jupyter notebook. 

```shell
make create_environment
```

If you need to update the dependencies, simply add the new package name(s) to `requirements.in` and then run:

```shell
make update_requirements

make dev_install
```
This will use pip-tools to search for compatible versions of all dependencies, then install the new dependencies in your conda environment. 


## TL;DR, Give Me A Brief Summary

To model and analyze the performance of the grid battery under different dynamic containment services, we followed a statistical approach using Monte Carlo simulations:

- We started by loading and inspecting the provided frequency dataset to understand its characteristics. We found it to be partially complete and that the distribution of deviations from 50Hz was approximately normally distributed with caveats (see section 4.)
- Next, we fitted a distribution to the frequency deviation data to model the variations statistically. We experimented with a Gaussian Mixture Model to account for Bimodality but found a basic normal distribution fit better.
- A Python class was developed to simulate the behavior of the grid battery, incorporating its charging and discharging mechanisms as specified by the parameters in the brief by default, but this can be tweeked and re-used. 
- The dynamic containment strategy was implemented as a function to determine power demand based on frequency deviations.
- Using a Monte Carlo simulation approach, we performed numerous simulations to estimate the battery's performance over time. Unfortunately my local hardware meant I was unable to run lots of simulations but I hope I proved the concept and adequately explain *how* I would scale this up given more time and a big EC2 instance! (See section 8 for full analysis). 
- Finally, we analyzed the results to determine the battery's state of charge, the longest expected service duration, and recommended the most suitable service type based on the simulation outcomes.

I had a lot of fun learning about dynamic containment and exploring some statistical methods, I hope you enjoy reading my submission 😀

**TL;DR Results**

1. **State-of-charge behavior when running the 'both' DC service:**
   - At all service power levels, the mean SOC gradually declines over time due to charging losses, even though the battery is being charged and discharged at approximately equal rates.

2. **Longest expected service duration without hitting limits:**
   - Based on the assumptions of our model and simulations, using only 5 simulations and commencing the battery SOC at 99%, we achieved the following mean durations to reach 0% SOC:
     - 2 MW: 324.94 days
     - 5 MW: 129.56 days
     - 10 MW: 64.86 days
   - Running thousands of simulations would provide more accurate estimates, confidence intervals, and insights into the battery's performance variability.
   - Both the 2MW and 5MW services can be safely run for an extended period without running into power limitations, but the 5MW service could potentially run into power limitations as the full service power is greater than the battery's maximum charge rate.
   - The 10MW service did run into a power limitation where the requested input from the grid exceeded the available power from the battery.

3. **Recommendations on running high, low, both services with different battery sizes:**
   - A 'Both' service will inevitably result in an empty battery due to charging losses over a long period of time.
   - A 'High' service will inevitably result in a full battery within days.
   - A 'Low' service will inevitably result in an empty battery within days.
   - There seems to be little benefit in running a 'low' service when a 'both' service can handle all of the simulated charge and discharge demand.
   - If we had several batteries, we could offer both 'High' and 'Both' services in conjunction, rotating batteries to allow adequate time for the 'Both' service to discharge our battery, then switch it to a 'high' service to recoup the lost charge. With some optimization, this would allow us to meet all of the grid's demands whilst never incurring an empty or full SOC in the battery.



## If I Had More Time

- There are obviously many ways this analysis could be taken. I would have liked to have explored a Monte Carlo Markov Chain (MCMC) approach to the problem. The basic Monte Carlo implementation here ignores the fact that the next frequency deviation is closely related to the current one, I feel this may have yielded a more accurate result with less simplification of the problem: In my implementation we have effectively assumed that the deviation is random. Similarly, Bayesian methods may work well here but this is somewhat outside my comfort zone. 

- I would have liked to have moved functions and classes into appropriate modules in the repository, with proper testing and error handling. This would make the notebook much more readable. Naturally this is a time limited task and these things are out of scope. There is far too much logic in this notebook to go untested!

- A simple improvement here would to create functions to create the plots, this would reduce code duplication and clutter in the notebook.

- I would have liked to have implemented a multi-processing approach to the monte carlo simulations to speed up the simulations to enable me to perform more. This would really improve the quality of the results (see section 8) and would have allowed me to give ranges of values with confidence intervals to better inform decisions. 

-Similarly, I would have liked to use our deviation distribution to calculate the probabilities of encountering a power limit at 5MW and 10MW for both charging and discharging as described in section 8.