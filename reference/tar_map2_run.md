# Run a dynamic batch of a `tar_map2()` target.

Run a dynamic batch of a
[`tar_map2()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2.md)
target.

## Usage

``` r
tar_map2_run(command, values, columns, rep_workers)
```

## Arguments

- command:

  Command to run.

- values:

  Data frame of named arguments produced by `command1` that `command2`
  dynamically maps over. Different from the `values` argument of
  [`tar_map2()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2.md).

- columns:

  tidyselect expression to select columns of `values` to append to the
  result.

- rep_workers:

  Positive integer of length 1, number of local R processes to use to
  run reps within batches in parallel. If 1, then reps are run
  sequentially within each batch. If greater than 1, then reps within
  batch are run in parallel using a PSOCK cluster.

## Value

A data frame with a `tar_group` column attached (if `group` is not
`NULL`).

## Details

For internal use only. Users should not invoke this function directly.
