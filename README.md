
<!-- README.md is generated from README.Rmd. Please edit that file -->

# covid19kosovo

<!-- badges: start -->
<!-- badges: end -->

## Installation

You can install the released version of covid19kosovo from GitHub with:

``` r
# install.library("devtool)
devtools::install_github("Kushtrimvisoka/covid19kosovo")
```

## Example

This is a basic example which shows you how to solve a common problem:

``` r
library(covid19kosovo)
```

What is special about using `README.Rmd` instead of just `README.md`?
You can include R chunks like so:

``` r
data <- covid19kosovo(level = "total") 
#> Downloading data from 'url'...

head(data)
#>         date confirmed healed dead confirmed_cumulative healed_cumulative
#> 1 2020-03-13         2      0    0                    2                 0
#> 2 2020-03-14         3      0    0                    5                 0
#> 3 2020-03-15         3      0    0                    8                 0
#> 4 2020-03-16         7      0    0                   15                 0
#> 5 2020-03-17         4      0    0                   19                 0
#> 6 2020-03-18         1      0    0                   20                 0
#>   dead_cumulative active
#> 1               0      2
#> 2               0      5
#> 3               0      8
#> 4               0     15
#> 5               0     19
#> 6               0     20
```

``` r
data <- covid19kosovo(level = "municipality") 
#> Downloading data from 'url'...

head(data)
#>         date id municipality confirmed
#> 1 2020-03-13  8        Klinë         1
#> 2 2020-03-13 26         Viti         1
#> 3 2020-03-14 30    Malishevë         1
#> 4 2020-03-14 26         Viti         2
#> 5 2020-03-15 30    Malishevë         3
#> 6 2020-03-16 15       Obiliq         1
```

``` r
data <- covid19kosovo(level = "village") 
#> Downloading data from 'url'...

head(data)
#>         date id municipality    cadastral_zone confirmed
#> 1 2020-03-13  8        Klinë         Dranashiq         1
#> 2 2020-03-13 26         Viti Stubëll e Poshtme         1
#> 3 2020-03-14 30    Malishevë           Bubavec         1
#> 4 2020-03-14 26         Viti Stubëll e Poshtme         2
#> 5 2020-03-15 30    Malishevë           Bubavec         1
#> 6 2020-03-15 30    Malishevë     Llashkadrenoc         1
```

``` r
library(tidyverse)
#> ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──
#> ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
#> ✓ tibble  3.0.4     ✓ dplyr   1.0.2
#> ✓ tidyr   1.1.2     ✓ stringr 1.4.0
#> ✓ readr   1.4.0     ✓ forcats 0.5.0
#> ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()
library(sf)
#> Linking to GEOS 3.8.1, GDAL 3.2.0, PROJ 7.2.0
library(kosovomaps)

rksmap <- mapof("municip")
  
data <- covid19kosovo(level = "municipality") %>% 
  group_by(id, municipality) %>% 
  summarise(confirmed = sum(confirmed))
#> Downloading data from 'url'...
#> `summarise()` regrouping output by 'id' (override with `.groups` argument)

rksmap <- merge(rksmap, data)

p <- ggplot()+
  geom_sf(data = rksmap, aes(fill = confirmed)) +
  scale_fill_viridis_c("Confirmed", direction = -1) +
  theme_void()

p
```

<img src="man/figures/README-unnamed-chunk-6-1.png" width="100%" />

In that case, don’t forget to commit and push the resulting figure
files, so they display on GitHub!
