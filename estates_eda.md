Estates EDA
================

``` r
library(tidyverse)
library(here)
```

# Eda of HESA Estates data

Environmental information for UK higher education providers collected as
part of the Estates management record. The data covers the academic
years 2015/16 to 2018/19.

The file `estates.csv` was created by `estates_preproc.Rmd`.

``` r
estates <- 
  read_csv(here('ProcData', 'estates.csv'))
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   ukprn = col_double(),
    ##   he_provider = col_character(),
    ##   academic_year = col_character(),
    ##   country_of_he_provider = col_character(),
    ##   region_of_he_provider = col_character(),
    ##   category_marker = col_character(),
    ##   category = col_character(),
    ##   value = col_double()
    ## )

``` r
estates %>% 
  filter(str_detect(he_provider, "Leicester")) %>% 
  filter(category_marker == "Total carbon reduction target", academic_year == "2018/19") %>% 
  select(he_provider, category_marker, value) %>% 
  arrange(desc(value))
```

    ## # A tibble: 1 x 3
    ##   he_provider                 category_marker               value
    ##   <chr>                       <chr>                         <dbl>
    ## 1 The University of Leicester Total carbon reduction target    25
