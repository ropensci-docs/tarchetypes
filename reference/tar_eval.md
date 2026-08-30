# Evaluate multiple expressions created with symbol substitution.

Loop over a grid of values, create an expression object from each one,
and then evaluate that expression. Helps with general metaprogramming.

`tar_eval()` expects an unevaluated expression for the `expr` object,
whereas `tar_eval_raw()` expects an evaluated expression object.

## Usage

``` r
tar_eval(expr, values, envir = parent.frame())

tar_eval_raw(expr, values, envir = parent.frame())
```

## Arguments

- expr:

  Starting expression. Values are iteratively substituted in place of
  symbols in `expr` to create each new expression, and then each new
  expression is evaluated.

  `tar_eval()` expects an unevaluated expression for the `expr` object,
  whereas `tar_eval_raw()` expects an evaluated expression object.

- values:

  List of values to substitute into `expr` to create the expressions.
  All elements of `values` must have the same length.

- envir:

  Environment in which to evaluate the new expressions.

## Value

A list of return values from the generated expression objects. Often,
these values are target definition objects. See the "Target definition
objects" section for background on target definition objects
specifically.

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
[`tar_sub()`](https://docs.ropensci.org/tarchetypes/reference/tar_sub.md)

## Examples

``` r
# tar_map() is incompatible with tar_render() because the latter
# operates on preexisting tar_target() objects. By contrast,
# tar_eval() and tar_sub() iterate over the literal code
# farther upstream.
values <- list(
  name = lapply(c("name1", "name2"), as.symbol),
  file = list("file1.Rmd", "file2.Rmd")
)
tar_sub(list(name, file), values = values)
#> [[1]]
#> expression(list(name1, "file1.Rmd"))
#> 
#> [[2]]
#> expression(list(name2, "file2.Rmd"))
#> 
tar_sub(tar_render(name, file), values = values)
#> [[1]]
#> expression(tar_render(name1, "file1.Rmd"))
#> 
#> [[2]]
#> expression(tar_render(name2, "file2.Rmd"))
#> 
path <- tempfile()
file.create(path)
#> [1] TRUE
str(tar_eval(tar_render(name, path), values = values))
#> List of 2
#>  $ :Classes 'tar_stem', 'tar_builder', 'tar_target', 'environment' <environment: 0x555b66c95fd0> 
#>  $ :Classes 'tar_stem', 'tar_builder', 'tar_target', 'environment' <environment: 0x555b66b3d010> 
str(tar_eval_raw(quote(tar_render(name, path)), values = values))
#> List of 2
#>  $ :Classes 'tar_stem', 'tar_builder', 'tar_target', 'environment' <environment: 0x555b667e5678> 
#>  $ :Classes 'tar_stem', 'tar_builder', 'tar_target', 'environment' <environment: 0x555b6663e750> 
# So in your _targets.R file, you can define a pipeline like as below.
# Just make sure to set a unique name for each target
# (which tar_map() does automatically).
values <- list(
  name = lapply(c("name1", "name2"), as.symbol),
  file = c(path, path)
)
list(
  tar_eval(tar_render(name, file), values = values)
)
#> [[1]]
#> [[1]][[1]]
#> <tar_stem> 
#>   name: name1 
#>   description:  
#>   command:
#>     tarchetypes::tar_render_run(path = "/tmp/Rtmp1fNMOq/file5b9a350f74", 
#>         args = list(input = "/tmp/Rtmp1fNMOq/file5b9a350f74", knit_root_dir = getwd(), 
#>             quiet = TRUE), deps = list()) 
#>   format: file 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     list() 
#>   cue:
#>     seed: TRUE
#>     file: TRUE
#>     iteration: TRUE
#>     repository: TRUE
#>     format: TRUE
#>     depend: TRUE
#>     command: TRUE
#>     mode: thorough 
#>   packages:
#>     tarchetypes
#>     stats
#>     graphics
#>     grDevices
#>     utils
#>     datasets
#>     methods
#>     base 
#>   library:
#>     NULL
#> [[1]][[2]]
#> <tar_stem> 
#>   name: name2 
#>   description:  
#>   command:
#>     tarchetypes::tar_render_run(path = "/tmp/Rtmp1fNMOq/file5b9a350f74", 
#>         args = list(input = "/tmp/Rtmp1fNMOq/file5b9a350f74", knit_root_dir = getwd(), 
#>             quiet = TRUE), deps = list()) 
#>   format: file 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     list() 
#>   cue:
#>     seed: TRUE
#>     file: TRUE
#>     iteration: TRUE
#>     repository: TRUE
#>     format: TRUE
#>     depend: TRUE
#>     command: TRUE
#>     mode: thorough 
#>   packages:
#>     tarchetypes
#>     stats
#>     graphics
#>     grDevices
#>     utils
#>     datasets
#>     methods
#>     base 
#>   library:
#>     NULL
#> 
```
