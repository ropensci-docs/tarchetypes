# Download multiple URLs and return the local paths.

Not a user-side function. Do not invoke directly.

## Usage

``` r
tar_download_run(urls, paths, method, quiet, mode, cacheOK, extra, headers)
```

## Arguments

- urls:

  Character vector of URLs to track and download. Must be known and
  declared before the pipeline runs.

- paths:

  Character vector of local file paths to download each of the URLs.
  Must be known and declared before the pipeline runs.

- method:

  Method to be used for downloading files. Current download methods are
  `"internal"`, `"libcurl"`, `"wget"`, `"curl"` and `"wininet"` (Windows
  only), and there is a value `"auto"`: see ‘Details’ and ‘Note’.

  The method can also be set through the option
  `"download.file.method"`: see
  [`options()`](https://rdrr.io/r/base/options.html).

- quiet:

  If `TRUE`, suppress status messages (if any), and the progress bar.

- mode:

  character. The mode with which to write the file. Useful values are
  `"w"`, `"wb"` (binary), `"a"` (append) and `"ab"`. Not used for
  methods `"wget"` and `"curl"`. See also ‘Details’, notably about using
  `"wb"` for Windows.

- cacheOK:

  logical. Is a server-side cached value acceptable?

- extra:

  character vector of additional command-line arguments for the `"wget"`
  and `"curl"` methods.

- headers:

  named character vector of additional HTTP headers to use in HTTP\[S\]
  requests. It is ignored for non-HTTP\[S\] URLs. The `User-Agent`
  header taken from the `HTTPUserAgent` option (see
  [`options`](https://rdrr.io/r/base/options.html)) is automatically
  used as the first header.

## Value

A character vector of file paths where the URLs were downloaded.

## Examples

``` r
if (identical(Sys.getenv("TAR_LONG_EXAMPLES"), "true")) {
  tarchetypes::tar_download_run(
    urls = "https://httpbin.org/etag/test",
    paths = tempfile(),
    method = NULL,
    quiet = TRUE,
    mode = "w",
    cacheOK = NULL,
    extra = NULL,
    headers = NULL
  )
}
```
