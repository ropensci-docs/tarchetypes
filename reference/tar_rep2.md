# Dynamic batched computation downstream of [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)

Batching is important for optimizing the efficiency of heavily
dynamically-branched workflows:
<https://books.ropensci.org/targets/dynamic.html#batching>. `tar_rep2()`
uses dynamic branching to iterate over the batches and reps of existing
upstream targets.

`tar_rep2()` expects unevaluated language for the `name`, `command`, and
`...` arguments (e.g.
`tar_rep2(name = sim, command = simulate(), data1, data2)`) whereas
`tar_rep2_raw()` expects an evaluated string for `name`, an evaluated
expression object for `command`, and a character vector for `targets`
(e.g.
`tar_rep2_raw("sim", quote(simulate(x, y)), targets = c("x', "y"))`).

## Usage

``` r
tar_rep2(
  name,
  command,
  ...,
  rep_workers = 1,
  tidy_eval = targets::tar_option_get("tidy_eval"),
  packages = targets::tar_option_get("packages"),
  library = targets::tar_option_get("library"),
  format = targets::tar_option_get("format"),
  repository = targets::tar_option_get("repository"),
  iteration = targets::tar_option_get("iteration"),
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

tar_rep2_raw(
  name,
  command,
  targets,
  rep_workers = 1,
  tidy_eval = targets::tar_option_get("tidy_eval"),
  packages = targets::tar_option_get("packages"),
  library = targets::tar_option_get("library"),
  format = targets::tar_option_get("format"),
  repository = targets::tar_option_get("repository"),
  iteration = targets::tar_option_get("iteration"),
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

  Name of the target. `tar_rep2()` expects unevaluated language for the
  `name`, `command`, and `...` arguments (e.g.
  `tar_rep2(name = sim, command = simulate(), data1, data2)`) whereas
  `tar_rep2_raw()` expects an evaluated string for `name`, an evaluated
  expression object for `command`, and a character vector for `targets`
  (e.g.
  `tar_rep2_raw("sim", quote(simulate(x, y)), targets = c("x', "y"))`).

- command:

  R code to run multiple times. Must return a list or data frame because
  [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)
  will try to append new elements/columns `tar_batch` and `tar_rep` to
  the output to denote the batch and rep-within-batch IDs, respectively.

  `tar_rep2()` expects unevaluated language for the `name`, `command`,
  and `...` arguments (e.g.
  `tar_rep2(name = sim, command = simulate(), data1, data2)`) whereas
  `tar_rep2_raw()` expects an evaluated string for `name`, an evaluated
  expression object for `command`, and a character vector for `targets`
  (e.g.
  `tar_rep2_raw("sim", quote(simulate(x, y)), targets = c("x', "y"))`).

- ...:

  Symbols to name one or more upstream batched targets created by
  [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md).
  If you supply more than one such target, all those targets must have
  the same number of batches and reps per batch. And they must all
  return either data frames or lists. List targets must use
  `iteration = "list"` in
  [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md).

- rep_workers:

  Positive integer of length 1, number of local R processes to use to
  run reps within batches in parallel. If 1, then reps are run
  sequentially within each batch. If greater than 1, then reps within
  batch are run in parallel using a PSOCK cluster.

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

- iteration:

  Character of length 1, name of the iteration mode of the target.
  Choices:

  - `"vector"`: branching happens with
    [`vctrs::vec_slice()`](https://vctrs.r-lib.org/reference/vec_slice.html)
    and aggregation happens with
    [`vctrs::vec_c()`](https://vctrs.r-lib.org/reference/vec_c.html).

  - `"list"`, branching happens with `[[]]` and aggregation happens with
    [`list()`](https://rdrr.io/r/base/list.html).

  - `"group"`:
    [`dplyr::group_by()`](https://dplyr.tidyverse.org/reference/group_by.html)-like
    functionality to branch over subsets of a non-dynamic data frame.
    For `iteration = "group"`, the target must not by dynamic (the
    `pattern` argument of
    [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
    must be left `NULL`). The target's return value must be a data frame
    with a special `tar_group` column of consecutive integers from 1
    through the number of groups. Each integer designates a group, and a
    branch is created for each collection of rows in a group. See the
    [`tar_group()`](https://docs.ropensci.org/targets/reference/tar_group.html)
    function to see how you can create the special `tar_group` column
    with
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

- targets:

  Character vector of names of upstream batched targets created by
  [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md).
  If you supply more than one such target, all those targets must have
  the same number of batches and reps per batch. And they must all
  return either data frames or lists. List targets must use
  `iteration = "list"` in
  [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md).

## Value

A new target definition object to perform batched computation. See the
"Target definition objects" section for background.

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
scheme are `tar_rep2()`,
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
in the individual R Markdown reports. Likewise with
[`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
and the `execute_params` argument.

## See also

Other branching:
[`tar_map2()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2.md),
[`tar_map2_count()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_count.md),
[`tar_map2_size()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_size.md),
[`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md),
[`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md),
[`tar_rep_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_map.md),
[`tar_rep_map_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_map_raw.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
targets::tar_script({
  library(tarchetypes)
  list(
    tar_rep(
      data1,
      data.frame(value = rnorm(1)),
      batches = 2,
      reps = 3
    ),
    tar_rep(
      data2,
      list(value = rnorm(1)),
      batches = 2, reps = 3,
      iteration = "list" # List iteration is important for batched lists.
    ),
    tar_rep2(
      aggregate,
      data.frame(value = data1$value + data2$value),
      data1,
      data2
    ),
    tar_rep2_raw(
      "aggregate2",
      quote(data.frame(value = data1$value + data2$value)),
      targets = c("data1", "data2")
    )
  )
})
targets::tar_make()
targets::tar_read(aggregate)
})
}
```
