# Append the `tar_group` variable to a `tar_map2()` target.

Append the `tar_group` variable to a
[`tar_map2()`](https://docs.ropensci.org/tarchetypes/reference/tar_map2.md)
target.

## Usage

``` r
tar_map2_group(data, group)
```

## Arguments

- data:

  Data frame to be returned from the target.

- group:

  Function on the data to return the `tar_group` column. If `group` is
  `NULL`, then no `tar_group` column is attached.

## Value

A data frame with a `tar_group` column attached (if `group` is not
`NULL`).

## Details

For internal use only. Users should not invoke this function directly.
