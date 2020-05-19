# bbnam.mix
**Code to Fit a Multi-informant Bayesian Network Accuracy Model (BNAM) with Graph Mixture Priors**

This simple R script contains the core function needed to reproduce the graph mixture prior BNAM of Butts (2014), along with the separate estimation of self-report and proxy reporting errors employed by Lee and Butts (2018,2020).  This function is based on the bbnam.actor function of the `sna` package for R, with which it will eventually be integrated.  (However, this stand-alone version may be useful in the interim.)

## Installation

Simply put the `mixture.code.R` file in a convenient location, and use `source("/path/to/mixture.code.R")` within R to load.  (Note that the `sna` package for R is required.)

## Usage

    bbnam.mix(dat, nprior = NULL, emprior = c(1, 11), epprior = c(1, 11), 
        diag = FALSE, mode = "digraph", reps = 5, draws = 1500, 
        burntime = 500, quiet = TRUE, anames = NULL, onames = NULL, 
        compute.sqrtrhat = TRUE) 


### Arguments:

+ `dat`: Input networks to be analyzed.  This may be supplied in any
reasonable form, but must be reducible to an array of
dimension *m x n x n*, where *n* is *|V(G)|*, the first dimension
indexes the observer (or information source), the second
indexes the sender of the relation, and the third dimension
indexes the recipient of the relation.  (E.g.,
`dat[i,j,k]==1` implies that `i` observed `j` sending the
relation in question to `k`.)  Note that only dichotomous data
is supported at present, and missing values are permitted;
the data collection pattern, however, is assumed to be
ignorable, and hence the posterior draws are implicitly
conditional on the observation pattern.  (Note: to estimate
self-report and proxy report error rates separately, 
provide two entries for each informant, one in which the
informant's own row and column are set as missing, and
another in which every entry other than those in the
informant's own row and column are set as missing.  This
will both treat self and proxy reports separately and
estimate distinct false positive and false negative rates
for the self and proxy reports produced by each informant.)

+ `nprior`: Network prior hyperparameters.  This should be a vector of
length 2 (for beta-Bernoulli graphs) or length 3 (for
Dirichlet-categorical graphs) containing the hyperparameters
for the graph mixture distribution.  For the beta-Binomial
model, these can be thought of as the alpha and beta parameters
for a beta hyperprior distribution on the expected graph
density.  For the Dirichlet-categorical model, these can be
thought as the three concentration parameters governing a
three-dimensional Dirichlet distribution over the expected
rates of incidence for mutual, asymmetric, and null dyads
(respectively) in the network prior.  In particular, note
that choosing `c(0.5,0.5)` or `c(0.5,0.5,0.5)` will employ the
Jeffreys hyperprior for each respective case; this is the
default.

+ `emprior`: Parameters for the (Beta) false negative prior; these should
be in the form of an *n x 2* matrix of (alpha,beta) pairs  (or something which can be converted to this form). If no
`emprior` is given, a weakly informative prior (1,11) will be
assumed.  Missing values are not allowed.

+ `epprior`: Parameters for the (Beta) false positive prior; these should
be in the form of an *n x 2* matrix of (alpha,beta) pairs (or something which can be converted to this form). If no
`epprior` is given, a weakly informative prior (1,11) will be
assumed.  Missing values are not allowed.

+ `diag`: Boolean indicating whether loops (matrix diagonals) should be
counted as data.

+  `mode`: A string indicating whether the data in question forms a
 `"graph"` or a `"digraph"`.

+ `reps`: Number of replicate chains for the Gibbs sampler.

+ `draws`: Integer indicating the total number of draws to take from the
posterior distribution.  Draws are taken evenly from each
replication (thus, the number of draws from a given chain is
`draws/reps`).

+ `burntime`: Integer indicating the burn-in time for the Markov Chain.
Each replication is iterated burntime times before taking
draws (with these initial iterations being discarded); hence,
one should realize that each increment to burntime increases
execution time by a quantity proportional to `reps`.

+ `quiet`: Boolean indicating whether MCMC diagnostics should be
displayed.

+ `anames`: A vector of names for the actors (vertices) in the graph.

+ `onames`: A vector of names for the observers (possibly the actors
themselves) whose reports are contained in the input data.

+ `compute.sqrtrhat`: A boolean indicating whether or not Gelman et al.'s
potential scale reduction measure (an MCMC convergence
diagnostic) should be computed.

### Return value:
  An object of class `bbnam.actor`.  (See the `sna` package.)


## References

Butts, C. T. (2014). Baseline mixture models for social networks. arXiv:1710.7402773.

Lee, F., and Butts, C. T. (2020).  On the validity of perceived social structure.  *Journal of Mathematical Psychology,* forthcoming.

Lee, F., and Butts, C. T. (2018). Mutual assent or unilateral nomination? A performance comparison of intersection and union rules for integrating self-reports of social relationships.  *Social Networks,* 55, 55–62.
