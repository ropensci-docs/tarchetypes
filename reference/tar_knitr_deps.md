# List literate programming dependencies.

List the target dependencies of one or more literate programming reports
(R Markdown or `knitr`).

## Usage

``` r
tar_knitr_deps(path)
```

## Arguments

- path:

  Character vector, path to one or more R Markdown or `knitr` reports.

## Value

Character vector of the names of targets that are dependencies of the
`knitr` report.

## See also

Other Literate programming utilities:
[`tar_knitr_deps_expr()`](https://docs.ropensci.org/tarchetypes/reference/tar_knitr_deps_expr.md),
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
tar_knitr_deps(report)
#> [1] "data1" "data2"
```
