# Convert Quarto or R Markdown to a pipeline

Convert a literate programming source file into a `targets` pipeline.

## Usage

``` r
tar_tangle(path)
```

## Arguments

- path:

  File path to the literate programming source file. The file can be a
  Quarto or R Markdown document.

## Value

A list of new target definition objects. See the "Target definition
objects" section for background.

## Details

The word "tangle" comes from the early days of literate programming (see
Knuth 1984). To "tangle" means to convert a literate programming source
document into pure code: code that a human may find cryptic but a
machine can run. For example, `knitr::knit(tangle = TRUE)` (and
[`knitr::purl()`](https://rdrr.io/pkg/knitr/man/knit.html)) accepts an
`.Rmd` file and returns an R script with all the R code chunks pasted
together.

`tar_tangle()` is similar, but for a `targets` pipeline. It accepts a
Quarto or R Markdown source file as input, and it returns a list of
target definition objects. Each target definition object comes from
evaluating
[`targets::tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
on the each of the assignment statements in each R code chunk in the
file.

For example, consider the following code chunk:

      ```{r, deployment = "main"}
      #| pattern: map(data)
      #| format: qs
      #| cue: tar_cue(mode = "always")
      data <- get_data(data)
      ```

`tar_tangle()` converts this chunk into:

      tar_target(
        name = target_name,
        command = command_to_run(data),
        pattern = map(data),
        format = "qs",
        deployment = "main",
        cue = tar_cue(mode = "always")
      )

To put it all together, suppose our `_targets.R` script for the pipeline
looks like this:

      library(targets)
      tar_source()
      list(
        tar_tangle("example.qmd"),
        tar_target(model, fit_model(data))
      )

The pipeline above is equivalent to:

      library(targets)
      tar_source()
      list(
        tar_target(
          name = target_name,
          command = command_to_run(data),
          pattern = map(data),
          format = "qs",
          deployment = "main",
          cue = tar_cue(mode = "always")
        ),
        tar_target(model, fit_model(data))
      )

This pattern is a nice compromise between interactivity and automation:
you can run the whole pipeline with
[`targets::tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html),
or you can explore individual code chunks in the report using an IDE
like RStudio, Positron, or VSCode. However, there is a chance other
tools like Quarto or `pkgdown` may automatically detect the report and
inappropriately try to run the whole thing from end to end. To prevent
this, you may wish to write `knitr::opts_chunk$set(eval = FALSE)` in a
code chunk at the top of the report.

See the "Examples" section in this help file for a runnable
demonstration with multiple code chunks.

Each code chunk can have more than one top-level assignment statement
(with `<-`, `=`, or `->`), and each assignment statement gets converted
into its own target. Non-assignment statements such as
[`library(dplyr)`](https://dplyr.tidyverse.org) are ignored. It is
always good practice to check your pipeline with
[`targets::tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.html)
and
[`targets::tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.html)
before running it with
[`targets::tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html).

## References

- Knuth, Donald E. (1984). "Literate Programming". The Computer Journal.
  27 (2). British Computer Society: 97-11. <doi:10.1093/comjnl/27.2.97>.

## See also

Other Domain-specific languages for pipeline construction:
[`tar_assign()`](https://docs.ropensci.org/tarchetypes/reference/tar_assign.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({  # tar_dir() runs code from a temporary directory.
write.csv(airquality, "data.csv")
lines <- c(
  "---",
  "title: \"Example pipeline\"",
  "---",
  "",
  "```{r}",
  "knitr::opts_chunk$set(eval = FALSE)",
  "```",
  "",
  "```{r}",
  "#| format: file",
  "file <- \"data.csv\"",
  "```",
  "",
  "```{r}",
  "#| memory: persistent",
  "#| packages: [dplyr, readr]",
  "data <- read_csv(file, col_types = cols()) |>",
  "  filter(!is.na(Ozone))",
  "```",
  "",
  "```{r}",
  "#| format: qs",
  "#| cue: tar_cue(mode = \"never\")",
  "model <- lm(Ozone ~ Temp, data) |>",
  "  coefficients()",
  "```",
  "",
  "```{r}",
  "#| deployment: main",
  "#| packages: ggplot2",
  "plot <- ggplot(data) +",
  "  geom_point(aes(x = Temp, y = Ozone)) +",
  "  geom_abline(intercept = model[1], slope = model[2]) +",
  "  theme_gray(24)",
  "```"
)
writeLines(lines, "pipeline.qmd")
targets::tar_script(tarchetypes::tar_tangle("pipeline.qmd"))
targets::tar_make()
targets::tar_read(plot)
})
}
```
