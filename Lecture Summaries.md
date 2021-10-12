# Data and it's properties
### Types of data:
- Simple
- Multidimensional
- Temporal
- Spatial
- Structured
- Unstructured
- Categorical
    + nominal
    + ordinal
    + interval
- Quantative
    + discrete
    + continuous

### Meta data:
Documents the purpose, limitations, units and sourcing of the dataset.

### Transformations:
- Scaling of the dataset (typically mean = 0 and standard deviation = 1)
- Time series: component vector transformations, through Fourier Transformations
- Log transformations
- Dimension reduction

# Clustering 
### Similarity:
A measure of how different any two points are to each other, typically interpretted and determined through distance metrics

Typically requires the data to be scaled, such that all numeric values are on the same scale. 

### Feature creation:
Using the important features, we can create new variables that can be leveraged in clustering processes, or analysing the parameters of the clusters.

### Distance Metrics:
Requirement          | Equation
---------------------|------------------
Equality of distance | `Dist(x,y) == Dist(y,x)`
Difference in values means distance in space | `Dist(x,y) =/= 0`, if `x=/=y`
Triangular inequality | `Dist(x,y) + Dist(y,z) =/= Dist(z,x)`

# Measuring supervised model preformance
### Components of model assessments:
- cross model comparisons
- cross dataset comparisons (testing and training data splits)
- sensitivity analysis; how the model changes when it's inputs change __covered further in lecture 25__

### Regression Metrics:
- Mean Squared Error
- Root Mean Squared Error
- Mean Absolute Error
- Root Relative Squared Error
- Coefficient of determination (R<sup>2</sup>)
### Classification Problems: Misclassification - Is the cost of one type of error higher than another?
- Sensitivity: what proportion of the true positives have we missed? `TP/(TP+FN)`
- Specificity: what proportion of the true negatives have we correctly identified? `TN/(TN+FP)`
- Receiver Operator Curves, ROC: TP relative to FP

