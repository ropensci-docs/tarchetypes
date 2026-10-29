# Target with an R Markdown document.

Shorthand to include an R Markdown document in a `targets` pipeline.

`tar_render()` expects an unevaluated symbol for the `name` argument,
and it supports named `...` arguments for
[`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html)
arguments. `tar_render_raw()` expects a character string for `name` and
supports an evaluated expression object `render_arguments` for
[`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html)
arguments.

## Usage

``` r
tar_render(
  name,
  path,
  output_file = NULL,
  working_directory = NULL,
  tidy_eval = targets::tar_option_get("tidy_eval"),
  packages = targets::tar_option_get("packages"),
  library = targets::tar_option_get("library"),
  error = targets::tar_option_get("error"),
  memory = targets::tar_option_get("memory"),
  garbage_collection = targets::tar_option_get("garbage_collection"),
  deployment = targets::tar_option_get("deployment"),
  priority = targets::tar_option_get("priority"),
  resources = targets::tar_option_get("resources"),
  retrieval = targets::tar_option_get("retrieval"),
  cue = targets::tar_option_get("cue"),
  description = targets::tar_option_get("description"),
  quiet = TRUE,
  ...
)

tar_render_raw(
  name,
  path,
  output_file = NULL,
  working_directory = NULL,
  packages = targets::tar_option_get("packages"),
  library = targets::tar_option_get("library"),
  error = targets::tar_option_get("error"),
  deployment = targets::tar_option_get("deployment"),
  priority = targets::tar_option_get("priority"),
  resources = targets::tar_option_get("resources"),
  retrieval = targets::tar_option_get("retrieval"),
  cue = targets::tar_option_get("cue"),
  description = targets::tar_option_get("description"),
  quiet = TRUE,
  render_arguments = quote(list())
)
```

## Arguments

- name:

  Name of the target. `tar_render()` expects an unevaluated symbol for
  the `name` argument, whereas `tar_render_raw()` expects a character
  string for `name`.

- path:

  Character string, file path to the R Markdown source file. Must have
  length 1.

- output_file:

  Character string, file path to the rendered output file.

- working_directory:

  Optional character string, path to the working directory to
  temporarily set when running the report. The default is `NULL`, which
  runs the report from the current working directory at the time the
  pipeline is run. This default is recommended in the vast majority of
  cases. To use anything other than `NULL`, you must manually set the
  value of the `store` argument relative to the working directory in all
  calls to
  [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html)
  and
  [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)
  in the report. Otherwise, these functions will not know where to find
  the data.

- tidy_eval:

  Logical, whether to enable tidy evaluation when interpreting `command`
  and `pattern`. If `TRUE`, you can use the "bang-bang" operator `!!` to
  programmatically insert the values of global objects.

- packages:

  Character vector of packages to load right before the target runs or
  the output data is reloaded for downstream targets. Use
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.html)
  to set packages globally for all subsequent targets you define.

- library:

  Character vector of library paths to try when loading `packages`.

- error:

  Character of length 1, what to do if the target stops and throws an
  error. Options:

  - `"stop"`: the whole pipeline stops and throws an error.

  - `"continue"`: the whole pipeline keeps going.

  - `"null"`: The errored target continues and returns `NULL`. The data
    hash is deliberately wrong so the target is not up to date for the
    next run of the pipeline. In addition, as of `targets` version
    1.8.0.9011, a value of `NULL` is given to upstream dependencies with
    `error = "null"` if loading fails.

  - `"abridge"`: any currently running targets keep running, but no new
    targets launch after that.

  - `"trim"`: all currently running targets stay running. A queued
    target is allowed to start if:

    1.  It is not downstream of the error, and

    2.  It is not a sibling branch from the same
        [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
        call (if the error happened in a dynamic branch).

    The idea is to avoid starting any new work that the immediate error
    impacts. `error = "trim"` is just like `error = "abridge"`, but it
    allows potentially healthy regions of the dependency graph to begin
    running. (Visit <https://books.ropensci.org/targets/debugging.html>
    to learn how to debug targets using saved workspaces.)

- memory:

  Character of length 1, memory strategy. Possible values:

  - `"auto"` (default): equivalent to `memory = "transient"` in almost
    all cases. But to avoid superfluous reads from disk,
    `memory = "auto"` is equivalent to `memory = "persistent"` for for
    non-dynamically-branched targets that other targets dynamically
    branch over. For example: if your pipeline has
    `tar_target(name = y, command = x, pattern = map(x))`, then
    `tar_target(name = x, command = f(), memory = "auto")` will use
    persistent memory for `x` in order to avoid rereading all of `x` for
    every branch of `y`.

  - `"transient"`: the target gets unloaded after every new target
    completes. Either way, the target gets automatically loaded into
    memory whenever another target needs the value.

  - `"persistent"`: the target stays in memory until the end of the
    pipeline (unless `storage` is `"worker"`, in which case `targets`
    unloads the value from memory right after storing it in order to
    avoid sending copious data over a network).

  For cloud-based file targets (e.g. `format = "file"` with
  `repository = "aws"`), the `memory` option applies to the temporary
  local copy of the file: `"persistent"` means it remains until the end
  of the pipeline and is then deleted, and `"transient"` means it gets
  deleted as soon as possible. The former conserves bandwidth, and the
  latter conserves local storage.

- garbage_collection:

  Logical: `TRUE` to run [`base::gc()`](https://rdrr.io/r/base/gc.html)
  just before the target runs, in whatever R process it is about to run
  (which could be a parallel worker). `FALSE` to omit garbage
  collection. Numeric values get converted to `FALSE`. The
  `garbage_collection` option in
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.html)
  is independent of the argument of the same name in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html).

