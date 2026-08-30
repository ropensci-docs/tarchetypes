# Run a `tar_rep()` batch.

Internal function needed for
[`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md).
Users should not invoke it directly.

## Usage

``` r
tar_rep_run(command, batch, reps, iteration, rep_workers)
```

## Arguments

- command:

  Expression object, command to replicate.

- batch:

  Numeric of length 1, batch index.

- reps:

  Numeric of length 1, number of reps per batch.

- iteration:

  Character, iteration method.

- rep_workers:

  Positive integer of length 1, number of local R processes to use to
  run reps within batches in parallel. If 1, then reps are run
  sequentially within each batch. If greater than 1, then reps within
  batch are run in parallel using a PSOCK cluster.

## Value

Aggregated results of multiple executions of the user-defined command
supplied to
[`tar_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep.md).
Depends on what the user specifies. Common use cases are simulated
datasets.
