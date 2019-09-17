Data Wrangling I
================
Molly Martorella
9/17/2019

## Load in a dataset

``` r
##reads in a dataset

##absolute paths do not always match, use of relative paths gives path from common starting point. Always use relative path, never absolute path (this allows you to send R project folder to someone, and they can do the same analysis).

litters_data <- read_csv(file = "./data_import_examples/FAS_litters.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Group = col_character(),
    ##   `Litter Number` = col_character(),
    ##   `GD0 weight` = col_double(),
    ##   `GD18 weight` = col_double(),
    ##   `GD of Birth` = col_double(),
    ##   `Pups born alive` = col_double(),
    ##   `Pups dead @ birth` = col_double(),
    ##   `Pups survive` = col_double()
    ## )

``` r
#first step shjould be to print or look at head/tail of data.

#reform variable names in dataset if necessary:

litters_data <- janitor::clean_names(litters_data)
```
