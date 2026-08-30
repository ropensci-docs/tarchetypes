# Quarto file detection

Detect the important files in a Quarto project.

## Usage

``` r
tar_quarto_files(path = ".", profile = NULL, quiet = TRUE)
```

## Arguments

- path:

  Character of length 1, either the file path to a Quarto source
  document or the directory path to a Quarto project. Defaults to the
  Quarto project in the current working directory.

- profile:

  Character of length 1, Quarto profile. If `NULL`, the default profile
  will be used. Requires Quarto version 1.2 or higher. See
  <https://quarto.org/docs/projects/profiles.html> for details.

- quiet:

  Suppress warning and other messages, from R and also Quarto CLI (i.e
  `--quiet` is passed as command line).

  `quarto.quiet` R option or `R_QUARTO_QUIET` environment variable can
  be used to globally override a function call (This can be useful to
  debug tool that calls `quarto_*` functions directly).

  On Github Actions, it will always be `quiet = FALSE`.

## Value

A named list of important file paths in a Quarto project or document:

- `sources`: source files which may reference upstream target
  dependencies in code chunks using
  [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)/[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html).

- `output`: output files that will be generated during
  [`quarto::quarto_render()`](https://quarto-dev.github.io/quarto-r/reference/quarto_render.html).

- `input`: pre-existing files required to render the project or
  document, such as `_quarto.yml` and quarto extensions.

## Details

This function is just a thin wrapper that interprets the output of
[`quarto::quarto_inspect()`](https://quarto-dev.github.io/quarto-r/reference/quarto_inspect.html)
and returns what `tarchetypes` needs to know about the current Quarto
project or document.

## See also

Other Literate programming utilities:
[`tar_knitr_deps()`](https://docs.ropensci.org/tarchetypes/reference/tar_knitr_deps.md),
[`tar_knitr_deps_expr()`](https://docs.ropensci.org/tarchetypes/reference/tar_knitr_deps_expr.md)

## Examples

``` r
lines <- c(
  "---",
  "title: source file",
  "---",
  "Assume these lines are in report.qmd.",
  "```{r}",
  "1 + 1",
  "```"
)
path <- tempfile(fileext = ".qmd")
writeLines(lines, path)
# If Quarto is installed, run:
# tar_quarto_files(path)
```
