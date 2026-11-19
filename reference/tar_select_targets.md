# Select target definition objects from a target list

Select target definition objects from a target list.

## Usage

``` r
tar_select_targets(targets, ...)
```

## Arguments

- targets:

  A list of target definition objects as described in the "Target
  definition objects" section. It does not matter how nested the list is
  as long as the only leaf nodes are targets.

- ...:

  One or more comma-separated `tidyselect` expressions, e.g.
  `starts_with("prefix")`. Just like `...` in
  [`dplyr::select()`](https://dplyr.tidyverse.org/reference/select.html).

## Value

A list of target definition objects. See the "Target definition objects"
section of this help file.

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

Other target selection:
[`tar_select_names()`](https://docs.ropensci.org/tarchetypes/reference/tar_select_names.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
targets <- list(
  list(
    targets::tar_target(x, 1),
    targets::tar_target(y1, 2)
  ),
  targets::tar_target(y2, 3),
  targets::tar_target(z, 4)
)
tar_select_targets(targets, starts_with("y"), contains("z"))
})
}
```
