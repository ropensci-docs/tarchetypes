# Run a `knitr` report inside a `tar_knit()` target.

Internal function needed for
[`tar_knit()`](https://docs.ropensci.org/tarchetypes/reference/tar_knit.md).
Users should not invoke it directly.

## Usage

``` r
tar_knit_run(path, working_directory, args, deps)
```

## Arguments

- path:

  Character string, file path to the `knitr` source file. Must have
  length 1.

- working_directory:

  Optional character string, path to the working directory to
  temporarily set when running the report. The default is `NULL`, which
  runs the report from the current working directory at the time the
  pipeline is run. This default is recommended in the vast majority of
  cases. To use anything other than `NULL`, you must manually set the
  value of the `store` argument relative to the working directory in all
  calls to
  [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html)
  and
  [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)
  in the report. Otherwise, these functions will not know where to find
  the data.

- args:

  A named list of arguments to
  [`knitr::knit()`](https://rdrr.io/pkg/knitr/man/knit.html).

- deps:

  An unnamed list of target dependencies of the `knitr` report,
  automatically created by
  [`tar_knit()`](https://docs.ropensci.org/tarchetypes/reference/tar_knit.md).

## Value

Character with the path to the `knitr` source file and the relative path
to the output `knitr` report. The output path depends on the input path
argument, which has no default.
