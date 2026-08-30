# Get overall rep index.

Get the integer index of the current replication in certain target
factories.

## Usage

``` r
tar_rep_index()
```

## Value

Positive integer from 1 to `batches * reps`, index of the current
replication in an ongoing pipeline.

## Details

`tar_rep_index()` cannot run in your interactive R session or even the
setup portion of `_targets.R`. It must be part of the R command of a
target actively running in a pipeline.

In addition, `tar_rep_index()` is only compatible with
[`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md),
[`tar_rep2()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep2.md),
[`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md),
[`tar_map2_count()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_count.md),
and
[`tar_map2_size()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2_size.md).
In the latter 3 cases, `tar_rep_index()` cannot be part of the `values`
or `command1` arguments.

In
[`tar_map_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_map_rep.md),
each row of the `values` argument (each "scenario") gets its own
independent set of index values from 1 to `batches * reps`.

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
targets::tar_script({
  tar_map_rep(
    x,
    data.frame(index = tar_rep_index()),
    batches = 2L,
    reps = 3L,
    values = list(value = c("a", "b"))
  )
})
targets::tar_make()
x <- targets::tar_read(x)
all(x$index == x$tar_rep + (3L * (x$tar_batch - 1L)))
#> TRUE
})
}
```
