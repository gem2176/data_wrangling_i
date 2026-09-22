Data Import
================

This file is for doing data import

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.1     ✔ stringr   1.5.1
    ## ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(readxl)
library(haven)
```

R wants to know where the files are (eg. where is the excel file?)

Two ways to do this:

- *Absolute path*: the full path from the “north pole”
- *Relative path*: path from where you are right now. Easy if you are
  doing the rdirectory because it can be reproducible. ALWAYS use
  relative paths.

Import our first data set

``` r
litters_df =
  read_csv(file = "data/FAS_litters.csv",
           na = c("", "NA", "."))
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
#if there is something that looks like numbers but is assigned character, something is wrong
#check the CSV and you will see that they used . and NA to represent NA values
#to change this, you can rename the NA categories
#there are many ways to data clean using read_csv

litters_df = janitor :: clean_names(litters_df)

#this makes the names in snake case rather than with a space 
```

Import your second data set

``` r
pups_df =
  read_csv(
    file= "data/FAS_pups.csv", 
    skip = 3,
    na = c("", "NA", ".")) # this part allows you to skip the first three rows 
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df = janitor:: clean_names(pups_df) 
#to prevent yourself to load too many libraries, you can use this format to use one function from a library without loading the entire thing. The package needs to be installed though. 
```

Head or tail

``` r
#look at the first rows of the data set
head(litters_df, 20) #number tells you how many rows to show
```

    ## # A tibble: 20 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## 11 Con8  #5/4/3/83/3           28          NA            19               9
    ## 12 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 13 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## 14 Con8  #2/2/95/2             NA          NA            19               5
    ## 15 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## 16 Mod7  #59                   17          33.4          19               8
    ## 17 Mod7  #103                  21.4        42.1          19               9
    ## 18 Mod7  #1/82/3-2             NA          NA            19               6
    ## 19 Mod7  #3/83/3-2             NA          NA            19               8
    ## 20 Mod7  #2/95/2-2             NA          NA            20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
#look at the last rows of the data set 
tail(litters_df,5)
```

    ## # A tibble: 5 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low8  #100                20          39.2          20               8
    ## 2 Low8  #4/84               21.8        35.2          20               4
    ## 3 Low8  #108                25.6        47.5          20               8
    ## 4 Low8  #99                 23.5        39            20               6
    ## 5 Low8  #110                25.5        42.7          20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

Skimming

``` r
skimr::skim(pups_df)
```

|                                                  |         |
|:-------------------------------------------------|:--------|
| Name                                             | pups_df |
| Number of rows                                   | 313     |
| Number of columns                                | 6       |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |         |
| Column type frequency:                           |         |
| character                                        | 1       |
| numeric                                          | 5       |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |         |
| Group variables                                  | None    |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| litter_number |         0 |             1 |   3 |  15 |     0 |       49 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |  mean |   sd |  p0 | p25 | p50 | p75 | p100 | hist  |
|:--------------|----------:|--------------:|------:|-----:|----:|----:|----:|----:|-----:|:------|
| sex           |         0 |          1.00 |  1.50 | 0.50 |   1 |   1 |   2 |   2 |    2 | ▇▁▁▁▇ |
| pd_ears       |        18 |          0.94 |  3.68 | 0.59 |   2 |   3 |   4 |   4 |    5 | ▁▅▁▇▁ |
| pd_eyes       |        13 |          0.96 | 12.99 | 0.62 |  12 |  13 |  13 |  13 |   15 | ▂▇▁▂▁ |
| pd_pivot      |        13 |          0.96 |  7.09 | 1.51 |   4 |   6 |   7 |   8 |   12 | ▂▇▂▂▁ |
| pd_walk       |         0 |          1.00 |  9.50 | 1.34 |   7 |   9 |   9 |  10 |   14 | ▆▇▇▂▁ |

Viewing

``` r
#view(pups_df)
#don't do this just do it in the console because otherwise every time you knit it will pop up an 
#additional window to view the df
```

reading csv!

Make sure you are using read_csv because read.csv is super old

## Oh excel…

Jenny Bryan made ‘readxl’ to solve our problems

``` r
mlb11 =
  read_excel("data/mlb11.xlsx")
```

Look at the data

``` r
mlb11
```

    ## # A tibble: 30 × 12
    ##    team        runs at_bats  hits homeruns bat_avg strikeouts stolen_bases  wins
    ##    <chr>      <dbl>   <dbl> <dbl>    <dbl>   <dbl>      <dbl>        <dbl> <dbl>
    ##  1 Texas Ran…   855    5659  1599      210   0.283        930          143    96
    ##  2 Boston Re…   875    5710  1600      203   0.28        1108          102    90
    ##  3 Detroit T…   787    5563  1540      169   0.277       1143           49    95
    ##  4 Kansas Ci…   730    5672  1560      129   0.275       1006          153    71
    ##  5 St. Louis…   762    5532  1513      162   0.273        978           57    90
    ##  6 New York …   718    5600  1477      108   0.264       1085          130    77
    ##  7 New York …   867    5518  1452      222   0.263       1138          147    97
    ##  8 Milwaukee…   721    5447  1422      185   0.261       1083           94    96
    ##  9 Colorado …   735    5544  1429      163   0.258       1201          118    73
    ## 10 Houston A…   615    5598  1442       95   0.258       1164          118    56
    ## # ℹ 20 more rows
    ## # ℹ 3 more variables: new_onbase <dbl>, new_slug <dbl>, new_obs <dbl>

Load some LotR data

Import FOTR words

``` r
fotr_df=
  read_excel(
    "data/LotR_Words.xlsx", 
    range = "B3:D6" #top left corner to bottom right
  )

#look at ?read_excel to see how to specify the range 
```

What about the two towers?

``` r
tt_df=
  read_excel(
    "data/LotR_Words.xlsx", 
    range = "F3:H6" #top left corner to bottom right
  )

tt_df
```

    ## # A tibble: 3 × 3
    ##   Race   Female  Male
    ##   <chr>   <dbl> <dbl>
    ## 1 Elf       331   513
    ## 2 Hobbit      0  2463
    ## 3 Man       401  3589

If you have excel file open while you are working in RStudio, it will
pop up in your git window. It will disappear if you close excel. Never
commit these.

\##Import SAS

Read in the PULSE data set

``` r
#similar structure to the others
pulse_df=
  read_sas("data/public_pulse_data.sas7bdat")

pulse_df = janitor:: clean_names(pulse_df)

#don't make variables capital, makes your variables follow a clean naming
#convention
```
