# Render a batch of parameterized R Markdown reports inside a `tar_render_rep()` target.

Internal function needed for
[`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md).
Users should not invoke it directly.

## Usage

``` r
tar_render_rep_run(path, params, args, deps, rep_workers)
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
  [`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md).

- rep_workers:

  Positive integer of length 1, number of local R processes to use to
  run reps within batches in parallel. If 1, then reps are run
  sequentially within each batch. If greater than 1, then reps within
  batch are run in parallel using a PSOCK cluster.

## Value

Character vector with the path to the R Markdown source file and the
rendered output file. Both paths depend on the input source path, and
they have no defaults.
