# Hook to prepend code

Prepend R code to the commands of multiple targets. `tar_hook_before()`
expects unevaluated expressions for the `hook` and `names` arguments,
whereas `tar_hook_before_raw()` expects evaluated expression objects.

## Usage

``` r
tar_hook_before(
  targets,
  hook,
  names = NULL,
  set_deps = TRUE,
  envir = parent.frame()
)

tar_hook_before_raw(
  targets,
  hook,
  names = NULL,
  set_deps = TRUE,
  envir = parent.frame()
)
```

## Arguments

- targets:

  A list of target definition objects. The input target list can be
  arbitrarily nested, but it must consist entirely of target objects. In
  addition, the return value is a simple list where each element is a
  target definition object. All hook functions remove the nested
  structure of the input target list.

- hook:

  R code to insert. `tar_hook_before()` expects unevaluated expressions
  for the `hook` and `names` arguments, whereas `tar_hook_before_raw()`
  expects evaluated expression objects.

- names:

  Name of targets in the target list to apply the hook. Supplied using
  `tidyselect` helpers like
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html),
  as in `names = starts_with("your_prefix_")`. Set to `NULL` to include
  all targets supplied to the `targets` argument. Targets not included
  in `names` still remain in the target list, but they are not modified
  because the hook does not apply to them.

  The regular hook functions expects unevaluated expressions for the
  `hook` and `names` arguments, whereas the `"_raw"` versions expect
  evaluated expression objects.

- set_deps:

  Logical of length 1, whether to refresh the dependencies of each
  modified target by scanning the newly generated target commands for
  dependencies. If `FALSE`, then the target will keep the original set
  of dependencies it had before the hook. Set to `NULL` to include all
  targets supplied to the `targets` argument. `TRUE` is recommended for
  nearly all situations. Only use `FALSE` if you have a specialized use
  case and you know what you are doing.

- envir:

  Optional environment to construct the quosure for the `names` argument
  to select names.

## Value

A flattened list of target definition objects with the hooks applied.
Even if the input target list had a nested structure, the return value
is a simple list where each element is a target definition object. All
hook functions remove the nested structure of the input target list.

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

Other hooks:
[`tar_hook_inner()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_inner.md),
[`tar_hook_outer()`](https://docs.ropensci.org/tarchetypes/reference/tar_hook_outer.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
targets::tar_dir({ # tar_dir() runs code from a temporary directory.
targets::tar_script({
  targets <- list(
    # Nested target lists work with hooks.
    list(
      targets::tar_target(x1, task1()),
      targets::tar_target(x2, task2(x1))
    ),
    targets::tar_target(x3, task3(x2)),
    targets::tar_target(y1, task4(x3))
  )
  tarchetypes::tar_hook_before(
    targets = targets,
    hook = print("Running hook."),
    names = starts_with("x")
  )
})
targets::tar_manifest(fields = command)
})
# With tar_hook_before_raw():
targets::tar_script({
  targets <- list(
    # Nested target lists work with hooks.
    list(
      targets::tar_target(x1, task1()),
      targets::tar_target(x2, task2(x1))
    ),
    targets::tar_target(x3, task3(x2)),
    targets::tar_target(y1, task4(x3))
  )
  tarchetypes::tar_hook_before_raw(
    targets = targets,
    hook = quote(print("Running hook.")),
    names = quote(starts_with("x"))
  )
})
}
```
