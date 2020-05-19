# bbnam.mix
Code to Fit a Multi-informant Bayesian Network Accuracy Model (BNAM) with Graph Mixture Priors

This simple R script contains the core function needed to reproduce the graph mixture prior BNAM of Butts (2014), along with the separate estimation of self-report and proxy reporting errors employed by Lee and Butts (2018,2020).  This function is based on the bbnam.actor function of the sna package for R, with which it will eventually be integrated.  (However, a stand-alone version may be useful in the interim.)

Butts, C. T. (2014). Baseline mixture models for social networks. arXiv:1710.7402773.

Lee, F., and Butts, C. T. (2020).  On the validity of perceived social structure.  Journal of Mathematical Psychology, forthcoming.

Lee, F., and Butts, C. T. (2018). Mutual assent or unilateral nomination? A performance comparison of intersection and union rules for integrating self-reports of social relationships.  Social Networks, 55, 55–62.
