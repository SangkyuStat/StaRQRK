# StaRQR-K

This repository includes an R function implemented using Rcpp named as `mrqscr_cpp`. This function is made for ultrahigh-dimensional feature screening with Regional Quantile Regression model, where we can screen important features across the quantile region of interests, denoted as $\Delta$. 

The function can be used via `sourceCpp` from `Rcpp` as follows:
```
library(Rcpp)
sourceCpp("mrqscr.cpp")
```

This can be utilized in the following way, with a small example:
```
set.seed(100)
n <- 100 # number of observations
p <- 1000 # number of variables
tau <- seq(0.45, 0.55, length.out = 5) # sequence of quantile levels
rho <- 0.5 # corr
corr_mat <- rho^abs(outer(1:p, 1:p, "-"))

x <- mvnfast::rmvn(n, rep(0, p), corr_mat) # utilize mvnfast to generate x quickly
x[,p] <- rbinom(n, 1, 0.5)
eps <- rnorm(n)
y <- 2*x[,1] + 2*x[,2] + 3*x[,5] + 2.5*x[,p]*eps

mrqscr_cpp(y = y, # input for the response vector y
           x_mat = x, # input for the matrix x
           tau_vec = tau, # input for the sequence of quantile levels
           order = 3) # order controls the order for the B-spline basis
```
