# Parameterized Quarto with dynamic branching.

Targets to render a parameterized Quarto document with multiple sets of
parameters. Assumes you do not specify `output-dir` in `_quarto.yml`.

`tar_quarto_rep()` expects an unevaluated symbol for the `name` argument
and an unevaluated expression for the `execute_params` argument.
`tar_quarto_rep_raw()` expects a character string for the `name`
argument and an evaluated expression object for the `execute_params`
argument.

## Usage

``` r
tar_quarto_rep(
  name,
  path,
  working_directory = NULL,
  execute_params = data.frame(),
  batches = NULL,
  extra_files = character(0),
  execute = TRUE,
  cache = NULL,
  cache_refresh = FALSE,
  debug = FALSE,
  quiet = TRUE,
  quarto_args = NULL,
  pandoc_args = NULL,
  rep_workers = 1,
  tidy_eval = targets::tar_option_get("tidy_eval"),
  packages = targets::tar_option_get("packages"),
  library = targets::tar_option_get("library"),
  format = targets::tar_option_get("format"),
  iteration = targets::tar_option_get("iteration"),
  error = targets::tar_option_get("error"),
  memory = targets::tar_option_get("memory"),
  garbage_collection = targets::tar_option_get("garbage_collection"),
  deployment = targets::tar_option_get("deployment"),
  priority = targets::tar_option_get("priority"),
  resources = targets::tar_option_get("resources"),
  retrieval = targets::tar_option_get("retrieval"),
  cue = targets::tar_option_get("cue"),
  description = targets::tar_option_get("description")
)

tar_quarto_rep_raw(
  name,
  path,
  working_directory = NULL,
  execute_params = expression(NULL),
  batches = NULL,
  extra_files = character(0),
  execute = TRUE,
  cache = NULL,
  cache_refresh = FALSE,
  debug = FALSE,
  quiet = TRUE,
  quarto_args = NULL,
  pandoc_args = NULL,
  rep_workers = 1,
  packages = targets::tar_option_get("packages"),
  library = targets::tar_option_get("library"),
  format = targets::tar_option_get("format"),
  iteration = targets::tar_option_get("iteration"),
  error = targets::tar_option_get("error"),
  memory = targets::tar_option_get("memory"),
  garbage_collection = targets::tar_option_get("garbage_collection"),
  deployment = targets::tar_option_get("deployment"),
  priority = targets::tar_option_get("priority"),
  resources = targets::tar_option_get("resources"),
  retrieval = targets::tar_option_get("retrieval"),
  cue = targets::tar_option_get("cue"),
  description = targets::tar_option_get("description")
)
```

## Arguments

- name:

  Name of the target. `tar_quarto_rep()` expects an unevaluated symbol
  for the `name` argument, and `tar_quarto_rep_raw()` expects a
  character string for `name`.

- path:

  Character string, path to the Quarto source file if rendering a single
  file, or the path to the root of the project if rendering a whole
  Quarto project.

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

- execute_params:

  Code to generate a data frame or `tibble` with one row per rendered
  report and one column per Quarto parameter. `tar_quarto_rep()` expects
  an unevaluated expression for the `execute_params` argument, whereas
  `tar_quarto_rep_raw()` expects an evaluated expression object.

  You may also include an `output_file` column in the parameters to
  specify the path of each rendered report. If included, the
  `output_file` column must be a character vector with one and only one
  output file for each row of parameters. If an `output_file` column is
  not included, then the output files are automatically determined using
  the parameters, and the default file format is determined by the YAML
  front-matter of the Quarto source document. Only the first file format
  is used, the others are not generated. Quarto parameters must not be
  named `tar_group` or `output_file`. This `execute_params` argument is
  converted into the command for a target that supplies the Quarto
  parameters.

- batches:

  Number of batches. This is also the number of dynamic branches created
  during
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html).

