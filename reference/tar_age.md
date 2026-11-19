# Create a target that runs when the last run gets old

`tar_age()` creates a target that reruns itself when it gets old enough.
In other words, the target reruns periodically at regular intervals of
time.

## Usage

``` r
tar_age(
  name,
  command,
  age,
  pattern = NULL,
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

  Name of the target.
  [`tar_cue_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md)
  expects an unevaluated symbol for the `name` argument, whereas
  [`tar_cue_age_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md)
  expects a character string for `name`.

- command:

  R code to run the target and return a value.

- age:

  A `difftime` object of length 1, such as
  `as.difftime(3, units = "days")`. If the target's output data files
  are older than `age` (according to the most recent time stamp over all
  the target's output files) then the target will rerun. On the other
  hand, if at least one data file is younger than `Sys.time() - age`,
  then the ordinary invalidation rules apply, and the target may or not
  rerun. If you want to force the target to run every 3 days, for
  example, set `age = as.difftime(3, units = "days")`.

- pattern:

  Code to define a dynamic branching branching for a target. In
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html),
  `pattern` is an unevaluated expression, e.g.
  `tar_target(pattern = map(data))`. In
  [`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.html),
  `command` is an evaluated expression, e.g.
  `tar_target_raw(pattern = quote(map(data)))`.

  To demonstrate dynamic branching patterns, suppose we have a pipeline
  with numeric vector targets `x` and `y`. Then,
  `tar_target(z, x + y, pattern = map(x, y))` implicitly defines
  branches of `z` that each compute `x[1] + y[1]`, `x[2] + y[2]`, and so
  on. See the user manual for details.

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

  Logical, whether to rerun the target if the user-specified storage
  format changed. The storage format is user-specified through
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
  or
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.html).

- repository:

  Logical, whether to rerun the target if the user-specified storage
  repository changed. The storage repository is user-specified through
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
  or
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.html).

- iteration:

  Logical, whether to rerun the target if the user-specified iteration
  method changed. The iteration method is user-specified through
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
  or
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.html).

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

  A
  [`targets::tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.html)
  object. (See the "Cue objects" section for background.) This cue
  object should contain any optional secondary invalidation rules,
  anything except the `mode` argument. `mode` will be automatically
  determined by the `age` argument of `tar_age()`.

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

A target definition object. See the "Target definition objects" section
for background.

## Details

`tar_age()` uses the cue from
[`tar_cue_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md),
which uses the time stamps from `targets::tar_meta()$time`. See the help
file of
[`targets::tar_timestamp()`](https://docs.ropensci.org/targets/reference/tar_timestamp.html)
for an explanation of how this time stamp is calculated.

## Dynamic branches at regular time intervals

Time stamps are not recorded for whole dynamic targets, so `tar_age()`
is not a good fit for dynamic branching. To invalidate dynamic branches
at regular intervals, it is recommended to use
[`targets::tar_older()`](https://docs.ropensci.org/targets/reference/tar_older.html)
in combination with
[`targets::tar_invalidate()`](https://docs.ropensci.org/targets/reference/tar_invalidate.html)
right before calling
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html).
For example, the call:

      tar_invalidate(
        any_of(
          tar_older(Sys.time - as.difftime(1, units = "weeks"))
        )
      )

invalidates all targets more than a week old. Then, the next
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html)
will rerun those targets.

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

Other cues:
[`tar_cue_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md),
[`tar_cue_force()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_force.md),
[`tar_cue_skip()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_skip.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
targets::tar_script({
  library(tarchetypes)
  list(
    tarchetypes::tar_age(
      data,
      data.frame(x = seq_len(26)),
      age = as.difftime(0.5, units = "secs")
    )
  )
})
targets::tar_make()
Sys.sleep(0.6)
targets::tar_make()
})
}
```
