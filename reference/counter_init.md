# Counter constructor.

Not a user-side function. Do not invoke directly.

## Usage

``` r
counter_init(names = NULL)
```

## Arguments

- names:

  Character vector of names to add to the new counter.

## Value

A new counter object.

## Details

Creates a counter object as described at
<https://books.ropensci.org/targets-design/classes.html#counter-class>.

## Examples

``` r
counter <- counter_init()
counter_set_names(counter, letters)
```