- deployment:

  Character of length 1. If `deployment` is `"main"`, then the target
  will run on the central controlling R process. Otherwise, if
  `deployment` is `"worker"` and you set up the pipeline with
  distributed/parallel computing, then the target runs on a parallel
  worker. For more on distributed/parallel computing in `targets`,
  please visit <https://books.ropensci.org/targets/crew.html>.

- priority:

  Deprecated on 2025-04-08 (`targets` version 1.10.1.9013). `targets`
  has moved to a more efficient scheduling algorithm
  (<https://github.com/ropensci/targets/issues/1458>) which cannot
  support priorities. The `priority` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
  no longer has a reliable effect on execution order.

- resources:

  Object returned by
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.html)
  with optional settings for high-performance computing functionality,
  alternative data storage formats, and other optional capabilities of
  `targets`. See
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.html)
  for details.

- retrieval:

  Character string to control when the current target loads its
  dependencies into memory before running. (Here, a "dependency" is
  another target upstream that the current one depends on.) Only
  relevant when using `targets` with parallel workers
  (<https://books.ropensci.org/targets/crew.html>). Must be one of the
  following values:

  - `"auto"` (default): equivalent to `retrieval = "worker"` in almost
    all cases. But to avoid unnecessary reads from disk,
    `retrieval = "auto"` is equivalent to `retrieval = "main"` for
    dynamic branches that branch over non-dynamic targets. For example:
    if your pipeline has `tar_target(x, command = f())`, then
    `tar_target(y, command = x, pattern = map(x), retrieval = "auto")`
    will use `"main"` retrieval in order to avoid rereading all of `x`
    for every branch of `y`.

  - `"worker"`: the worker loads the target's dependencies.

  - `"main"`: the target's dependencies are loaded on the host machine
    and sent to the worker before the target runs.

  - `"none"`: `targets` makes no attempt to load its dependencies. With
    `retrieval = "none"`, loading dependencies is the responsibility of
    the user. Use with caution.

- cue:

  An optional object from
  [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.html)
  to customize the rules that decide whether the target is up to date.

- description:

  Character of length 1, a custom free-form human-readable text
  description of the target. Descriptions appear as target labels in
  functions like
  [`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.html)
  and
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.html),
  and they let you select subsets of targets for the `names` argument of
  functions like
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html).
  For example,
  `tar_manifest(names = tar_described_as(starts_with("survival model")))`
  lists all the targets whose descriptions start with the character
  string `"survival model"`.

- quiet:

  An option to suppress printing during rendering from knitr, pandoc
  command line and others. To only suppress printing of the last "Output
  created: " message, you can set `rmarkdown.render.message` to `FALSE`

- ...:

  Named arguments to
  [`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html).
  These arguments are evaluated when the target actually runs in
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html),
  not when the target is defined. That means, for example, you can use
  upstream targets as parameters of parameterized R Markdown reports.
  The target:

        tar_render(
          your_target,
         "your_report.Rmd",
          params = list(your_param = your_target)
        )

  will run:

        rmarkdown::render(
          "your_report.Rmd",
          params = list(your_param = your_target)
        )

  For parameterized reports, it is recommended to supply a distinct
  `output_file` argument to each `tar_render()` call and set useful
  defaults for parameters in the R Markdown source. See the examples
  section for a demonstration.

