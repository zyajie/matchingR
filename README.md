matchingR: Efficient Computation of the Gale-Shapley Algorithm in R and C++ [![Build Status](https://travis-ci.org/jtilly/matchingR.png)](https://travis-ci.org/jtilly/matchingR) [![Coverage Status](https://coveralls.io/repos/jtilly/matchingR/badge.svg?branch=master)](https://coveralls.io/r/jtilly/matchingR?branch=master)
===============

matchingR is an R Package that quickly computes the Gale-Shapley Algorithm for large matching markets. This package can be useful when the number of market participants is large or when very many matchings need to be computed (e.g. for extensive simulations or for estimation purposes). The package has successfully been used to simulate preferences and compute the matching with 30,000 participants on each side of the market.

Installation
------------

This package can be installed from [CRAN](http://cran.r-project.org/web/packages/matchingR/):
```
install.packages("matchingR")
```

## Documentation
* [Package Documentation](http://jtilly.io/matchingR/matchingR-documentation.pdf)
* [Vignette: Introduction to matchingR](http://jtilly.io/matchingR/matchingR-intro.pdf)

### Example: Marriage Market
The following is an example of `one2one` with different numbers of participants on each side of the market. By construction, 500 men will remain unmatched.
```{r}
# set seed
set.seed(1)
# set number of men
nmen = 2500
# set number of women
nwomen = 2000

# generate preferences
uM = matrix(runif(nmen*nwomen), nrow=nmen, ncol=nwomen) 
uW = matrix(runif(nmen*nwomen), nrow=nwomen, ncol=nmen) 

# male optimal matching
resultsM = one2one(uM, uW)
# female optimal matching
resultsW = one2one(uW, uM)

# check if matchings are stable
checkStability(uM, uW, resultsM$proposals, resultsM$engagements)
checkStability(uW, uM, resultsW$proposals, resultsW$engagements)
```

### Example: College Admissions Problem
The following is an example of `one2many` where 1000 students get matched to 400 colleges, where each college has two slots. By construction, 200 students will remain unmatched.
```{r}
# set seed
set.seed(1)
# set number of workers
nstudents = 1000
# set number of colleges
ncolleges = 400

# generate preferences
uStudents = matrix(runif(ncolleges*nstudents), nrow=nstudents, ncol=ncolleges) 
uColleges = matrix(runif(nstudents*ncolleges), nrow=ncolleges, ncol=nstudents) 

# worker optimal matching
results = one2many(uStudents, uColleges, slots=2)

# check if matching is stable
checkStability(uStudents, uColleges, results$proposals, results$engagements)
```
