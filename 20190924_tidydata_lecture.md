Tidy Data
================
Molly Martorella
9/24/2019

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ───────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
options(tibble.print_min = 5)
```

``` r
pulse_data <- 
  haven::read_sas("./data_import_examples/public_pulse_data.sas7bdat") %>%
  janitor::clean_names()
  

pulse_data
```

    ## # A tibble: 1,087 x 7
    ##      id   age sex   bdi_score_bl bdi_score_01m bdi_score_06m bdi_score_12m
    ##   <dbl> <dbl> <chr>        <dbl>         <dbl>         <dbl>         <dbl>
    ## 1 10003  48.0 male             7             1             2             0
    ## 2 10015  72.5 male             6            NA            NA            NA
    ## 3 10022  58.5 male            14             3             8            NA
    ## 4 10026  72.7 male            20             6            18            16
    ## 5 10035  60.4 male             4             0             1             2
    ## # … with 1,082 more rows

``` r
pulse_tidy_data <- 
  pivot_longer(
    pulse_data, 
    bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    values_to = "bdi")

pulse_tidy_data
```

    ## # A tibble: 4,348 x 5
    ##      id   age sex   visit           bdi
    ##   <dbl> <dbl> <chr> <chr>         <dbl>
    ## 1 10003  48.0 male  bdi_score_bl      7
    ## 2 10003  48.0 male  bdi_score_01m     1
    ## 3 10003  48.0 male  bdi_score_06m     2
    ## 4 10003  48.0 male  bdi_score_12m     0
    ## 5 10015  72.5 male  bdi_score_bl      6
    ## # … with 4,343 more rows

``` r
pulse_tidy_data <- 
  pivot_longer(
    pulse_data, 
    bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    names_prefix = "bdi_score_",
    values_to = "bdi")

pulse_tidy_data
```

    ## # A tibble: 4,348 x 5
    ##      id   age sex   visit   bdi
    ##   <dbl> <dbl> <chr> <chr> <dbl>
    ## 1 10003  48.0 male  bl        7
    ## 2 10003  48.0 male  01m       1
    ## 3 10003  48.0 male  06m       2
    ## 4 10003  48.0 male  12m       0
    ## 5 10015  72.5 male  bl        6
    ## # … with 4,343 more rows

``` r
#can pipe altogether as:

pulse_data <- 
  haven::read_sas("./data_import_examples/public_pulse_data.sas7bdat") %>%
  janitor::clean_names() %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    names_prefix = "bdi_score_",
    values_to = "bdi")
```

``` r
read_csv("./data_import_examples/FAS_litters.csv", col_types = "ccddiiii") %>% 
  janitor::clean_names() %>%
  count(group)
```

    ## # A tibble: 6 x 2
    ##   group     n
    ##   <chr> <int>
    ## 1 Con7      7
    ## 2 Con8      8
    ## 3 Low7      8
    ## 4 Low8      7
    ## 5 Mod7     12
    ## 6 Mod8      7

``` r
litters_data = 
  read_csv("./data_import_examples/FAS_litters.csv", col_types = "ccddiiii") %>% 
  janitor::clean_names() %>%
  separate(group, into = c("dose", "day_of_tx"), sep = 3) %>%
  mutate(
    dose = str_to_lower(dose),
    wt_gain = gd18_weight - gd0_weight)

litters_data
```

    ## # A tibble: 49 x 10
    ##   dose  day_of_tx litter_number gd0_weight gd18_weight gd_of_birth
    ##   <chr> <chr>     <chr>              <dbl>       <dbl>       <int>
    ## 1 con   7         #85                 19.7        34.7          20
    ## 2 con   7         #1/2/95/2           27          42            19
    ## 3 con   7         #5/5/3/83/3-3       26          41.4          19
    ## 4 con   7         #5/4/2/95/2         28.5        44.1          19
    ## 5 con   7         #4/2/95/3-3         NA          NA            20
    ## # … with 44 more rows, and 4 more variables: pups_born_alive <int>,
    ## #   pups_dead_birth <int>, pups_survive <int>, wt_gain <dbl>

``` r
analysis_result <- tibble(
  group = c("treatment", "treatment", "placebo", "placebo"),
  time = c("pre", "post", "pre", "post"),
  mean = c(4, 8, 3.5, 4)
)

