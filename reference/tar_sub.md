# Create multiple expressions with symbol substitution.

Loop over a grid of values and create an expression object from each
one. Helps with general metaprogramming.

`tar_sub()` expects an unevaluated expression for the `expr` object,
whereas `tar_sub_raw()` expects an evaluated expression object.

## Usage

``` r
tar_sub(expr, values)

tar_sub_raw(expr, values)
```

## Arguments

- expr:

  Starting expression. Values are iteratively substituted in place of
  symbols in `expr` to create each new expression.

  `tar_sub()` expects an unevaluated expression for the `expr` object,
  whereas `tar_sub_raw()` expects an evaluated expression object.

- values:

  List of values to substitute into `expr` to create the expressions.
  All elements of `values` must have the same length.

## Value

A list of expression objects. Often, these expression objects evaluate
to target definition objects (but not necessarily). See the "Target
definition objects" section for background.

## Target definition objects

Most `tarchetypes` functions are target factories, which means they
return target definition objects or lists of target definition objects.
target definition objects represent skippable steps of the analysis
pipeline as described at <https://books.ropensci.org/targets/>. Please
read the walkthrough at
<https://books.ropensci.org/targets/walkthrough.html> to understand the
role of target definition objects in analysis pipelines.

For developers,
<https://wlandau.github.io/targetopia/contributing.html#target-factories>
explains target factories (functions like this one which generate
targets) and the design specification at
<https://books.ropensci.org/targets-design/> details the structure and
composition of target definition objects.

## See also

Other Metaprogramming utilities:
[`tar_eval()`](https://docs.ropensci.org/tarchetypes/reference/tar_eval.md)

## Examples

``` r
# tar_map() is incompatible with tar_render() because the latter
# operates on preexisting tar_target() objects. By contrast,
# tar_eval() and tar_sub() iterate over code farther upstream.
values <- list(
  name = lapply(c("name1", "name2"), as.symbol),
  file = list("file1.Rmd", "file2.Rmd")
)
tar_sub(tar_render(name, file), values = values)
#> [[1]]
#> expression(tar_render(name1, "file1.Rmd"))
#> 
#> [[2]]
#> expression(tar_render(name2, "file2.Rmd"))
#> 
tar_sub_raw(quote(tar_render(name, file)), values = values)
#> [[1]]
#> expression(tar_render(name1, "file1.Rmd"))
#> 
#> [[2]]
#> expression(tar_render(name2, "file2.Rmd"))
#> 
```
