# Render a Quarto project inside a `tar_quarto()` target.

Internal function needed for
[`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md).
Users should not invoke it directly.

## Usage

``` r
tar_quarto_run(args, deps, sources, output, input)
```

## Arguments

- args:

  A named list of arguments to
  [`quarto::quarto_render()`](https://quarto-dev.github.io/quarto-r/reference/quarto_render.html).

- deps:

  An unnamed list of target dependencies of the Quarto source files.

- sources:

  Character vector of Quarto source files.

- output:

  Character vector of Quarto output files and directories.

- input:

  Character vector of non-source Quarto input files and directories.

## Value

Sorted character vector with the paths to all the important files that
`targets` should track for changes.

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({  # tar_dir() runs code from a temporary directory.
# Unparameterized Quarto document:
lines <- c(
  "---",
  "title: Quarto source file",
  "output_format: html",
  "---",
  "Assume these lines are in the Quarto source file.",
  "```{r}",
  "1 + 1",
  "```"
)
tmp <- tempfile(fileext = ".qmd")
writeLines(lines, tmp)
args <- list(input = tmp, quiet = TRUE)
files <- fs::path_ext_set(tmp, "html")
tar_quarto_run(args = args, deps = list(), files = files)
file.exists(files)
})
}
```
