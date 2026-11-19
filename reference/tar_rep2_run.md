# Run [`tar_rep2()`](https://docs.ropensci.org/tarchetypes/reference/tar_rep2.md) batches.

Not a user-side function. Do not invoke directly.

## Usage

``` r
tar_rep2_run(command, batches, iteration, rep_workers)
```

## Arguments

- command:

  R expression, the command to run on each rep.

- batches:

  Named list of batch data to map over.

- iteration:

  Iteration method: `"list"`, `"vector"`, or `"group"`.

- rep_workers:

  Positive integer of length 1, number of local R processes to use to
  run reps within batches in parallel. If 1, then reps are run
  sequentially within each batch. If greater than 1, then reps within
  batch are run in parallel using a PSOCK cluster.

## Value

The result of batched replication.