analysis_result
```

    ## # A tibble: 4 x 3
    ##   group     time   mean
    ##   <chr>     <chr> <dbl>
    ## 1 treatment pre     4  
    ## 2 treatment post    8  
    ## 3 placebo   pre     3.5
    ## 4 placebo   post    4

``` r
pivot_wider(
  analysis_result, 
  names_from = "time", 
  values_from = "mean")
```

    ## # A tibble: 2 x 3
    ##   group       pre  post
    ##   <chr>     <dbl> <dbl>
    ## 1 treatment   4       8
    ## 2 placebo     3.5     4

``` r
fellowship_ring = 
  readxl::read_excel("./data_import_examples/LotR_Words.xlsx", range = "B3:D6") %>%
  mutate(movie = "fellowship_ring")

two_towers = 
  readxl::read_excel("./data_import_examples/LotR_Words.xlsx", range = "F3:H6") %>%
  mutate(movie = "two_towers")

return_king = 
  readxl::read_excel("./data_import_examples/LotR_Words.xlsx", range = "J3:L6") %>%
  mutate(movie = "return_king")
```

``` r
lotr_tidy = 
  bind_rows(fellowship_ring, two_towers, return_king) %>%
  janitor::clean_names() %>%
  pivot_longer(
    female:male,
    names_to = "sex", 
    values_to = "words") %>%
  mutate(race = str_to_lower(race)) %>%
  select(movie, everything())

lotr_tidy
```

    ## # A tibble: 18 x 4
    ##    movie           race   sex    words
    ##    <chr>           <chr>  <chr>  <dbl>
    ##  1 fellowship_ring elf    female  1229
    ##  2 fellowship_ring elf    male     971
    ##  3 fellowship_ring hobbit female    14
    ##  4 fellowship_ring hobbit male    3644
    ##  5 fellowship_ring man    female     0
    ##  6 fellowship_ring man    male    1995
    ##  7 two_towers      elf    female   331
    ##  8 two_towers      elf    male     513
    ##  9 two_towers      hobbit female     0
    ## 10 two_towers      hobbit male    2463
    ## 11 two_towers      man    female   401
    ## 12 two_towers      man    male    3589
    ## 13 return_king     elf    female   183
    ## 14 return_king     elf    male     510
    ## 15 return_king     hobbit female     2
    ## 16 return_king     hobbit male    2673
    ## 17 return_king     man    female   268
    ## 18 return_king     man    male    2459

``` r
pup_data <- 
  read_csv("./data_import_examples/FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names() %>%
  mutate(sex = recode(sex, `1` = "male", `2` = "female"))

litter_data <- 
  read_csv("./data_import_examples/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group))

fas_data <- 
  left_join(pup_data, litter_data, by = "litter_number")
fas_data
```

    ## # A tibble: 313 x 13
    ##   litter_number sex   pd_ears pd_eyes pd_pivot pd_walk group gd0_weight
    ##   <chr>         <chr>   <int>   <int>    <int>   <int> <chr>      <dbl>
    ## 1 #85           male        4      13        7      11 con7        19.7
    ## 2 #85           male        4      13        7      12 con7        19.7
    ## 3 #1/2/95/2     male        5      13        7       9 con7        27  
    ## 4 #1/2/95/2     male        5      13        8      10 con7        27  
    ## 5 #5/5/3/83/3-3 male        5      13        8      10 con7        26  
    ## # … with 308 more rows, and 5 more variables: gd18_weight <dbl>,
    ## #   gd_of_birth <int>, pups_born_alive <int>, pups_dead_birth <int>,
    ## #   wt_gain <dbl>
