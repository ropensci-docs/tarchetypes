# Package index

## Help

- [`tarchetypes-package`](https://docs.ropensci.org/tarchetypes/reference/tarchetypes-package.md)
  : targets: Archetypes for Targets

## Literate programming targets

- [`tar_knit()`](https://docs.ropensci.org/tarchetypes/reference/tar_knit.md)
  [`tar_knit_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_knit.md)
  :

  Target with a `knitr` document.

- [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  [`tar_quarto_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.md)
  : Target with a Quarto project.

- [`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  [`tar_quarto_rep_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md)
  : Parameterized Quarto with dynamic branching.

- [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)
  [`tar_render_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.md)
  : Target with an R Markdown document.

- [`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md)
  [`tar_render_rep_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md)
  : Parameterized R Markdown with dynamic branching.

## Literate programming utilities

- [`tar_knitr_deps()`](https://docs.ropensci.org/tarchetypes/reference/tar_knitr_deps.md)
  : List literate programming dependencies.
- [`tar_knitr_deps_expr()`](https://docs.ropensci.org/tarchetypes/reference/tar_knitr_deps_expr.md)
  : Expression with literate programming dependencies.
- [`tar_quarto_files()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_files.md)
  : Quarto file detection

## Target factories for storage formats

- [`tar_url()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_file()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_file_fast()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_rds()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_qs()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_keras()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_torch()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_arrow_feather()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_parquet()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_fst()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_fst_dt()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_fst_tbl()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  [`tar_nanoparquet()`](https://docs.ropensci.org/tarchetypes/reference/tar_formats.md)
  : Target factories for storage formats

## Storage formats

- [`tar_format_nanoparquet()`](https://docs.ropensci.org/tarchetypes/reference/tar_format_nanoparquet.md)
  : Nanoparquet format

## Simple files

- [`tar_file_read()`](https://docs.ropensci.org/tarchetypes/reference/tar_file_read.md)
  : Track a file and read the contents.

## Static branching

- [`tar_combine()`](https://docs.ropensci.org/tarchetypes/reference/tar_combine.md)
  [`tar_combine_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_combine.md)
  : Static aggregation
- [`tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.md)
  : Static branching.

## Dynamic branching over files

- [`tar_files()`](https://docs.ropensci.org/tarchetypes/reference/tar_files.md)
  [`tar_files_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_files.md)
  : Dynamic branching over output or input files.
- [`tar_files_input()`](https://docs.ropensci.org/tarchetypes/reference/tar_files_input.md)
  [`tar_files_input_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_files_input.md)
  : Dynamic branching over input files or URLs

## Dynamic grouped data frames

- [`tar_group_by()`](https://docs.ropensci.org/tarchetypes/reference/tar_group_by.md)
  : Group a data frame target by one or more variables.

- [`tar_group_count()`](https://docs.ropensci.org/tarchetypes/reference/tar_group_count.md)
  : Group the rows of a data frame into a given number groups

- [`tar_group_select()`](https://docs.ropensci.org/tarchetypes/reference/tar_group_select.md)
  :

  Group a data frame target with `tidyselect` semantics.

- [`tar_group_size()`](https://docs.ropensci.org/tarchetypes/reference/tar_group_size.md)
  : Group the rows of a data frame into groups of a given size.

## Dynamic batched replication

- [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)
  [`tar_rep_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)
  : Batched replication with dynamic branching.

- [`tar_rep2()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep2.md)
  [`tar_rep2_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep2.md)
  :

  Dynamic batched computation downstream of
  [`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md)

## Dynamic batched replication within static branches for data frames

- [`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md)
  [`tar_map_rep_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md)
  : Dynamic batched replication within static branches for data frames.

## Batched dynamic-within-static branching for data frames

- [`tar_map2_count()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_count.md)
  [`tar_map2_count_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_count.md)
  : Dynamic-within-static branching for data frames (count batching).
- [`tar_map2_size()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_size.md)
  [`tar_map2_size_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_size.md)
  : Dynamic-within-static branching for data frames (size batching).

## Dynamic batched replication indexing

- [`tar_rep_index()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep_index.md)
  : Get overall rep index.

## Target selection

- [`tar_select_names()`](https://docs.ropensci.org/tarchetypes/reference/tar_select_names.md)
  : Select target names from a target list
- [`tar_select_targets()`](https://docs.ropensci.org/tarchetypes/reference/tar_select_targets.md)
  : Select target definition objects from a target list

## Targets with custom invalidation rules

- [`tar_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_age.md)
  : Create a target that runs when the last run gets old
- [`tar_change()`](https://docs.ropensci.org/tarchetypes/reference/tar_change.md)
  : Target that responds to an arbitrary change.
- [`tar_download()`](https://docs.ropensci.org/tarchetypes/reference/tar_download.md)
  : Target that downloads URLs.
- [`tar_force()`](https://docs.ropensci.org/tarchetypes/reference/tar_force.md)
  : Target with a custom condition to force execution.
- [`tar_skip()`](https://docs.ropensci.org/tarchetypes/reference/tar_skip.md)
  : Target with a custom cancellation condition.

## Cues

- [`tar_cue_age()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md)
  [`tar_cue_age_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_age.md)
  : Cue to run a target when the last output reaches a certain age
- [`tar_cue_force()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_force.md)
  : Cue to force a target to run if a condition is true
- [`tar_cue_skip()`](https://docs.ropensci.org/tarchetypes/reference/tar_cue_skip.md)
  : Cue to skip a target if a condition is true

## Hooks

- [`tar_hook_before()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_before.md)
  [`tar_hook_before_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_before.md)
  : Hook to prepend code
- [`tar_hook_inner()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_inner.md)
  [`tar_hook_inner_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_inner.md)
  : Hook to wrap dependencies
- [`tar_hook_outer()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_outer.md)
  [`tar_hook_outer_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_outer.md)
  : Hook to wrap commands

## Metaprogramming utilities

- [`tar_eval()`](https://docs.ropensci.org/tarchetypes/reference/tar_eval.md)
  [`tar_eval_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_eval.md)
  : Evaluate multiple expressions created with symbol substitution.
- [`tar_sub()`](https://docs.ropensci.org/tarchetypes/reference/tar_sub.md)
  [`tar_sub_raw()`](https://docs.ropensci.org/tarchetypes/reference/tar_sub.md)
  : Create multiple expressions with symbol substitution.

## Domain-specific languages for pipeline construction

- [`tar_assign()`](https://docs.ropensci.org/tarchetypes/reference/tar_assign.md)
  : An assignment-based pipeline DSL

- [`tar_plan()`](https://docs.ropensci.org/tarchetypes/reference/tar_plan.md)
  :

  A `drake`-plan-like pipeline DSL

- [`tar_tangle()`](https://docs.ropensci.org/tarchetypes/reference/tar_tangle.md)
  : Convert Quarto or R Markdown to a pipeline