- render_arguments:

  Optional language object with a list of named arguments to
  [`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html).
  Cannot be an expression object. (Use
  [`quote()`](https://rdrr.io/r/base/substitute.html), not
  [`expression()`](https://rdrr.io/r/base/expression.html).) The reason
  for quoting is that these arguments may depend on upstream targets
  whose values are not available at the time the target is defined, and
  because `tar_render_raw()` is the "raw" version of a function, we want
  to avoid all non-standard evaluation.

## Value

A target definition object with `format = "file"`. When this target
runs, it returns a character vector of file paths: the rendered
document, the source file, and then the `*_files/` directory if it
exists. Unlike
[`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html),
all returned paths are *relative* paths to ensure portability (so that
the project can be moved from one file system to another without
invalidating the target). See the "Target definition objects" section
for background.

## Details

`tar_render()` is an alternative to
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
for R Markdown reports that depend on other targets. The R Markdown
source should mention dependency targets with
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)
and
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html)
in the active code chunks (which also allows you to render the report
outside the pipeline if the `_targets/` data store already exists). (Do
not use
[`tar_load_raw()`](https://docs.ropensci.org/targets/reference/tar_load.html)
or
[`tar_read_raw()`](https://docs.ropensci.org/targets/reference/tar_read.html)
for this.) Then, `tar_render()` defines a special kind of target. It 1.
Finds all the
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)/[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html)
dependencies in the report and inserts them into the target's command.
This enforces the proper dependency relationships. (Do not use
[`tar_load_raw()`](https://docs.ropensci.org/targets/reference/tar_load.html)
or
[`tar_read_raw()`](https://docs.ropensci.org/targets/reference/tar_read.html)
for this.) 2. Sets `format = "file"` (see
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html))
so `targets` watches the files at the returned paths and reruns the
report if those files change. 3. Configures the target's command to
return both the output report files and the input source file. All these
file paths are relative paths so the project stays portable. 4. Forces
the report to run in the user's current working directory instead of the
working directory of the report. 5. Sets convenient default options such
as `deployment = "main"` in the target and `quiet = TRUE` in
[`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html).

## Literate programming limitations

Literate programming files are messy and variable, so functions like
`tar_render()` have limitations: \* Child documents are not tracked for
changes. \* Upstream target dependencies are not detected if
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html)
and/or
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)
are called from a user-defined function. In addition, single target
names must be mentioned and they must be symbols. `tar_load("x")` and
`tar_load(contains("x"))` may not detect target `x`. \* Special/optional
input/output files may not be detected in all cases. \* `tar_render()`
and friends are for local files only. They do not integrate with the
cloud storage capabilities of `targets`.

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

Other Literate programming targets:
[`tar_knit()`](https://docs.ropensci.org/tarchetypes/reference/tar_knit.md),
[`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md),
[`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md),
[`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({  # tar_dir() runs code from a temporary directory.
# Unparameterized R Markdown:
lines <- c(
  "---",
  "title: report.Rmd source file",
  "output_format: html_document",
  "---",
  "Assume these lines are in report.Rmd.",
  "```{r}",
  "targets::tar_read(data)",
  "```"
)
# Include the report in a pipeline as follows.
targets::tar_script({
  library(tarchetypes)
  list(
    tar_target(data, data.frame(x = seq_len(26), y = letters)),
    tar_render(report, "report.Rmd")
  )
}, ask = FALSE)
# Then, run the targets pipeline as usual.

# Parameterized R Markdown:
lines <- c(
  "---",
  "title: 'report.Rmd source file with parameters'",
  "output_format: html_document",
  "params:",
  "  your_param: \"default value\"",
  "---",
  "Assume these lines are in report.Rmd.",
  "```{r}",
  "print(params$your_param)",
  "```"
)
# Include the report in the pipeline as follows.
targets::tar_script({
  library(tarchetypes)
  list(
    tar_target(data, data.frame(x = seq_len(26), y = letters)),
    tar_render(
      name = report,
      "report.Rmd",
      params = list(your_param = data)
    ),
    tar_render_raw(
      name = "report2",
      "report.Rmd",
      params = quote(list(your_param = data))
    )
  )
}, ask = FALSE)
})
# Then, run the targets pipeline as usual.
}
```
