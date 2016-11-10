<!-- README.md is generated from README.Rmd. Please edit that file -->
Withr - Run Code 'With' Modified State
======================================

[![Travis-CI Build Status](https://travis-ci.org/jimhester/withr.svg?branch=master)](https://travis-ci.org/jimhester/withr) [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/jimhester/withr?branch=master&svg=true)](https://ci.appveyor.com/project/jimhester/withr) [![Coverage Status](https://img.shields.io/codecov/c/github/jimhester/withr/master.svg)](https://codecov.io/github/jimhester/withr?branch=master) [![CRAN Version](http://www.r-pkg.org/badges/version/withr)](http://www.r-pkg.org/pkg/withr)

A set of functions to run code 'with' safely and temporarily modified global state. There are two sets of functions, those prefixed with `with_` and those with `scoped_`. The former reset their state as soon as the `code` argument has been evaluated. The latter reset when they reach the end of their scope, usually at the end of a function body.

Many of these functions were originally a part of the [devtools](https://github.com/hadley/devtools) package, this provides a simple package with limited dependencies to provide access to these functions.

-   `with_collate()` / `scoped_collate()` - collation order
-   `with_dir()` / `scoped_dir()` - working directory
-   `with_envvar()` / `scoped_envvar()` - environment variables
-   `with_libpaths()` / `scoped_libpaths()` - library paths
-   `with_locale()` / `scoped_locale()` - any locale setting
-   `with_makevars()` / `scoped_makevars()` - Makevars variables
-   `with_options()` / `scoped_options()` - options
-   `with_par()` / `scoped_par()` - graphics parameters
-   `with_path()` / `scoped_path()` - PATH environment variable

There are also `with_()` and `scoped_()` functions to construct new `with_*` and `scoped_*` functions if needed.

``` r
dir.create("test")
#> Warning in dir.create("test"): 'test' already exists
getwd()
#> [1] "/private/var/folders/dt/r5s12t392tb5sk181j3gs4zw0000gn/T/RtmpRNDrWs"
with_dir("test", getwd())
#> [1] "/private/var/folders/dt/r5s12t392tb5sk181j3gs4zw0000gn/T/RtmpRNDrWs/test"
getwd()
#> [1] "/private/var/folders/dt/r5s12t392tb5sk181j3gs4zw0000gn/T/RtmpRNDrWs"
unlink("test")

Sys.getenv("HADLEY")
#> [1] ""
with_envvar(c("HADLEY" = 2), Sys.getenv("HADLEY"))
#> [1] "2"
Sys.getenv("HADLEY")
#> [1] ""

with_envvar(c("A" = 1),
  with_envvar(c("A" = 2), action = "suffix", Sys.getenv("A"))
)
#> [1] "1 2"
```

See Also
========

-   [Devtools](https://github.com/hadley/devtools)
