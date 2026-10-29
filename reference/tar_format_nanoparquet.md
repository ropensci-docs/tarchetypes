# Nanoparquet format

Nanoparquet storage format for data frames. Uses
[`nanoparquet::read_parquet()`](https://nanoparquet.r-lib.org/reference/read_parquet.html)
and
[`nanoparquet::write_parquet()`](https://nanoparquet.r-lib.org/reference/write_parquet.html)
to read and write data frames returned by targets in a pipeline. Note:
attributes such as `dplyr` row groupings and `posterior` draws info are
dropped during the writing process.

## Usage

``` r
tar_format_nanoparquet(compression = "snappy", class = "tbl")
```

## Arguments

- compression:

  Character string, compression type for saving the data. See the
  `compression` argument of
  [`nanoparquet::write_parquet()`](https://nanoparquet.r-lib.org/reference/write_parquet.html)
  for details.

- class:

  Character vector with the data frame subclasses to assign. See the
  `class` argument of
  [`nanoparquet::parquet_options()`](https://nanoparquet.r-lib.org/reference/parquet_options.html)
  for details.

## Value

A
[`targets::tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.html)
storage format specification string that can be directly supplied to the
`format` argument of
[`targets::tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.html)
or
[`targets::tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.html).

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
targets::tar_script({
  library(targets)
  libary(tarchetypes)
  list(
    tar_target(
      name = data,
      command = data.frame(x = 1),
      format = tar_format_nanoparquet()
    )
  )
})
tar_make()
tar_read(data)
})
}
```