No overlap, so no misclassification | Some overlap and misclassification
------------------------------------|--------------
![Screenshot (1140)](https://user-images.githubusercontent.com/80669114/137035836-91e820d0-4a12-4ac3-87b7-61137f3a024a.png) | ![Screenshot (1141)](https://user-images.githubusercontent.com/80669114/137035926-98c481c7-5839-4265-982e-8735a0fd38c5.png)

### Cross Validation
### Boostrapping

# Decision Trees
### Governing principle:
Recursively splitting the dataset by the variable that increases the 'purity' of each node, relative to the parent node. 

Once a given purity or node size is reached, stop recursively splitting.

### Entropy:
`H(p) = -sum(P(s)-log(P(s)))`, where P(s) is the probability of being in state s.

Maximum entropy occcurs when, from all the information you have, the likelihood of a given instance being in any given state is equally likely.

### Information Gain:
We want to maximise the information gain that occurs at every itterative stage: the aim is to increase the reliability and accuracy of our predictions of where each instance should go. 

### Aggregate Models:
#### Bagging

#### Boosting

#### Random Forest


# Time Series
### Univariate data, what does this give us? 
- differences
- return periods
- seasonal trends
- anomaly detection
- summary statistics (mean, var, min, ...)

### What we don't have:
- Correlated factors and external event data
- No explanatories; we only have the response variable

### Exponential Smoothing:
Weights are given to all instances, and these weights decrease as the distance as the instance gets older (inversely proportional to difference between instance and most recent instance), at an exponential rate.

This has the effect of placing more emphasis on significant events and recent events.

### Equations
- Level: l<sub>t</sub> = alpha(Y<sub>t</sub> - S<sub>t</sub> - m) + (1- alpha) * (l<sub>t</sub> + b<sub>t-1</sub>)
- Trend: b<sub>t</sub> = B<sub>t</sub> (l<sub>t</sub> - l<sub>t-1</sub>) - (1 - B) * b<sub>t-1</sub>
- Seasonal: S<sub>t</sub> = Lambda (y<sub>t</sub> - (l<sub>t-1</sub>) - (b<sub>t-1</sub>)) + (1 - lambda) * S<sub>t-m</sub>, where `m` is the seasonal period

# Genetic Algorithms
### Evolutionary Strategies
Mutating computed parameters of a fitted function and selecting those subversions that worked better. Effectively a hill climbing algorithm, where it can't see the hill

Each individual is a fixed length reprentation of values that inform a strategy relating to a scoring heuristic. _"Most fit"_ individuals reproduce and push out _"less fit"_ individuals, promoting successful strategies, mimicing real world evolution.

### Fitness:
Fitness is defined as how well an agent's parameters allow it to solve the problem. 
A way of defining the fitness of a given individual is required to run and train a genetic algorithm. This is typically done as a "fitness function", evaluated at the end of a generation/epoch. 

    Initialisation 
    |   
    |       --- variation <--
    Evaluation              |
    |       --> selection ---
    |
    Termination

### Types of new instances:
- generational: whole population or a proportion of the population is replaced.
- steady state: newly created individuals replace single instance from within the population.

### Selection processes:
Methods: 
- __proportional selection:__ each individual is given a proportional chance of being selected, defined by it's fitness relative to the population's net fitness. `probSelection(i) = fitness(i)/sum(fitness(population))`
- __tournament:__ randomly select N individuals from the population and place them in a "pool". From that pool, we select the fittest individual and pass them forward into the next generation.
- __ranked:__ order the population by fitness. Parents are then selected from an upper proportion of this ranked list.

All selection methods are designed to create a biased selection criteria, biased towards selecting _"fitter"_ individuals over _"less fit"_ individuals.

### Crossover "recombination" and mutation:
New instances are created by combining the values of multiple parents, and then subsequently applying mutation.
Crossover strategies | Specification
---------------------|-------------------
fixed point | [0,0,0,0] & [1,1,1,1] = [0,0,1,1]
multi point | [0,0,0,0] & [1,1,1,1] = [0,1,1,0]
uniform probability | fixed probability of a given value being crossed in from one parent, overriding another, applied to each gene

__Mutation:__ modifying the values of specific genes within an organism, based on probabilistic chance.
Can be changed to a random value, or to within a range of the original value 

### Issues with Genetic Algorithms
- This approach needs dynamic problems to be effective. It's very poor on (logistic) regression and classification problems, as it needs to be able to assess the quality of the solution.
- __Premature stabilisation/convergence:__ The selection processes will naturally push the population state to stability, thus requiring the mutation parameter to prevent convergence before a solution is found.
- Genetic Algorithms are not ensured to reach an optimal solution
- __Drift:__ Given children are created from randomly selected parents, there is a chance for stochasticly induced drift away from optimal behaviour. Ideally, the fitness function should limit this, eliminating the _unfit_ individuals and rewarding a return to _fitter_ behaviour. 
- They're very slow, the random initialised behaviour, random selection, and lack of a strict gradient curve to move along means these algorithms can take a long time to convergence, if they do at all.
- As it's a random process, it's not directly reproducible, and small changes to random states and inital params can have a significant effect on the final outcome.

### Assets of Genetic Algorithms
- Genetic Algorithms doesnt require a defined gradient/cost surface to be described before hand
- Genetic Algorithms are a highly flexible general purpose architecture
- Genetic algorithm still function under unoptimised hyperparameters, it's fairly robust, it just won't be as efficent as it could be
- It is highly applicable to complex multivariable environments, where bruteforce search methods and are slow and expensive.

# Genetic Programming:
### Genetic Programming: Trees
Rather than having a fixed length reprentation (_chromosome_), tree-type genetic programming uses a variable length decision tree type structure to solve a problem. 

These have two fitness attributes:
- __Traditional fitness:__ accuracy of solution
- __Efficency:__ how long is the solution _"parismony"_

Evolutionary principles thus happen on the candidate programs themselves (the solution trees).
- Crossover is achieved by swapping branches of the various trees.
- Mutation is done by deleting and growing branches.

### Steps to Genetic Programming:
1. Define a set of terminals (explanatory variables and random constants)
2. Define a set of primative functions to be used within each branch 
3. Define a fitness function
4. Set hyperparameters
5. Set a termination criteria and a return structure, so you can get the solution(s) back out.

### Motivations of Genetic Programming:
- When explantory variables are interconnected in an unknown way
- When an approximate solution is sufficient _(always makes me uncomfortable, these tend to be the most permenant)_
- Areas where a solution architecture is unknown and undefined, but a fitness measure is defined.

# Linear Regression
### MLR

### Splines and local models

# Multi Objective Optimisation

# Artifical Neural Networks
