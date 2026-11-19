# Render a batch of parameterized Quarto reports inside a `tar_quarto_rep()` target.

Internal function needed for
[`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md).
Users should not invoke it directly.

## Usage

``` r
tar_quarto_rep_run(
  args,
  execute_params,
  extra_files,
  deps,
  default_output_file,
  rep_workers
)
```

## Arguments

- args:

  A named list of arguments to
  [`quarto::quarto_render()`](https://quarto-dev.github.io/quarto-r/reference/quarto_render.html).

- execute_params:

  A data frame of Quarto parameters to branch over.

- extra_files:

  Character vector of extra files that `targets` should track for
  changes. If the content of one of these files changes, then the report
  will rerun over all the parameters on the next
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html).
  These files are *extra* files, and they do not include the Quarto
  source document or rendered output document, which are already tracked
  for changes. Examples include bibliographies, style sheets, and
  supporting image files.

- deps:

  An unnamed list of target dependencies of the Quarto report,
  automatically created by
  [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md).

- default_output_file:

  Output file path determined by the YAML front-matter of the Quarto
  source document. Automatic output file names are based on this file.

- rep_workers:

  Positive integer of length 1, number of local R processes to use to
  run reps within batches in parallel. If 1, then reps are run
  sequentially within each batch. If greater than 1, then reps within
  batch are run in parallel using a PSOCK cluster.

## Value

Character vector with the path to the Quarto source file and the
rendered output file. Both paths depend on the input source path, and
they have no defaults.

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
# Parameterized Quarto:
lines <- c(
  "---",
  "title: 'report.qmd source file'",
  "output_format: html_document",
  "params:",
  "  par: \"default value\"",
  "---",
  "Assume these lines are in a file called report.qmd.",
  "```{r}",
  "print(params$par)",
  "```"
)
writeLines(lines, "report.qmd") # In tar_dir(), not the user's file space.
args <- list(
  input = "report.qmd",
  execute = TRUE,
  execute_dir = quote(getwd()),
  execute_daemon = 0,
  execute_daemon_restart = FALSE,
  execute_debug = FALSE,
  cache = FALSE,
  cache_refresh = FALSE,
  debug = FALSE,
  quiet = TRUE,
  as_job = FALSE
)
execute_params <- tibble::tibble(
  par = c("non-default value 1", "non-default value 2"),
  output_file = c("report1.html", "report2.html")
)
tar_quarto_rep_run(
  args = args,
  execute_params = execute_params,
  extra_files = character(0),
  deps = NULL,
  default_output_file = "report_default.html"
)
})
}
```
