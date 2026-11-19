# Dynamic-within-static branching for data frames (size batching).

Define targets for batched dynamic-within-static branching for data
frames, where the user sets the (maximum) size of each batch.

`tar_map2_size()` expects unevaluated language for arguments `name`,
`command1`, `command2`, `columns1`, and `columns2`.
`tar_map2_size_raw()` expects a character string for `name` and an
evaluated expression object for each of `command1`, `command2`,
`columns1`, and `columns2`.

## Usage

``` r
tar_map2_size(
  name,
  command1,
  command2,
  values = NULL,
  names = NULL,
  descriptions = tidyselect::everything(),
  size = Inf,
  combine = TRUE,
  suffix1 = "1",
  suffix2 = "2",
  columns1 = tidyselect::everything(),
  columns2 = tidyselect::everything(),
  rep_workers = 1,
  delimiter = "_",
  unlist = FALSE,
  tidy_eval = targets::tar_option_get("tidy_eval"),
  packages = targets::tar_option_get("packages"),
  library = targets::tar_option_get("library"),
  format = targets::tar_option_get("format"),
  repository = targets::tar_option_get("repository"),
  error = targets::tar_option_get("error"),
  memory = targets::tar_option_get("memory"),
  garbage_collection = targets::tar_option_get("garbage_collection"),
  deployment = targets::tar_option_get("deployment"),
  priority = targets::tar_option_get("priority"),
  resources = targets::tar_option_get("resources"),
  storage = targets::tar_option_get("storage"),
  retrieval = targets::tar_option_get("retrieval"),
  cue = targets::tar_option_get("cue"),
  description = targets::tar_option_get("description")
)

tar_map2_size_raw(
  name,
  command1,
  command2,
  values = NULL,
  names = NULL,
  descriptions = quote(tidyselect::everything()),
  size = Inf,
  combine = TRUE,
  suffix1 = "1",
  suffix2 = "2",
  columns1 = quote(tidyselect::everything()),
  columns2 = quote(tidyselect::everything()),
  rep_workers = 1,
  delimiter = "_",
  unlist = FALSE,
  tidy_eval = targets::tar_option_get("tidy_eval"),
  packages = targets::tar_option_get("packages"),
  library = targets::tar_option_get("library"),
  format = targets::tar_option_get("format"),
  repository = targets::tar_option_get("repository"),
  error = targets::tar_option_get("error"),
  memory = targets::tar_option_get("memory"),
  garbage_collection = targets::tar_option_get("garbage_collection"),
  deployment = targets::tar_option_get("deployment"),
  priority = targets::tar_option_get("priority"),
  resources = targets::tar_option_get("resources"),
  storage = targets::tar_option_get("storage"),
  retrieval = targets::tar_option_get("retrieval"),
  cue = targets::tar_option_get("cue"),
  description = targets::tar_option_get("description")
)
```

## Arguments