- extra_files:

  Character vector of extra files and directories to track for changes.
  The target will be invalidated (rerun on the next
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html))
  if the contents of these files changes. No need to include anything
  already in the output of
  [`tar_quarto_files()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_files.md),
  the list of file dependencies automatically detected through
  [`quarto::quarto_inspect()`](https://quarto-dev.github.io/quarto-r/reference/quarto_inspect.html).

- execute:

  Whether to execute embedded code chunks.

- cache:

  Cache execution output (uses knitr cache and jupyter-cache
  respectively for Rmd and Jupyter input files).

- cache_refresh:

  Force refresh of execution cache.

- debug:

  Leave intermediate files in place after render.

- quiet:

  Suppress warning and other messages, from R and also Quarto CLI (i.e
  `--quiet` is passed as command line).

  `quarto.quiet` R option or `R_QUARTO_QUIET` environment variable can
  be used to globally override a function call (This can be useful to
  debug tool that calls `quarto_*` functions directly).

  On Github Actions, it will always be `quiet = FALSE`.

- quarto_args:

  Character vector of other `quarto` CLI arguments to append to the
  Quarto command executed by this function. This is mainly intended for
  advanced usage and useful for CLI arguments which are not yet mirrored
  in a dedicated parameter of this R function. See
  `quarto render --help` for options.

- pandoc_args:

  Additional command line arguments to pass on to Pandoc.

- rep_workers:

  Positive integer of length 1, number of local R processes to use to
  run reps within batches in parallel. If 1, then reps are run
  sequentially within each batch. If greater than 1, then reps within
  batch are run in parallel using a PSOCK cluster.

- tidy_eval:

  Logical of length 1, whether to use tidy evaluation to resolve
  `execute_params`. Similar to the `tidy_eval` argument of
  [`targets::tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html).

- packages:

  Character vector of packages to load right before the target runs or
  the output data is reloaded for downstream targets. Use
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.html)
  to set packages globally for all subsequent targets you define.

- library:

  Character vector of library paths to try when loading `packages`.

- format:

  Optional storage format for the target's return value. With the
  exception of `format = "file"`, each target gets a file in
  `_targets/objects`, and each format is a different way to save and
  load this file. See the "Storage formats" section for a detailed list
  of possible data storage formats.

