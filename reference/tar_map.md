# Static branching.

Define multiple new targets based on existing target definition objects.

## Usage

``` r
tar_map(
  values,
  ...,
  names = tidyselect::everything(),
  descriptions = tidyselect::everything(),
  unlist = FALSE,
  delimiter = "_"
)
```

## Arguments

- values:

  Named list or data frame with values to iterate over. The names are
  the names of symbols in the commands and pattern statements, and the
  elements are values that get substituted in place of those symbols.
  `tar_map()` uses these elements to create new R code, so they should
  be basic types, symbols, or R expressions. For objects even a little
  bit complicated, especially objects with attributes, it is not obvious
  how to convert the object into code that generates it. For complicated
  objects, consider using
  [`quote()`](https://rdrr.io/r/base/substitute.html) when you define
  `values`, as shown at
  <https://github.com/ropensci/tarchetypes/discussions/105>.

- ...:

  One or more target definition objects or list of target definition
  objects. Lists can be arbitrarily nested, as in
  [`list()`](https://rdrr.io/r/base/list.html).

- names:

  Subset of `names(values)` used to generate the suffixes in the names
  of the new targets. The value of `names` should be a `tidyselect`
  expression such as a call to
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).

- descriptions:

  Names of a column in `values` to append to the custom description of
  each generated target. The value of `descriptions` should be a
  `tidyselect` expression such as a call to
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).

- unlist:

  Logical, whether to flatten the returned list of targets. If
  `unlist = FALSE`, the list is nested and sub-lists are named and
  grouped by the original input targets. If `unlist = TRUE`, the return
  value is a flat list of targets named by the new target names.

- delimiter:

  Character of length 1, string to insert between other strings when
  creating names of targets.

## Value

A list of new target definition objects. If `unlist` is `FALSE`, the
list is nested and sub-lists are named and grouped by the original input
targets. If `unlist = TRUE`, the return value is a flat list of targets
named by the new target names. See the "target definition objects"
section for background.

## Details

`tar_map()` creates collections of new targets by iterating over a list
of arguments and substituting symbols into commands and pattern
statements.

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

## See also

Other static branching:
[`tar_combine()`](https://docs.ropensci.org/tarchetypes/reference/tar_combine.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
targets::tar_script({
  list(
    tarchetypes::tar_map(
      list(a = c(12, 34), b = c(45, 78)),
      targets::tar_target(x, a + b),
      targets::tar_target(y, x + a, pattern = map(x))
    )
  )
})
targets::tar_manifest()
})
}
```