- name:

  Name of the target.
  [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)
  expects unevaluated `name` and `command` arguments (e.g.
  `tar_rep(name = sim, command = simulate())`) whereas
  [`tar_rep_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)
  expects an evaluated string for `name` and an evaluated expression
  object for `command` (e.g.
  `tar_rep_raw(name = "sim", command = quote(simulate()))`).

- command1:

  R code to create named arguments to `command2`. Must return a data
  frame with one row per call to `command2` when run.

  In regular `tarchetypes` functions, the `command1` argument is an
  unevaluated expression. In the `"_raw"` versions of functions,
  `command1` is an evaluated expression object.

- command2:

  R code to map over the data frame of arguments produced by `command1`.
  Must return a data frame.

  In regular `tarchetypes` functions, the `command2` argument is an
  unevaluated expression. In the `"_raw"` versions of functions,
  `command2` is an evaluated expression object.

- values:

  Named list or data frame with values to iterate over. The names are
  the names of symbols in the commands and pattern statements, and the
  elements are values that get substituted in place of those symbols.
  [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md)
  uses these elements to create new R code, so they should be basic
  types, symbols, or R expressions. For objects even a little bit
  complicated, especially objects with attributes, it is not obvious how
  to convert the object into code that generates it. For complicated
  objects, consider using
  [`quote()`](https://rdrr.io/r/base/substitute.html) when you define
  `values`, as shown at
  <https://github.com/ropensci/tarchetypes/discussions/105>.

- names:

  Subset of `names(values)` used to generate the suffixes in the names
  of the new targets. The value of `names` should be a `tidyselect`
  expression such as a call to
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).

- descriptions:

  Names of a column in `values` to append to the custom description of
  each generated target. The value of `descriptions` should be a
  `tidyselect` expression such as a call to
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).

- size:

  Positive integer of length 1, maximum number of rows in each batch for
  the downstream (`command2`) targets. Batches are formed from row
  groups of the `command1` target output.

- combine:

  Logical of length 1, whether to create additional downstream targets
  to combine the results of static branches. The `values` argument must
  not be `NULL` for this combining to take effect. If `combine` is
  `TRUE` and `values` is not `NULL`, then separate targets aggregate all
  dynamic branches within each static branch, and then a final target
  combines all the static branches together.

- suffix1:

  Character of length 1, suffix to apply to the `command1` targets to
  distinguish them from the `command2` targets.

- suffix2:

  Character of length 1, suffix to apply to the `command2` targets to
  distinguish them from the `command1` targets.

- columns1:

  A tidyselect expression to select which columns of `values` to append
  to the output of all targets. Columns already in the target output are
  not appended.

  In regular `tarchetypes` functions, the `columns1` argument is an
  unevaluated expression. In the `"_raw"` versions of functions,
  `columns1` is an evaluated expression object.

- columns2:

  A tidyselect expression to select which columns of `command1` output
  to append to `command2` output. Columns already in the target output
  are not appended. `columns1` takes precedence over `columns2`.

  In regular `tarchetypes` functions, the `columns2` argument is an
  unevaluated expression. In the `"_raw"` versions of functions,
  `columns2` is an evaluated expression object.

- rep_workers:

  Positive integer of length 1, number of local R processes to use to
  run reps within batches in parallel. If 1, then reps are run
  sequentially within each batch. If greater than 1, then reps within
  batch are run in parallel using a PSOCK cluster.

- delimiter:

  Character of length 1, string to insert between other strings when
  creating names of targets.

- unlist:

  Logical, whether to flatten the returned list of targets. If
  `unlist = FALSE`, the list is nested and sub-lists are named and
  grouped by the original input targets. If `unlist = TRUE`, the return
  value is a flat list of targets named by the new target names.

- tidy_eval:

  Whether to invoke tidy evaluation (e.g. the `!!` operator from
  `rlang`) as soon as the target is defined (before
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html)).
  Applies to the `command` argument.

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

- repository:

  Character of length 1, remote repository for target storage. Choices:

  - `"local"`: file system of the local machine.

  - `"aws"`: Amazon Web Services (AWS) S3 bucket. Can be configured with
    a non-AWS S3 bucket using the `endpoint` argument of
    [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.html),
    but versioning capabilities may be lost in doing so. See the cloud
    storage section of <https://books.ropensci.org/targets/data.html>
    for details for instructions.

  - `"gcp"`: Google Cloud Platform storage bucket. See the cloud storage
    section of <https://books.ropensci.org/targets/data.html> for
    details for instructions.

  - A character string from
    [`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.html)
    for content-addressable storage.

  Note: if `repository` is not `"local"` and `format` is `"file"` then
  the target should create a single output file. That output file is
  uploaded to the cloud and tracked for changes where it exists in the
  cloud. As of `targets` version 1.11.0 and higher, the local file is no
  longer deleted after the target runs.

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

- storage:

  Character string to control when the output of the target is saved to
  storage. Only relevant when using `targets` with parallel workers
  (<https://books.ropensci.org/targets/crew.html>). Must be one of the
  following values:

  - `"worker"` (default): the worker saves/uploads the value.

  - `"main"`: the target's return value is sent back to the host machine
    and saved/uploaded locally.

  - `"none"`: `targets` makes no attempt to save the result of the
    target to storage in the location where `targets` expects it to be.
    Saving to storage is the responsibility of the user. Use with
    caution.

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

A list of new target definition objects. See the "Target definition
objects" section for background.

## Details

Static branching creates one pair of targets for each row in `values`.
In each pair, there is an upstream non-dynamic target that runs
`command1` and a downstream dynamic target that runs `command2`.
`command1` produces a data frame of arguments to `command2`, and
`command2` dynamically maps over these arguments in batches.

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
`tar_map2_size()`, and
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
in the individual R Markdown reports. Likewise with
[`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
and the `execute_params` argument.

## See also

Other branching:
[`tar_map2()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2.md),
[`tar_map2_count()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_count.md),
[`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md),
[`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md),
[`tar_rep2()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep2.md),
[`tar_rep_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_map.md),
[`tar_rep_map_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_map_raw.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
targets::tar_script({
  tarchetypes::tar_map2_size(
    x,
    command1 = tibble::tibble(
      arg1 = arg1,
      arg2 = seq_len(6)
     ),
    command2 = tibble::tibble(
      result = paste(arg1, arg2),
      random = sample.int(1e9, size = 1),
      length_input = length(arg1)
    ),
    values = tibble::tibble(arg1 = letters[seq_len(2)]),
    size = 2
   )
})
targets::tar_make()
targets::tar_read(x)
# With tar_map2_size_raw():
targets::tar_script({
  tarchetypes::tar_map2_size_raw(
    name = "x",
    command1 = quote(
      tibble::tibble(
        arg1 = arg1,
        arg2 = seq_len(6)
      )
    ),
    command2 = quote(
      tibble::tibble(
        result = paste(arg1, arg2),
        random = sample.int(1e9, size = 1),
        length_input = length(arg1)
      )
    ),
    values = tibble::tibble(arg1 = letters[seq_len(2)]),
    size = 2
   )
})
})
}
```
