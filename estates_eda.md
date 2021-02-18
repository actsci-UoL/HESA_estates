---
title: "Estates EDA"
output:
  html_document: 
    keep_md: yes
---




```r
library(tidyverse)
library(here)
```

# Eda of HESA Estates data

Environmental information for UK higher education providers collected as part of the Estates management record. The data covers the academic years 2015/16 to 2018/19.

The file `estates.csv` was created by `estates_preproc.Rmd`.


```r
estates <- 
  read_csv(here('ProcData', 'estates.csv'))
```

```
## Parsed with column specification:
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
```
## Can we match the HESA data with figures in the UoL dashboard?

### Electricity use

From the filter below we can see that the 2018 figure is 36654250 kWh (this matches the figure taken directly from the web page, which is where I got the units from).



```r
estates %>% 
  filter(str_detect(he_provider, "Leicester")) %>% 
  filter(academic_year == "2018/19") %>% 
  filter(str_detect(category, "electricity")) %>% 
  select(he_provider, category_marker, value) %>% 
  arrange(desc(value))
```

```
## # A tibble: 3 x 3
##   he_provider               category_marker                                value
##   <chr>                     <chr>                                          <dbl>
## 1 The University of Leices… Energy consumption                            3.67e7
## 2 The University of Leices… Scope 1 and 2 carbon emissions                9.37e6
## 3 The University of Leices… Total generation of electricity exported t…   0.
```


From a UK government [publication](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/790626/2018-provisional-emissions-statistics-report.pdf){target="_blank"}, we can find a conversion factor of 180 tonnes of $CO_2$ per GWh on average.

So the university's electricity consumption should be equivalent to $180 \times 36.654250 = 6598", which doesn't match the figure given in the dashboard of 9369. So more investigation to be done there! Perhaps the national grid figure used is different.


