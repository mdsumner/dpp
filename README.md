<!-- README.md is generated from README.Rmd. Please edit that file -->
dpp
===

dplyr for paired calculations

dpp depends on dplyr and imports proj4, and can be installed with

``` r
devtools::install_github("mdsumner/dpp")
```

In the function `simd` I apply a geographic transformation on two columns x,y in a `tbl_df`. To do this I extract the columns as a matrix and pass them to proj4::project, then pull them back to copy over the original longitude/latitude x,y columns.

The function is used like this:

``` r
library(dpp) 
x <- simd(n = 10, p = "+proj=laea")
```

and it works like this:

``` r
  x <- data_frame(x = runif(n, -180, 180), y = runif(n, -90, 90), id = seq(n))
  if (!is.null(p)) {
    m <- as.matrix(x[,c("x", "y")])
    m <- project(m, p)
    x$x <- m[,1L]
    x$y <- m[,2L]
  }
```

The question: is there a more dplyr-ish way to apply this paired variable mutation, without extracting to the matrix? Ultimately I'd like to have project methods for objects composed of tbl\_dfs, so perhaps I need to write a new version of proj4::project that works more deeply with dplyr?
