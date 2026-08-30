# Run a rep in a `tar_map2()`-powered function.

Not a user-side function. Do not invoke directly.

## Usage

``` r
tar_map2_run_rep(rep, values, command, batch, seeds, columns, envir)
```

## Arguments

- rep:

  Rep number.

- values:

  Data frame of mapped-over values.

- command:

  R command to run.

- batch:

  Batch number.

- seeds:

  Random number generator seeds of the batch.

- columns:

  Expression for appending static columns.

- envir:

  Environment of the target.

## Value

The result of running `expr`.

## Examples

``` r
# See the examples of tar_map2_count().
```
