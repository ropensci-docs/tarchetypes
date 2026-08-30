# Prepare R Markdown parameters for `tar_render_rep()`.

Internal function needed for
[`tar_render_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_render_rep.md).
Users should not invoke it directly.

## Usage

``` r
tar_render_rep_run_params(params, batches)
```

## Arguments

- params:

  Data frame of R Markdown parameters.

- batches:

  Number of batches to split up the renderings.

## Value

A batched data frame of R Markdown parameters.

## Examples

``` r
params <- tibble::tibble(param1 = letters[seq_len(4)])
tar_render_rep_run_params(params, 1)
#> # A tibble: 4 × 2
#>   param1 tar_group
#>   <chr>      <int>
#> 1 a              1
#> 2 b              1
#> 3 c              1
#> 4 d              1
tar_render_rep_run_params(params, 2)
#> # A tibble: 4 × 2
#>   param1 tar_group
#>   <chr>      <int>
#> 1 a              1
#> 2 b              1
#> 3 c              2
#> 4 d              2
tar_render_rep_run_params(params, 3)
#> # A tibble: 4 × 2
#>   param1 tar_group
#>   <chr>      <int>
#> 1 a              1
#> 2 b              1
#> 3 c              2
#> 4 d              3
tar_render_rep_run_params(params, 4)
#> # A tibble: 4 × 2
#>   param1 tar_group
#>   <chr>      <int>
#> 1 a              1
#> 2 b              2
#> 3 c              3
#> 4 d              4
```
