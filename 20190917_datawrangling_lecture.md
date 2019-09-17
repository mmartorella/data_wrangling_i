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
#first step should be to print or view data.

#reform variable names in dataset if necessary:

litters_data <- janitor::clean_names(litters_data)

#use :: if did not previously load library earlier in code. Often use janitor::clean_names() because not using that library for anything else, and if attach that package can have name conflicts.
```

## Load in pups dataset:

``` r
pups_data <- read_csv("./data_import_examples/FAS_pups.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Litter Number` = col_character(),
    ##   Sex = col_double(),
    ##   `PD ears` = col_double(),
    ##   `PD eyes` = col_double(),
    ##   `PD pivot` = col_double(),
    ##   `PD walk` = col_double()
    ## )

``` r
pups_data <- janitor::clean_names(pups_data)

#gives summary of variable types, histogram of values, etc.
skimr::skim(pups_data)
```

    ## Skim summary statistics
    ##  n obs: 313 
    ##  n variables: 6 
    ## 
    ## ── Variable type:character ────────────────────────────────────────────────────────────
    ##       variable missing complete   n min max empty n_unique
    ##  litter_number       0      313 313   3  15     0       49
    ## 
    ## ── Variable type:numeric ──────────────────────────────────────────────────────────────
    ##  variable missing complete   n  mean   sd p0 p25 p50 p75 p100     hist
    ##   pd_ears      18      295 313  3.68 0.59  2   3   4   4    5 ▁▁▅▁▁▇▁▁
    ##   pd_eyes      13      300 313 12.99 0.62 12  13  13  13   15 ▂▁▇▁▁▂▁▁
    ##  pd_pivot      13      300 313  7.09 1.51  4   6   7   8   12 ▃▆▇▃▂▁▁▁
    ##   pd_walk       0      313 313  9.5  1.34  7   9   9  10   14 ▁▅▇▅▃▂▁▁
    ##       sex       0      313 313  1.5  0.5   1   1   2   2    2 ▇▁▁▁▁▁▁▇

## Read in an excel file:

``` r
mlb_data <- read_excel(path = "./data_import_examples/mlb11.xlsx")

# exporting data:

write_csv(mlb_data, path = "./data_import_examples/mlb_export.csv")

#it will write this file every time you knit!!
```

## Read in SAS:

``` r
pulse_data <- haven::read_sas("./data_import_examples/public_pulse_data.sas7bdat")
```
