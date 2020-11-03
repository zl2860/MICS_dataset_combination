sample
================
Zongchao Liu
11/2/2020

This document is for simply checking MICS datasets.

The regions we use here are:

  - DRCongo

  - Gambia

  - Zimbabwe

  - Ghana

  - Guinea Bissau

  - Lesotho

  - Madagascar

  - Togo

  - Tunisia

  - Zimbabwe

# Helpers

``` r
# import a country's data
countries = list.files('./data/')
read_data = function(country){
  path = list.dirs(paste('./data/', country, sep = ''))[2]
  file_list = list.files(path)
  list = NULL
  for (i in 1:length(file_list)) {
    df = spss.get(paste(path,file_list[i],sep = '/'))
    list[[i]] = df
  }
  names(list) = str_remove(file_list, ".sav")
  return(list)
}

# import all country's data
read_all = function(country_list = countries){
  list = NULL
  for (i in 1:length(countries)) {
    print(countries[i])
    list[[i]] = read_data(countries[i])
  }
  names(list) = country_list
  return(list)
}

# check dimensions & names of the dataframes
check_data = function(list, country_s = NULL, country_list = countries){
  res = NULL
  for (country in 1:length(country_list)) {
    for (df in 1:length(list[country][[1]])) {
      temp = tibble(country = as.character(names(list)[country]),
                        df_name = as.character(names(list[country][[1]])[df]),
                        row = dim(list[country][[1]][[df]])[1],
                        col = dim(list[country][[1]][[df]])[2])
      res = rbind(res, temp)
      #print(temp)
    }
  }
  return(res)
}
```

# Import Data

``` r
list = read_all()
```

    ## [1] "DRCongo"
    ## [1] "Gambia"
    ## [1] "Ghana"
    ## [1] "Guinea Bissau"
    ## [1] "Lesotho"
    ## [1] "Madagascar"
    ## [1] "Togo"
    ## [1] "Tunisia"
    ## [1] "Zimbabwe"

``` r
# call a specific country's data
# list$DRCongo
# call a country's specific dataframe
# list$DRCongo$bh
```

# check data

## names & dimensions

  - `col_xx`: \# of columns for the xx dataframe for that country

  - `row_xx`: \# of observations for the xx dataframe for that country

<!-- end list -->

``` r
check = check_data(list = list)
check %>% 
  pivot_wider(., names_from = "df_name",
              values_from = c("col", "row")) %>%
  knitr::kable()
```

| country       | col\_bh | col\_ch | col\_fs | col\_hh | col\_hl | col\_mn | col\_tn | col\_wm | col\_fg | col\_mm | row\_bh | row\_ch | row\_fs | row\_hh | row\_hl | row\_mn | row\_tn | row\_wm | row\_fg | row\_mm |
| :------------ | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: |
| DRCongo       |      50 |     521 |     288 |     206 |      94 |     238 |      36 |     424 |      NA |      NA |   61703 |   21477 |   14038 |   20810 |  103422 |    6161 |   25347 |   21828 |      NA |      NA |
| Gambia        |      58 |     465 |     288 |     203 |      83 |     216 |      37 |     409 |      41 |      NA |   37009 |   10156 |    5850 |    7750 |   61858 |    5226 |   30296 |   14297 |   16364 |      NA |
| Ghana         |      64 |     463 |     285 |     359 |      90 |     214 |      36 |     412 |      44 |      NA |   34595 |    8903 |    8965 |   13202 |   61254 |    5476 |   25316 |   14609 |   10840 |      NA |
| Guinea Bissau |      49 |     424 |     272 |     189 |      84 |     201 |      32 |     389 |      49 |      NA |   25966 |    7536 |    5849 |    7500 |   49172 |    3028 |   25567 |   11188 |   11144 |      NA |
| Lesotho       |      NA |      NA |      NA |      NA |      NA |     190 |      NA |     361 |      NA |      NA |      NA |      NA |      NA |      NA |      NA |    4505 |      NA |    8521 |      NA |      NA |
| Madagascar    |      48 |     453 |     567 |     335 |      96 |     222 |      34 |     519 |      NA |      35 |   46867 |   13355 |   12429 |   20117 |   82875 |    8980 |   33837 |   18812 |      NA |   89420 |
| Togo          |      58 |     467 |     299 |     297 |      80 |     209 |      33 |     423 |      42 |      NA |   18511 |    5030 |    5062 |    8404 |   34988 |    2456 |   12738 |    7657 |    6046 |      NA |
| Tunisia       |      43 |     462 |     243 |     193 |      72 |     203 |      NA |     363 |      NA |      NA |   14058 |    3474 |    4983 |   11996 |   44276 |    2673 |      NA |   11017 |      NA |      NA |
| Zimbabwe      |      64 |     466 |     356 |     327 |      88 |     166 |      33 |     443 |      NA |      48 |   22844 |    6223 |    7155 |   12012 |   44472 |    4677 |   10519 |   10703 |      NA |   47835 |

# To be continued…
