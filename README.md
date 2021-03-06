
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Sentry

<!-- badges: start -->
<!-- badges: end -->

The goal of Sentry is to aid in extracting, transforming, and loading
(ETL) laboratory data provided to the bureau of water assessment and
management (BWAM).

## Installation

You can install the development version of Sentry from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("BWAM/sentry")
```

## Example

Load the R package.

``` r
library(Sentry)
```

Define the output directory. Here I am just creating a temporary
directory to store the output.

``` r
(output_dir <- tempdir())
#> [1] "C:\\Users\\zmsmith.000\\AppData\\Local\\Temp\\RtmpaemxkD"
```

## Single Zip File

Extract and transform ALS Rochester data.

``` r
dat <- read_als(zip_path = here::here("inst",
                                      "example_zips",
                                      "R2004299.zip"))
```

Export the ALS data as a JSON object.

``` r
export_json(
  x = dat,
  path = output_dir,
  filename = "r2004299"
)
```

## Multiple Zip Files

Create a vector that contains the filepaths of all of the data files of
interest.

``` r
file_vec <- list.files(
  path = here::here("inst",
                    "example_zips"),
  pattern = "zip",
  full.names = TRUE,
  include.dirs = TRUE
)
```

Name the vector elements based on the base file name.

``` r
names(file_vec) <- gsub(pattern = "\\.zip",
                        replacement = "",
                        x = basename(file_vec))
```

This for loop iterates over the vector of files created above
(`file_vec`) importing, transforming, and exporting data as a JSON
object.

``` r
for (i in seq_along(file_vec)) {
   dat <- read_als(zip_path = file_vec[i])
  
  export_json(x = dat,
              path = output_dir,
              filename = names(file_vec[i]))
}
```
