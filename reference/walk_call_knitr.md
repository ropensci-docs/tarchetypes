# Code analysis for knitr reports.

Walk an abstract syntax tree and capture knitr dependencies.

## Usage

``` r
walk_call_knitr(expr, counter)
```

## Arguments

- expr:

  A language object or function to scan.

- counter:

  An internal counter object that keeps track of detected target names
  so far.

## Value

A character vector of target names found during static code analysis.

## Details

For internal use only. Not a user-side function. Powers automatic
detection of
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)/[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html)
dependencies in
[`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md).
Packages `codetools` and `CodeDepends` have different (more
sophisticated and elaborate) implementations of the concepts documented
at <https://adv-r.hadley.nz/expressions.html#ast-funs>.

## Examples

``` r
# How tar_render() really works:
expr <- quote({
  if (a > 1) {
    tar_load(target_name)
  }
  process_stuff(target_name)
})
walk_ast(expr, walk_call_knitr)
#> [1] "target_name"
```
