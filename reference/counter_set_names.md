# Add data to an existing counter object.

Not a user-side function. Do not invoke directly.

## Usage

``` r
counter_set_names(counter, names)
```

## Arguments

- counter:

  A counter object, defined for internal purposes only.

- names:

  Character vector of names to add to the counter.

## Value

`NULL` (invisibly)

## Examples

``` r
counter <- counter_init()
counter_set_names(counter, letters)
```
