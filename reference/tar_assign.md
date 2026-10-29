# An assignment-based pipeline DSL

An assignment-based domain-specific language for pipeline construction.

## Usage

``` r
tar_assign(targets)
```

## Arguments

- targets:

  An expression with special syntax to define a collection of targets in
  a pipeline. Example: `tar_assign(x <- tar_target(get_data()))` is
  equivalent to `list(tar_target(x, get_data()))`. The rules of the
  syntax are as follows:

  - The code supplied to `tar_assign()` must be enclosed in curly braces
    beginning with `{` and `}` unless it only contains a one-line
    statement or uses `=` as the assignment.

  - Each statement in the code block must be of the form `x <- f()`, or
    `x = f()` where `x` is the name of a target and `f()` is a function
    like
    [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
    or
    [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
    which accepts a `name` argument.

  - The native pipe operator `|>` is allowed because it lazily evaluates
    its arguments and be converted into non-pipe syntax without
    evaluating the code.

## Value

A list of target definition objects. See the "Target definition objects"
section for background.

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

Other Domain-specific languages for pipeline construction:
[`tar_tangle()`](https://docs.ropensci.org/tarchetypes/reference/tar_tangle.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
write.csv(airquality, "data.csv", row.names = FALSE)
targets::tar_script({
  library(tarchetypes)
  tar_option_set(packages = c("readr", "dplyr", "ggplot2"))
  tar_assign({
    file <- tar_target("data.csv", format = "file")

    data <- read_csv(file, col_types = cols()) |>
      filter(!is.na(Ozone)) |>
      tar_target()

    model = lm(Ozone ~ Temp, data) |>
      coefficients() |>
      tar_target()

    plot <- {
        ggplot(data) +
          geom_point(aes(x = Temp, y = Ozone)) +
          geom_abline(intercept = model[1], slope = model[2]) +
          theme_gray(24)
      } |>
        tar_target()
  })
})
targets::tar_make()
})
}
```
