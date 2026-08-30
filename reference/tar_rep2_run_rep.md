# Run a rep in a `tar_rep2()`-powered function.

Not a user-side function. Do not invoke directly.

## Usage

``` r
tar_rep2_run_rep(rep, slice, command, batch, seeds, envir)
```

## Arguments

- rep:

  Rep number.

- slice:

  Slice of the upstream batch data of the given rep.

- command:

  R command to run.

- batch:

  Batch number.

- seeds:

  Random number generator seeds of the batch.

- envir:

  Environment of the target.

## Value

The result of running `expr`.

## Examples

``` r
# See the examples of tar_rep2().
```
