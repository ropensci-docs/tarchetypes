# Expression with literate programming dependencies.

Construct an expression whose global variable dependencies are the
target dependencies of one or more literate programming reports (R
Markdown or `knitr`). This helps third-party developers create their own
third-party target factories for literate programming targets (similar
to
[`tar_knit()`](https://docs.ropensci.org/tarchetypes/reference/tar_knit.md)
and
[`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)).

## Usage

``` r
tar_knitr_deps_expr(path)
```

## Arguments

- path:

  Character vector, path to one or more R Markdown or `knitr` reports.

## Value

Expression object to name the dependency targets of the `knitr` report,
which will be detected in the static code analysis of `targets`.

## See also

Other Literate programming utilities:
[`tar_knitr_deps()`](https://docs.ropensci.org/tarchetypes/reference/tar_knitr_deps.md),
[`tar_quarto_files()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_files.md)

## Examples

``` r
lines <- c(
  "---",
  "title: report",
  "output_format: html_document",
  "---",
  "",
  "```{r}",
  "targets::tar_load(data1)",
  "targets::tar_read(data2)",
  "```"
)
report <- tempfile()
writeLines(lines, report)
tar_knitr_deps_expr(report)
#> list(data1, data2)
```