- iteration:

  Character of length 1, name of the iteration mode of the target.
  Choices:

  - `"vector"`: branching happens with `vectors::vec_slice()` and
    aggregation happens with
    [`vctrs::vec_c()`](https://vctrs.r-lib.org/reference/vec_c.html).

  - `"list"`, branching happens with `[[]]` and aggregation happens with
    [`list()`](https://rdrr.io/r/base/list.html). In the case of list
    iteration, `tar_read(your_target)` will return a list of lists,
    where the outer list has one element per batch and each inner list
    has one element per rep within batch. To un-batch this nested list,
    call `tar_read(your_target, recursive = FALSE)`.

  - `"group"`:
    [`dplyr::group_by()`](https://dplyr.tidyverse.org/reference/group_by.html)-like
    functionality to branch over subsets of a data frame. The target's
    return value must be a data frame with a special `tar_group` column
    of consecutive integers from 1 through the number of groups. Each
    integer designates a group, and a branch is created for each
    collection of rows in a group. See the
    [`tar_group()`](https://docs.ropensci.org/targets/reference/tar_group.html)
    function in `targets` to see how you can create the special
    `tar_group` column with
    [`dplyr::group_by()`](https://dplyr.tidyverse.org/reference/group_by.html).

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

## Value

A list of target definition objects to render the Quarto reports.
Changes to the parameters, source file, dependencies, etc. will cause
the appropriate targets to rerun during
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html).
See the "Target definition objects" section for background.

## Details

`tar_quarto_rep()` is an alternative to
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
for a parameterized Quarto document that depends on other targets.
Parameters must be given as a data frame with one row per rendered
report and one column per parameter. An optional `output_file` column
may be included to set the output file path of each rendered report.
(See the `execute_params` argument for details.)

The Quarto source should mention other dependency targets
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)
and
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html)
in the active code chunks (which also allows you to render the report
outside the pipeline if the `_targets/` data store already exists and
appropriate defaults are specified for the parameters). (Do not use
[`tar_load_raw()`](https://docs.ropensci.org/targets/reference/tar_load.html)
or
[`tar_read_raw()`](https://docs.ropensci.org/targets/reference/tar_read.html)
for this.) Then,
[`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
defines a special kind of target. It 1. Finds all the
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
return the output report files: the rendered document, the source file,
and file paths mentioned in `files`. All these file paths are relative
paths so the project stays portable. 4. Forces the report to run in the
user's current working directory instead of the working directory of the
report. 5. Sets convenient default options such as `deployment = "main"`
in the target and `quiet = TRUE` in
[`quarto::quarto_render()`](https://quarto-dev.github.io/quarto-r/reference/quarto_render.html).

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

## Replicate-specific seeds

In ordinary pipelines, each target has its own unique deterministic
pseudo-random number generator seed derived from its target name. In
batched replicate, however, each batch is a target with multiple
replicate within that batch. That is why
[`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)
and friends give each *replicate* its own unique seed. Each
replicate-specific seed is created based on the dynamic parent target
name, `tar_option_get("seed")` (for `targets` version 0.13.5.9000 and
above), batch index, and rep-within-batch index. The seed is set just
before the replicate runs. Replicate-specific seeds are invariant to
batching structure. In other words,
`tar_rep(name = x, command = rnorm(1), batches = 100, reps = 1, ...)`
produces the same numerical output as
`tar_rep(name = x, command = rnorm(1), batches = 10, reps = 10, ...)`
(but with different batch names). Other target factories with this seed
scheme are
[`tar_rep2()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep2.md),
[`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md),
[`tar_map2_count()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_count.md),
[`tar_map2_size()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_size.md),
and
[`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md).
For the `tar_map2_*()` functions, it is possible to manually supply your
own seeds through the `command1` argument and then invoke them in your
custom code for `command2`
([`set.seed()`](https://rdrr.io/r/base/Random.html),
[`withr::with_seed`](https://withr.r-lib.org/reference/with_seed.html),
or
[`withr::local_seed()`](https://withr.r-lib.org/reference/with_seed.html)).
For
[`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md),
custom seeds can be supplied to the `params` argument and then invoked
in the individual R Markdown reports. Likewise with `tar_quarto_rep()`
and the `execute_params` argument.

## Literate programming limitations

Literate programming files are messy and variable, so functions like
[`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)
have limitations: \* Child documents are not tracked for changes. \*
Upstream target dependencies are not detected if
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html)
and/or
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)
are called from a user-defined function. In addition, single target
names must be mentioned and they must be symbols. `tar_load("x")` and
`tar_load(contains("x"))` may not detect target `x`. \* Special/optional
input/output files may not be detected in all cases. \*
[`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)
and friends are for local files only. They do not integrate with the
cloud storage capabilities of `targets`.

## Quarto troubleshooting

If you encounter difficult errors, please read
<https://github.com/quarto-dev/quarto-r/issues/16>. In addition, please
try to reproduce the error using
`quarto::quarto_render("your_report.qmd", execute_dir = getwd())`
without using `targets` at all. Isolating errors this way makes them
much easier to solve.

## See also

Other Literate programming targets:
[`tar_knit()`](https://docs.ropensci.org/tarchetypes/reference/tar_knit.md),
[`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md),
[`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md),
[`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
# Parameterized Quarto:
lines <- c(
  "---",
  "title: 'report.qmd file'",
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
# The following pipeline will run the report for each row of params.
targets::tar_script({
  library(tarchetypes)
  list(
    tar_quarto_rep(
      name = report,
      path = "report.qmd",
      execute_params = tibble::tibble(par = c(1, 2))
    ),
    tar_quarto_rep_raw(
      name = "report",
      path = "report.qmd",
      execute_params = quote(tibble::tibble(par = c(1, 2)))
    )
  )
}, ask = FALSE)
# Then, run the targets pipeline as usual.
})
}
```
