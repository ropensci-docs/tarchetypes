# Convert a condition into a change.

Supports
[`tar_force()`](https://docs.ropensci.org/tarchetypes/reference/tar_force.md).
This is really an internal function and not meant to be called by users
directly.

## Usage

``` r
tar_force_change(condition)
```

## Arguments

- condition:

  Logical, whether to run the downstream target in
  [`tar_force()`](https://docs.ropensci.org/tarchetypes/reference/tar_force.md).

## Value

A hash that changes when the downstream target is supposed to run.
