# Prepare Quarto parameters for `tar_quarto_rep()`.

Internal function needed for
[`tar_quarto_rep()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto_rep.md).
Users should not invoke it directly.

## Usage

``` r
tar_quarto_rep_run_params(execute_params, batches, default_output_file)
```

## Arguments

- execute_params:

  Data frame of Quarto parameters.

- batches:

  Number of batches to split up the renderings.

- default_output_file:

  Default output file path deduced from the YAML front-matter of the
  Quarto source document.

## Value

A batched data frame of Quarto parameters.

## Examples

``` r
execute_params <- tibble::tibble(param1 = letters[seq_len(4)])
tar_quarto_rep_run_params(execute_params, 1, "report.html")
#> # A tibble: 4 × 3
#>   param1 output_file                  tar_group
#>   <chr>  <chr>                            <int>
#> 1 a      report_2a5405fc13868c96.html         1
#> 2 b      report_21139b961dcbce03.html         1
#> 3 c      report_3917a4143d2cbeeb.html         1
#> 4 d      report_a2b27e0a0fd35ba6.html         1
tar_quarto_rep_run_params(execute_params, 2, "report.html")
#> # A tibble: 4 × 3
#>   param1 output_file                  tar_group
#>   <chr>  <chr>                            <int>
#> 1 a      report_2a5405fc13868c96.html         1
#> 2 b      report_21139b961dcbce03.html         1
#> 3 c      report_3917a4143d2cbeeb.html         2
#> 4 d      report_a2b27e0a0fd35ba6.html         2
tar_quarto_rep_run_params(execute_params, 3, "report.html")
#> # A tibble: 4 × 3
#>   param1 output_file                  tar_group
#>   <chr>  <chr>                            <int>
#> 1 a      report_2a5405fc13868c96.html         1
#> 2 b      report_21139b961dcbce03.html         1
#> 3 c      report_3917a4143d2cbeeb.html         2
#> 4 d      report_a2b27e0a0fd35ba6.html         3
tar_quarto_rep_run_params(execute_params, 4, "report.html")
#> # A tibble: 4 × 3
#>   param1 output_file                  tar_group
#>   <chr>  <chr>                            <int>
#> 1 a      report_2a5405fc13868c96.html         1
#> 2 b      report_21139b961dcbce03.html         2
#> 3 c      report_3917a4143d2cbeeb.html         3
#> 4 d      report_a2b27e0a0fd35ba6.html         4
```
