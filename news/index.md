# Changelog

## tarchetypes 0.14.1

CRAN release: 2026-03-24

- Stay compatible with Quarto CLI \>= 1.9.0. At some point between
  version 1.6.0 and 1.9.36, `quarto inspect` started returning relative
  paths instead of absolute paths under `fileInformation$includeMap`.
  This changed the output of
  [`quarto::quarto_inspect()`](https://quarto-dev.github.io/quarto-r/reference/quarto_inspect.html)
  in a way that broke `tarchetypes::tar_quarto_file()`. This version of
  `tarchetypes` fixes that issue for newer versions of Quarto while
  trying to stay compatible with older versions. The cutoff for
  `tarchetypes` is 1.9.0.

## tarchetypes 0.14.0

CRAN release: 2026-02-09

- Try different paths to look for output files in
  [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  ([\#217](https://github.com/ropensci/tarchetypes/issues/217)). Hard to
  tell if Quarto uses the current working directory or the directory of
  the source file as the destination for the output, and their policy
  may have changed in a different version of Quarto.
- Add
  [`tar_tangle()`](https://docs.ropensci.org/tarchetypes/reference/tar_tangle.md)
  ([\#226](https://github.com/ropensci/tarchetypes/issues/226)).
- Avoid Quarto parallel rendering bug with a workaround in
  [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  ([\#230](https://github.com/ropensci/tarchetypes/issues/230),
  [@ant-durrant](https://github.com/ant-durrant)).

## tarchetypes 0.13.2

CRAN release: 2025-09-13

- Disable `targets` internal processing progress bars in
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)
  and
  [`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md).
- Add `quarto` and `rmarkdown` to `packages` for
  [`targets::tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.html)
  (<https://github.com/ropensci/targets/issues/1506>,
  [@valentingar](https://github.com/valentingar)).
- Set `deployment = "main"` in the batch target of
  [`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md).

## tarchetypes 0.13.1

CRAN release: 2025-05-08

- Restore custom output files in
  [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  ([\#217](https://github.com/ropensci/tarchetypes/issues/217),
  [@arcruz0](https://github.com/arcruz0)). For
  [\#211](https://github.com/ropensci/tarchetypes/issues/211), users
  should write relative paths to output files in the `output_file`
  column of `execute_params`, and they should avoid writing a
  `_quarto.yml` with an `output-dir` field.
- Set `deployment = targets::tar_option_get("deployment")` in
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)
  and
  [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md).
- Set `cli.num_colors = 1L` and `cli.dynamic = FALSE` in R Markdown
  target factories.

## tarchetypes 0.13.0

CRAN release: 2025-04-10

- Support `pattern` in
  [`tar_skip()`](https://docs.ropensci.org/tarchetypes/reference/tar_skip.md)
  ([\#212](https://github.com/ropensci/tarchetypes/issues/212),
  [@CorradoLanera](https://github.com/CorradoLanera)).
- Allow
  [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  to write reports to subdirectories with the help of a project-level
  `_quarto.yml`
  ([\#211](https://github.com/ropensci/tarchetypes/issues/211),
  [@lgaborini](https://github.com/lgaborini)).
- `tar_map2*()` functions now aggregate dynamic branches in parallel
  over static branches
  ([\#213](https://github.com/ropensci/tarchetypes/issues/213)).
- `tar_map2*()` functions gain an `unlist` argument.
- Call
  [`parallel::clusterExport()`](https://rdrr.io/r/parallel/clusterApply.html)
  in `make_psock_cluster()` to make sure globals carry over to parallel
  socket clusters.

## tarchetypes 0.12.0

CRAN release: 2025-01-31

- Fix
  [`tar_combine()`](https://docs.ropensci.org/tarchetypes/reference/tar_combine.md)
  help file examples
  ([\#206](https://github.com/ropensci/tarchetypes/issues/206),
  [@weberse2](https://github.com/weberse2)).
- Account for project-level `output_dir` in non-project
  [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  calls ([\#207](https://github.com/ropensci/tarchetypes/issues/207),
  [@brndngrhm](https://github.com/brndngrhm)).
- Use name when passing the `quiet` argument to `quarto_inspect()`
  ([\#208](https://github.com/ropensci/tarchetypes/issues/208),
  [@yusuke-sasaki-jprep](https://github.com/yusuke-sasaki-jprep)).
- Explicitly pass the `profile` argument to `quarto_inspect()` and
  `quarto_render()`. Requires R package `quarto >= 1.4`, which is
  already in the `DESCIRPTION`.
- [`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md)
  now aggregates dynamic branches in parallel over static branches
  ([\#204](https://github.com/ropensci/tarchetypes/issues/204)).
- Add
  [`tar_rep_index()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_index.md)
  ([\#203](https://github.com/ropensci/tarchetypes/issues/203)).

## tarchetypes 0.11.0

CRAN release: 2024-11-15

- Add an `output_file` argument to
  [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  and
  [`tar_quarto_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  for single documents
  ([\#198](https://github.com/ropensci/tarchetypes/issues/198),
  [@mutlusun](https://github.com/mutlusun)).
- Detect child quarto documents
  ([\#199](https://github.com/ropensci/tarchetypes/issues/199),
  [@mutlusun](https://github.com/mutlusun)).
- Improve reporting of static branch names from
  [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md)
  and
  [`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md)
  ([\#201](https://github.com/ropensci/tarchetypes/issues/201),
  [@kkmann](https://github.com/kkmann)).
- Ensure compatibility with `targets` after
  <https://github.com/ropensci/targets/issues/1368>.
- Improve detection of Quarto files in `tar_quarto_inspect()`
  ([\#200](https://github.com/ropensci/tarchetypes/issues/200),
  [@multusun](https://github.com/multusun)).

## tarchetypes 0.10.0

CRAN release: 2024-09-26

- Add a `delimiter` argument to
  [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md)
  etc. for customizing separators in target names
  ([\#177](https://github.com/ropensci/tarchetypes/issues/177),
  [@psychelzh](https://github.com/psychelzh)).
- Add “raw” hook functions
  ([\#185](https://github.com/ropensci/tarchetypes/issues/185),
  [@multimeric](https://github.com/multimeric)).
- Add
  [`tar_assign()`](https://docs.ropensci.org/tarchetypes/reference/tar_assign.md)
  ([\#186](https://github.com/ropensci/tarchetypes/issues/186),
  <https://github.com/ropensci/targets/issues/1309>,
  [@hadley](https://github.com/hadley)).
- Merge help files of “\_raw” functions
  ([\#191](https://github.com/ropensci/tarchetypes/issues/191),
  [@hadley](https://github.com/hadley)).
- Supersede
  [`tar_format_feather()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats_superseded.md)
  in favor of
  [`tar_arrow_feather()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  ([\#190](https://github.com/ropensci/tarchetypes/issues/190)).
- Supersede the `tar_aws_*()` target factories. They are obsolete
  because of the `repository` argument in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html).

## tarchetypes 0.9.0

CRAN release: 2024-04-17

### Invalidating changes

- To align with <https://github.com/ropensci/targets/issues/1244> and
  <https://github.com/ropensci/targets/pull/1262>, switch the hashing
  functions from
  [`digest::digest()`](https://eddelbuettel.github.io/digest/man/digest.html)
  to
  [`secretbase::siphash13()`](https://shikokuchuo.net/secretbase/reference/siphash13.html).

## tarchetypes 0.8.0

CRAN release: 2024-03-18

- Expose the new `description` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
  in `targets` 1.5.1.9001.
- [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md)
  and other static branching target factories now append values to the
  target descriptions. Use the `descriptions` argument of those
  functions to customize.
- Ensure consistent `repository` settings in
  [`tar_change()`](https://docs.ropensci.org/tarchetypes/reference/tar_change.md)
  and
  [`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md).
- [`tar_knit()`](https://docs.ropensci.org/tarchetypes/reference/tar_knit.md),
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md),
  [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md),
  and their “rep” and “raw” versions all gain a `working_directory`
  argument to change the working directory the report knits from. Users
  who set `working_directory` need to supply the `store` argument of
  [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.html)
  and
  [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.html)
  relative to the working directory so the report knows where to find
  the data
  ([\#169](https://github.com/ropensci/tarchetypes/issues/169)).
- [`tar_knit()`](https://docs.ropensci.org/tarchetypes/reference/tar_knit.md),
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md),
  [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md),
  and their “raw” versions all gain an `output_file` argument to more
  conveniently set the file path to the rendered output file.
- [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  and its “rep” and “raw” versions all gain a new `quarto_args` argument
  for advanced Quarto arguments
  ([\#166](https://github.com/ropensci/tarchetypes/issues/166),
  [@petrbouchal](https://github.com/petrbouchal)).

## tarchetypes 0.7.12

CRAN release: 2024-02-06

- Adjust tests because group iteration is now explicitly prohibited for
  dynamic targets.

## tarchetypes 0.7.11

CRAN release: 2024-01-09

- Use
  [`tar_seed_create()`](https://docs.ropensci.org/targets/reference/tar_seed_create.html)
  and
  [`tar_seed_set()`](https://docs.ropensci.org/targets/reference/tar_seed_set.html)
  from `targets`.
- Document limitations of literate programming target factories like
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)
  ([\#158](https://github.com/ropensci/tarchetypes/issues/158)).
- Make
  [`tar_rep2()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep2.md)
  compatible with
  [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md)
  ([\#165](https://github.com/ropensci/tarchetypes/issues/165)).

## tarchetypes 0.7.10

CRAN release: 2023-12-04

- Prepare to use
  [`tar_seed_create()`](https://docs.ropensci.org/targets/reference/tar_seed_create.html)
  and
  [`tar_seed_set()`](https://docs.ropensci.org/targets/reference/tar_seed_set.html)
  (<https://github.com/ropensci/targets/issues/1139>). Future versions
  of `tarchetypes` should use these package functions, but this version
  cannot because of the compatibility constraints of the release cycle.
- Migrate tests to `targets` \>= 1.3.2.9004 progress statuses
  (“completed” instead of “built”, “dispatched” instead of “started”).

## tarchetypes 0.7.9

CRAN release: 2023-10-04

- Deprecate the `packages` and `library` arguments of
  [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  and
  [`tar_quarto_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  ([\#155](https://github.com/ropensci/tarchetypes/issues/155),
  [@svraka](https://github.com/svraka)).
- Switch to from `furrr` to `parallel` for `rep_workers` in
  [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)
  etc. ([\#251](https://github.com/ropensci/tarchetypes/issues/251),
  [@solmos](https://github.com/solmos)).

## tarchetypes 0.7.8

CRAN release: 2023-09-01

- Relax overly strict assertion on R Markdown / Quarto parameter lists
  ([@rmgpanw](https://github.com/rmgpanw),
  [\#152](https://github.com/ropensci/tarchetypes/issues/152)).
- Adjust a test to comply with upcoming `targets` 1.3.0.

## tarchetypes 0.7.7

CRAN release: 2023-06-15

- Allow `format = "file_fast"` in target factories.

## tarchetypes 0.7.6

CRAN release: 2023-05-02

- Support Quarto profiles through the `QUARTO_PROFILE` environment
  variable ([\#139](https://github.com/ropensci/tarchetypes/issues/139),
  [@andrewheiss](https://github.com/andrewheiss)).
- Take the basename of the source file for
  [\#129](https://github.com/ropensci/tarchetypes/issues/129) so the
  output files land correctly when the source file is in a subdirectory
  ([\#129](https://github.com/ropensci/tarchetypes/issues/129),
  `targets` issue 1047, [@joelnitta](https://github.com/joelnitta)).
- Use `targets::tar_runtime_object()$store` instead of
  `targets::tar_runtime_object()$get_store()` to ensure forward
  compatibility with `targets`.
- Use interactive test for
  [`tar_download()`](https://docs.ropensci.org/tarchetypes/reference/tar_download.md)
  to avoid unpredictable network issues outside our control.

## tarchetypes 0.7.5

CRAN release: 2023-03-07

- Implement a new `set_deps` argument in the hook functions to force
  modified targets to keep the dependencies they had before applying the
  hook ([\#131](https://github.com/ropensci/tarchetypes/issues/131),
  [@edalfon](https://github.com/edalfon)).
- Forward all settings to `tar_copy_target()`
  ([\#131](https://github.com/ropensci/tarchetypes/issues/131),
  [@edalfon](https://github.com/edalfon)).
- Initialize the directory of output files in
  [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  and
  [`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md)
  ([\#129](https://github.com/ropensci/tarchetypes/issues/129),
  [@benzipperer](https://github.com/benzipperer)).
- Work around <https://github.com/quarto-dev/quarto-cli/pull/2456> by
  writing temporary local files in
  [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  ([\#129](https://github.com/ropensci/tarchetypes/issues/129),
  [@benzipperer](https://github.com/benzipperer)).

## tarchetypes 0.7.4

CRAN release: 2023-01-06

- Implement `rep_workers` to control inner parallelism in batched
  replication functions
  ([\#117](https://github.com/ropensci/tarchetypes/issues/117)).
- Ensure the function passed to `furrr` functions has environment
  `tar_option_get("envir")`.
- Allow subdirectories of rendered reports with
  [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  ([\#129](https://github.com/ropensci/tarchetypes/issues/129),
  [@mglev1n](https://github.com/mglev1n)).

## tarchetypes 0.7.3

CRAN release: 2022-11-29

- Support nested futures for parallelism among reps within batches
  ([\#117](https://github.com/ropensci/tarchetypes/issues/117),
  [@kkmann](https://github.com/kkmann)).
- Add Quarto troubleshooting section to help files.

## tarchetypes 0.7.2

CRAN release: 2022-10-31

- Migrate away from deprecated
  [`targets::tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.html).
- Implement and return resilient seeds in batched replication
  ([\#111](https://github.com/ropensci/tarchetypes/issues/111),
  [\#113](https://github.com/ropensci/tarchetypes/issues/113)).

## tarchetypes 0.7.1

CRAN release: 2022-09-07

- Document <https://github.com/ropensci/tarchetypes/discussions/105>
  ([@MarekGierlinski](https://github.com/MarekGierlinski)).
- Adapt tests to changes in `tar_manfiest()` default output.

## tarchetypes 0.7.0

CRAN release: 2022-08-05

- Add new functions
  [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  and
  [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  (and “raw” versions) for Quarto documents and projects in pipelines
  ([\#89](https://github.com/ropensci/tarchetypes/issues/89)).
- Add new function
  [`tar_quarto_files()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_files.md)
  to inspect Quarto projects and documents for important files: source
  files to inspect for target dependencies, output documents, and
  Quarto-specific inputs like `_quarto.yml`. Uses
  [`quarto::quarto_inspect()`](https://quarto-dev.github.io/quarto-r/reference/quarto_inspect.html)
  and powers the automatic file detection in
  [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  etc. ([\#89](https://github.com/ropensci/tarchetypes/issues/89)).
- Add runtime guardrails to the `params` argument of
  [`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md)
  (must be a data frame with unique rows (or unique elements of
  `output_file`)).
- Temporarily change `root.dir` when scanning for dependencies so
  `knitr` child documents work
  ([\#93](https://github.com/ropensci/tarchetypes/issues/93),
  [@mutlusun](https://github.com/mutlusun)).
- Use `format = "rds"` for `target_batch` in
  [`tar_map_rep_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md)
  so the global format option does not mess up the pipeline.
- Handle non-atomic length-one list columns in
  [`tar_append_static_values()`](https://docs.ropensci.org/tarchetypes/reference/tar_append_static_values.md).
- Allow
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)
  to work with just one row of parameters
  ([\#96](https://github.com/ropensci/tarchetypes/issues/96),
  [\#97](https://github.com/ropensci/tarchetypes/issues/97),
  [@ugoebel73](https://github.com/ugoebel73)).
- Remove dependencies and collect garbage before running reports.
- Make sure all the target factories have `memory` and
  `garbage_collection` arguments.

## tarchetypes 0.6.0

CRAN release: 2022-04-19

- Implement
  [`tar_file_read()`](https://docs.ropensci.org/tarchetypes/reference/tar_file_read.md)
  ([\#84](https://github.com/ropensci/tarchetypes/issues/84),
  [@petrbouchal](https://github.com/petrbouchal)).
- Suppress warnings for deprecated AWS formats.
- Select the correct targets in
  [`tar_select_targets()`](https://docs.ropensci.org/tarchetypes/reference/tar_select_targets.md)
  ([\#92](https://github.com/ropensci/tarchetypes/issues/92),
  [@arcruz0](https://github.com/arcruz0)).
- Support the `repository` argument for `targets` \>= 0.11.0.

## tarchetypes 0.4.1

CRAN release: 2022-01-07

- Select list elements from `command1` using `[[` and not `[` in
  [`tar_map2()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2.md)
  functions.

## tarchetypes 0.4.0

CRAN release: 2021-12-10

- Implement
  [`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md)
  and
  [`tar_map_rep_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md)
  for dynamic batched replication within static branching for data
  frames ([\#78](https://github.com/ropensci/tarchetypes/issues/78)).
- Implement
  [`tar_map2_count()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_count.md),
  [`tar_map2_count_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_count.md),
  [`tar_map2_size()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_size.md),
  and
  [`tar_map2_size_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_size.md)
  for batched dynamic-within-static branching for data frames
  ([\#78](https://github.com/ropensci/tarchetypes/issues/78)).
- Deprecate
  [`tar_rep_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_map.md)
  in favor of
  [`tar_rep2()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep2.md)
  to avoid name confusion. Likewise with
  [`tar_rep_map_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_map_raw.md)
  to
  [`tar_rep2_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep2.md)
  ([\#78](https://github.com/ropensci/tarchetypes/issues/78)).

## tarchetypes 0.3.2

CRAN release: 2021-10-26

- Allow empty / `NULL` target list in
  [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md)
  ([@kkami1115](https://github.com/kkami1115)).
- Do not claim to support `"aws_file"` format in
  [`tar_files()`](https://docs.ropensci.org/tarchetypes/reference/tar_files.md)
  or related target factories.

## tarchetypes 0.3.1

CRAN release: 2021-09-21

- Relax assertion on language objects.
- Explain `targets` timestamps correctly in the help files of
  [`tar_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_age.md)
  and
  [`tar_cue_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md).

## tarchetypes 0.3.0

CRAN release: 2021-08-04

### Invalidating changes

- When `names = NULL` in
  [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md),
  use hashes instead of numeric indexes for generated target names
  ([\#67](https://github.com/ropensci/tarchetypes/issues/67)). That way,
  target names are no longer sensitive to the order of `values`, and so
  targets will incorrectly invalidate less often. *Unfortunately, this
  is an invalidating change: some targets will automatically rerun after
  you install this version of `tarchetypes`.* I apologize for the
  inconvenience this causes. However, we do need this patch in order to
  solve [\#67](https://github.com/ropensci/tarchetypes/issues/67), and
  targets will incorrectly invalidate less frequently in the future.

### Enhancements

- Migrate to utilities for error handling and metaprogramming exported
  from `targets`
  ([\#59](https://github.com/ropensci/tarchetypes/issues/59)).

## tarchetypes 0.2.1

CRAN release: 2021-06-21

### Bug fixes

- Make the `*_raw()` target factories process `command` the same way
  whether it is an expression or ordinary language object.
- Ensure compatibility with `targets` 0.5.0.9000, which logs skipped
  targets.

### New features

- Add
  [`tar_rep_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_map.md)
  and
  [`tar_rep_map_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_map_raw.md)
  to perform batched computation downstream of
  [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)
  ([\#50](https://github.com/ropensci/tarchetypes/issues/50)).
- Add
  [`tar_select_names()`](https://docs.ropensci.org/tarchetypes/reference/tar_select_names.md)
  and
  [`tar_select_targets()`](https://docs.ropensci.org/tarchetypes/reference/tar_select_targets.md)
  to make certain metaprogramming tasks easier.
- In
  [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md),
  attempt to convert the elements of `values` into lists of language
  objects.

## tarchetypes 0.2.0

CRAN release: 2021-05-11

- Allow trailing commas in
  [`tar_plan()`](https://docs.ropensci.org/tarchetypes/reference/tar_plan.md)
  ([\#40](https://github.com/ropensci/tarchetypes/issues/40),
  [@kendonB](https://github.com/kendonB)).
- Implement
  [`tar_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_age.md)
  based on
  [`tar_cue_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md)
  ([\#39](https://github.com/ropensci/tarchetypes/issues/39),
  [@petrbouchal](https://github.com/petrbouchal)).
- Implement new cue factories
  [`tar_cue_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md),
  [`tar_cue_age_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md),
  [`tar_cue_force()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_force.md),
  and
  [`tar_cue_skip()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_skip.md)
  ([\#39](https://github.com/ropensci/tarchetypes/issues/39)).
- Implement
  [`tar_download()`](https://docs.ropensci.org/tarchetypes/reference/tar_download.md)
  ([\#38](https://github.com/ropensci/tarchetypes/issues/38),
  [@noamross](https://github.com/noamross),
  [@petrbouchal](https://github.com/petrbouchal))
- Set intermediate temporary directory to remove race condition in
  [`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md)
  ([\#36](https://github.com/ropensci/tarchetypes/issues/36),
  [@gorgitko](https://github.com/gorgitko)).
- Prefix internal condition classes with “tar\_”.
- Add new format helpers such as
  [`tar_aws_rds()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats_superseded.md)
  and
  [`tar_parquet()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md).
- Support hooks
  [`tar_hook_before()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_before.md),
  [`tar_hook_inner()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_inner.md),
  and
  [`tar_hook_outer()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_outer.md)
  ([\#44](https://github.com/ropensci/tarchetypes/issues/44)).
- Deep-copy the cue in
  [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md).

## tarchetypes 0.1.1

CRAN release: 2021-03-28

- Unset `crayon.enabled` for literate programming.
- Switch meaning of `%||%` and `%|||%` to conform to historical
  precedent.

## tarchetypes 0.1.0

CRAN release: 2021-02-27

- Add new functions for easier grouping of data frames for dynamic
  branching:
  [`tar_group_by()`](https://docs.ropensci.org/tarchetypes/reference/tar_group_by.md),
  [`tar_group_select()`](https://docs.ropensci.org/tarchetypes/reference/tar_group_select.md),
  [`tar_group_size()`](https://docs.ropensci.org/tarchetypes/reference/tar_group_size.md),
  [`tar_group_count()`](https://docs.ropensci.org/tarchetypes/reference/tar_group_count.md)
  ([\#32](https://github.com/ropensci/tarchetypes/issues/32),
  [@liutiming](https://github.com/liutiming)).
- In
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)
  and related functions, track the `*_files/` output directory if it
  exists ([\#30](https://github.com/ropensci/tarchetypes/issues/30)).
- Implement an external
  [`walk_ast()`](https://docs.ropensci.org/tarchetypes/reference/walk_ast.md)
  function to make it easier for other developers to extend the static
  code analysis of `tarchetypes`
  ([@MilesMcBain](https://github.com/MilesMcBain)).

## tarchetypes 0.0.4

CRAN release: 2021-02-02

- Skip literate programming tests if pandoc is missing or has an
  insufficient version.
- Use explicit temp files in examples even when running inside
  [`targets::tar_dir()`](https://docs.ropensci.org/targets/reference/tar_dir.html).
  ([`targets::tar_dir()`](https://docs.ropensci.org/targets/reference/tar_dir.html)
  and
  [`targets::tar_test()`](https://docs.ropensci.org/targets/reference/tar_test.html)
  already run code in a temporary directory.)
- Add comments in the examples to emphasize that
  [`targets::tar_dir()`](https://docs.ropensci.org/targets/reference/tar_dir.html)
  runs code in a temporary directory, which means all ostensibly files
  created in the enclosed expression will actually be written to
  temporary storage and not the user’s file space.

## tarchetypes 0.0.2

CRAN release: 2021-02-01

- Make sure every function with a help file in `man/` has Rd-tags
  `\value` and `\arguments`.
- For every function with a help file in `man/`, describe the return
  value in the `\value` Rd tag. For each function that returns a target
  definition object or list of target definition objects, the `\value`
  tag now links to <https://books.ropensci.org/targets/>, the user
  manual where the purpose of target definition objects is explained,
  and <https://books.ropensci.org/targets-design/>, the design
  specification which documents the structure and composition of target
  definition objects.
- Ensure that examples, vignettes, and test do not write to the home
  file space of the user.
- Ensure that no function defined in the `tarchetypes` package writes by
  default to the home file space of the user. The paths of all output
  files are controlled by non-`tarchetypes` functions that invoke
  `tarchetypes`.

## tarchetypes 0.0.1

- [`tar_plan()`](https://docs.ropensci.org/tarchetypes/reference/tar_plan.md)
  now returns a list of target definition objects rather than a pipeline
  object. Related: <https://github.com/ropensci/targets/issues/253>.

## tarchetypes 0.0.0.9000

- First version.
- Implement
  [`tar_knitr_deps()`](https://docs.ropensci.org/tarchetypes/reference/tar_knitr_deps.md)
  and
  [`tar_knitr_deps_expr()`](https://docs.ropensci.org/tarchetypes/reference/tar_knitr_deps_expr.md)
  to accommodate custom multi-file literate programming projects like R
  Markdown sites and `bookdown` projects
  ([\#23](https://github.com/ropensci/tarchetypes/issues/23),
  [@tjmahr](https://github.com/tjmahr)).
