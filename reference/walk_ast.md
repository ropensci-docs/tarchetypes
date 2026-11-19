# Static code analysis for `tarchetypes`.

Walk an abstract syntax tree and capture data.

## Usage

``` r
walk_ast(expr, walk_call)
```

## Arguments

- expr:

  A language object or function to scan.

- walk_call:

  A function to handle a specific kind of function call relevant to the
  code analysis at hand.

## Value

A character vector of data found during static code analysis.

## Details

For internal use only. Not a user-side function. Powers functionality
like automatic detection of
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
# Custom code analysis for developers of tarchetypes internals:
walk_custom <- function(expr, counter) {
  # New internals should use targets::tar_deparse_safe(backtick = FALSE).
  name <- deparse(expr[[1]])
  if (identical(name, "detect_this")) {
    counter_set_names(counter, as.character(expr[[2]]))
  }
}
expr <- quote({
  for (i in seq_len(10)) {
    for (j in seq_len(20)) {
      if (i > 1) {
        detect_this("prize")
      } else {
        ignore_this("penalty")
      }
    }
  }
})
walk_ast(expr, walk_custom)
#> [1] "prize"
```
