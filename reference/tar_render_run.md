# Render an R Markdown report inside a `tar_render()` target.

Internal function needed for
[`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md).
Users should not invoke it directly.

## Usage

``` r
tar_render_run(path, args, deps)
```

## Arguments

- path:

  Path to the R Markdown source file.

- args:

  A named list of arguments to
  [`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html).

- deps:

  An unnamed list of target dependencies of the R Markdown report,
  automatically created by
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md).

## Value

Character vector with the path to the R Markdown source file and the
relative path to the output. These paths depend on the input source file
path and have no defaults.
