# A `drake`-plan-like pipeline DSL

Simplify target specification in pipelines.

## Usage

``` r
tar_plan(...)
```

## Arguments

- ...:

  Named and unnamed targets. All named targets must follow the
  `drake`-plan-like `target = command` syntax, and all unnamed arguments
  must be explicit calls to create target definition objects, e.g.
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html),
  target factories like
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md),
  or similar.

## Value

A list of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
objects. See the "Target definition objects" section for background.

## Details

Allows targets with just targets and commands to be written in the
pipeline as `target = command` instead of `tar_target(target, command)`.
Also supports ordinary target definition objects if they are unnamed.
`tar_plan(x = 1, y = 2, tar_target(z, 3), tar_render(r, "r.Rmd"))` is
equivalent to:

      list(
        tar_target(x, 1),
        tar_target(y, 2),
        tar_target(z, 3),
        tar_render(r, "r.Rmd")
      )

## Target definition objects

Most `tarchetypes` functions are target factories, which means they
return target definition objects or lists of target definition objects.
target definition objects represent skippable steps of the analysis
pipeline as described at <https://books.ropensci.org/targets/>. Please
read the walkthrough at
<https://books.ropensci.org/targets/walkthrough.html> to understand the
role of target definition objects in analysis pipelines.

For developers,
<https://wlandau.github.io/targetopia/contributing.html#target-factories>
explains target factories (functions like this one which generate
targets) and the design specification at
<https://books.ropensci.org/targets-design/> details the structure and
composition of target definition objects.

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
targets::tar_script({
  library(tarchetypes)
  tar_plan(
    tarchetypes::tar_fst_tbl(data, data.frame(x = seq_len(26))),
    means = colMeans(data) # No need for tar_target() for simple cases.
  )
})
targets::tar_make()
})
}
```
