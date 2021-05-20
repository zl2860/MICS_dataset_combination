sample
================
Zongchao Liu
11/2/2020

This document is for simply checking MICS datasets.

We cover 38 countries/regions here

# Reorganize folders

``` r
countries = list.files('./data/非洲全部/')
setwd("./data/非洲全部/")
file.rename(from = countries, to = sapply(strsplit(countries, " MICS6"), "[[", 1))
```

    ##  [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
    ## [15] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
    ## [29] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE

# Helpers

``` r
# import a country's data
countries = list.files('./data/非洲全部/')
read_data = function(country){
  path = list.dirs(paste('./data/非洲全部/', country, sep = ''))[2]
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

# Export datasets
export_data = function(list){
  for (country in 1:length(list)) {
    path = str_c('./clean/',names(list)[country],'.xlsx')
    print(path)
    for (df in 1:length(list[country][[1]])) {
      write.xlsx(list[country][[1]][[df]], file = path, sheetName = names(list[country][[1]])[df])
    }
  }
}
```

# Import Data

``` r
list = read_all()
```

    ## [1] "Algeria"
    ## [1] "Bangladesh"
    ## [1] "Central African Republic"
    ## [1] "Chad"
    ## [1] "Costa Rica"
    ## [1] "Cuba"
    ## [1] "DRCongo"
    ## [1] "Georgia"
    ## [1] "Ghana"
    ## [1] "Guinea Bissau"
    ## [1] "Guyana"
    ## [1] "Iraq"
    ## [1] "Kiribati"
    ## [1] "Kosovo (UNSCR 1244)"
    ## [1] "Kosovo (UNSCR 1244) (Roma, Ashkali and Egyptian Communities)"
    ## [1] "Kyrgyz Republic"
    ## [1] "Lao PDR"
    ## [1] "Lesotho"
    ## [1] "Madagascar"
    ## [1] "Mongolia MICS 2018 SPSS Datasets"
    ## [1] "Montenegro"
    ## [1] "Montenegro (Roma Settlements)"
    ## [1] "Nepal"
    ## [1] "Pakistan Punjab"
    ## [1] "Republic of North Macedonia"
    ## [1] "Republic of North Macedonia (Roma Settlements)"
    ## [1] "Sao Tome and Principe"
    ## [1] "Serbia"
    ## [1] "Serbia (Roma Settlements)"
    ## [1] "Sierra Leone"
    ## [1] "State of Palestine"
    ## [1] "Suriname"
    ## [1] "The Gambia"
    ## [1] "Togo"
    ## [1] "Tonga"
    ## [1] "Tunisia"
    ## [1] "Turkmenistan"
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

| country                                                      | col\_bh | col\_ch | col\_fs | col\_gm | col\_hh | col\_hl | col\_wm | col\_fg | col\_mn | col\_tn | col\_ab | col\_lt | col\_lvalue | col\_mm | col\_pn | row\_bh | row\_ch | row\_fs | row\_gm | row\_hh | row\_hl | row\_wm | row\_fg | row\_mn | row\_tn | row\_ab | row\_lt | row\_lvalue | row\_mm | row\_pn |
| :----------------------------------------------------------- | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ----------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ------: | ----------: | ------: | ------: |
| Algeria                                                      |      49 |     425 |     200 |      27 |     257 |     109 |     507 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |   53847 |   15224 |   17210 |    2237 |   31325 |  151745 |   37227 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Bangladesh                                                   |      50 |     346 |     268 |      NA |     273 |      74 |     412 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |  121184 |   24686 |   40617 |      NA |   64400 |  262159 |   68709 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Central African Republic                                     |      49 |     452 |     409 |      NA |     238 |      86 |     444 |      41 |     244 |      32 |      NA |      NA |          NA |      NA |      NA |   26536 |    9037 |    6167 |      NA |    8994 |   45797 |    9778 |    9560 |    4375 |   13772 |      NA |      NA |          NA |      NA |      NA |
| Chad                                                         |      50 |     481 |     298 |      NA |     327 |      89 |     463 |      50 |     249 |      33 |      NA |      NA |          NA |      NA |      NA |   74153 |   21860 |   14865 |      NA |   19217 |  112604 |   22797 |   25227 |    7078 |   36286 |      NA |      NA |          NA |      NA |      NA |
| Costa Rica                                                   |      NA |     406 |     150 |      NA |     245 |      76 |     380 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |      NA |    3757 |    4122 |      NA |   10093 |   30161 |    8217 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Cuba                                                         |      NA |     453 |     117 |      NA |     238 |      65 |     377 |      NA |     210 |      NA |      NA |      NA |          NA |      NA |      NA |      NA |    5270 |    4306 |      NA |   12480 |   38688 |    8942 |      NA |    3809 |      NA |      NA |      NA |          NA |      NA |      NA |
| DRCongo                                                      |      50 |     521 |     288 |      NA |     206 |      94 |     424 |      NA |     238 |      36 |      NA |      NA |          NA |      NA |      NA |   61703 |   21477 |   14038 |      NA |   20810 |  103422 |   21828 |      NA |    6161 |   25347 |      NA |      NA |          NA |      NA |      NA |
| Georgia                                                      |      NA |     324 |     129 |      NA |     320 |      77 |     285 |      NA |     145 |      NA |      39 |      52 |           9 |      NA |      NA |      NA |    2824 |    4221 |      NA |   14120 |   41378 |    8511 |      NA |    4438 |      NA |     766 |    2633 |        2633 |      NA |      NA |
| Ghana                                                        |      64 |     463 |     285 |      NA |     359 |      90 |     412 |      44 |     214 |      36 |      NA |      NA |          NA |      NA |      NA |   34595 |    8903 |    8965 |      NA |   13202 |   61254 |   14609 |   10840 |    5476 |   25316 |      NA |      NA |          NA |      NA |      NA |
| Guinea Bissau                                                |      49 |     424 |     272 |      NA |     189 |      84 |     389 |      49 |     201 |      32 |      NA |      NA |          NA |      NA |      NA |   25966 |    7536 |    5849 |      NA |    7500 |   49172 |   11188 |   11144 |    3028 |   25567 |      NA |      NA |          NA |      NA |      NA |
| Guyana                                                       |      52 |     400 |     282 |      NA |     327 |      85 |     416 |      NA |     228 |      33 |      NA |      NA |          NA |      NA |      NA |   11377 |    2925 |    3423 |      NA |    8285 |   26209 |    6576 |      NA |    2916 |   12929 |      NA |      NA |          NA |      NA |      NA |
| Iraq                                                         |      57 |     416 |     152 |      NA |     224 |      73 |     421 |      40 |      NA |      NA |      NA |      NA |          NA |      28 |      NA |   70986 |   16689 |   15613 |      NA |   20521 |  131394 |   31060 |   12557 |      NA |      NA |      NA |      NA |          NA |  186790 |      NA |
| Kiribati                                                     |      51 |     428 |     277 |      NA |     312 |      88 |     597 |      NA |     293 |      37 |      NA |      NA |          NA |      NA |      NA |    8402 |    2189 |    2272 |      NA |    3280 |   17451 |    4235 |      NA |    2153 |    6391 |      NA |      NA |          NA |      NA |      NA |
| Kosovo (UNSCR 1244)                                          |      52 |     409 |     480 |      NA |     330 |      68 |     289 |      NA |     142 |      NA |      NA |      NA |          NA |      NA |      NA |    8556 |    1781 |    2691 |      NA |    6556 |   25034 |    6483 |      NA |    3197 |      NA |      NA |      NA |          NA |      NA |      NA |
| Kosovo (UNSCR 1244) (Roma, Ashkali and Egyptian Communities) |      47 |     404 |     475 |      NA |     327 |      63 |     284 |      NA |     137 |      NA |      NA |      NA |          NA |      NA |      NA |    3605 |     811 |     839 |      NA |    1459 |    6956 |    1721 |      NA |     874 |      NA |      NA |      NA |          NA |      NA |      NA |
| Kyrgyz Republic                                              |      57 |     446 |     266 |      NA |     268 |      72 |     395 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |   12742 |    3552 |    3897 |      NA |    7200 |   28180 |    5826 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Lao PDR                                                      |      57 |     443 |     129 |      NA |     314 |      85 |     409 |      NA |     184 |      34 |      NA |      NA |          NA |      NA |      NA |   54163 |   11812 |   15494 |      NA |   23299 |  106623 |   26088 |      NA |   12687 |   59294 |      NA |      NA |          NA |      NA |      NA |
| Lesotho                                                      |      58 |     440 |     556 |      NA |     301 |      71 |     361 |      NA |     190 |      NA |      NA |      NA |          NA |      NA |      NA |   11313 |    3560 |    5301 |      NA |   10413 |   35110 |    8521 |      NA |    4505 |      NA |      NA |      NA |          NA |      NA |      NA |
| Madagascar                                                   |      48 |     453 |     567 |      NA |     335 |      96 |     519 |      NA |     222 |      34 |      NA |      NA |          NA |      35 |      NA |   46867 |   13355 |   12429 |      NA |   20117 |   82875 |   18812 |      NA |    8980 |   33837 |      NA |      NA |          NA |   89420 |      NA |
| Mongolia MICS 2018 SPSS Datasets                             |      65 |     443 |     277 |      NA |     209 |      69 |     444 |      NA |     241 |      NA |      43 |      NA |          NA |      NA |      NA |   22935 |    6269 |    7628 |      NA |   14500 |   49839 |   11737 |      NA |    5513 |      NA |     594 |      NA |          NA |      NA |      NA |
| Montenegro                                                   |      NA |     238 |     160 |      NA |     280 |      71 |     383 |      NA |     176 |      NA |      NA |      NA |          NA |      NA |      NA |      NA |    1329 |    1352 |      NA |    6000 |   13391 |    2928 |      NA |    1479 |      NA |      NA |      NA |          NA |      NA |      NA |
| Montenegro (Roma Settlements)                                |      NA |     233 |     156 |      NA |     275 |      66 |     381 |      NA |     171 |      NA |      NA |      NA |          NA |      NA |      NA |      NA |     736 |     606 |      NA |    1165 |    4732 |    1048 |      NA |     591 |      NA |      NA |      NA |          NA |      NA |      NA |
| Nepal                                                        |      53 |     449 |     254 |      NA |     471 |      84 |     414 |      NA |     199 |      NA |      NA |      NA |          NA |      NA |      NA |   27345 |    6749 |    7824 |      NA |   12800 |   56595 |   15019 |      NA |    5605 |      NA |      NA |      NA |          NA |      NA |      NA |
| Pakistan Punjab                                              |      60 |     418 |     274 |      NA |     329 |     102 |     418 |      NA |     215 |      NA |      NA |      NA |          NA |      38 |      NA |  157899 |   42408 |   37052 |      NA |   53840 |  330027 |   79510 |      NA |   39458 |      NA |      NA |      NA |          NA |  401080 |      NA |
| Republic of North Macedonia                                  |      51 |     358 |     346 |      NA |     202 |      73 |     285 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |    4887 |    1564 |    1479 |      NA |    4777 |   15635 |    3391 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Republic of North Macedonia (Roma Settlements)               |      44 |     351 |     339 |      NA |     197 |      66 |     279 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |    3035 |     703 |     784 |      NA |    1584 |    6249 |    1500 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Sao Tome and Principe                                        |      51 |     451 |     299 |      NA |     238 |      87 |     447 |      NA |     220 |      34 |      NA |      NA |          NA |      NA |      NA |    7477 |    1859 |    2275 |      NA |    3728 |   13957 |    3244 |      NA |    1570 |    6333 |      NA |      NA |          NA |      NA |      NA |
| Serbia                                                       |      NA |     433 |     171 |      NA |     284 |     115 |     324 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |      NA |    1967 |    1824 |      NA |    8101 |   20517 |    4219 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Serbia (Roma Settlements)                                    |      NA |     432 |     170 |      NA |     283 |     114 |     323 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |      NA |    1096 |    1010 |      NA |    1934 |    8329 |    1912 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Sierra Leone                                                 |      63 |     456 |     286 |      NA |     322 |      95 |     534 |      48 |     216 |      38 |      NA |      NA |          NA |      NA |     133 |   42071 |   11774 |   11046 |      NA |   15605 |   75015 |   18006 |   18210 |    7534 |   26785 |      NA |      NA |          NA |      NA |   11906 |
| State of Palestine                                           |      55 |     387 |     283 |      NA |     252 |      79 |     356 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |   25482 |    6394 |    5479 |      NA |   10080 |   46729 |   11464 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Suriname                                                     |      60 |     374 |     281 |      NA |     319 |      75 |     378 |      NA |     207 |      NA |      NA |      NA |          NA |      NA |      NA |   13890 |    4654 |    4329 |      NA |    9508 |   34515 |    8533 |      NA |    4025 |      NA |      NA |      NA |          NA |      NA |      NA |
| The Gambia                                                   |      58 |     465 |     288 |      NA |     203 |      83 |     409 |      41 |     216 |      37 |      NA |      NA |          NA |      NA |      NA |   37009 |   10156 |    5850 |      NA |    7750 |   61858 |   14297 |   16364 |    5226 |   30296 |      NA |      NA |          NA |      NA |      NA |
| Togo                                                         |      58 |     467 |     299 |      NA |     297 |      80 |     423 |      42 |     209 |      33 |      NA |      NA |          NA |      NA |      NA |   18511 |    5030 |    5062 |      NA |    8404 |   34988 |    7657 |    6046 |    2456 |   12738 |      NA |      NA |          NA |      NA |      NA |
| Tonga                                                        |      58 |     430 |     439 |      NA |     335 |      83 |     611 |      NA |     253 |      NA |      NA |      NA |          NA |      NA |      NA |    5507 |    1378 |    1666 |      NA |    2751 |   13122 |    3157 |      NA |    1453 |      NA |      NA |      NA |          NA |      NA |      NA |
| Tunisia                                                      |      43 |     462 |     243 |      NA |     193 |      72 |     363 |      NA |     203 |      NA |      NA |      NA |          NA |      NA |      NA |   14058 |    3474 |    4983 |      NA |   11996 |   44276 |   11017 |      NA |    2673 |      NA |      NA |      NA |          NA |      NA |      NA |
| Turkmenistan                                                 |      50 |     213 |     310 |      NA |     200 |      67 |     346 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |   13549 |    3729 |    3776 |      NA |    6361 |   31192 |    7732 |      NA |      NA |      NA |      NA |      NA |          NA |      NA |      NA |
| Zimbabwe                                                     |      64 |     466 |     356 |      NA |     327 |      88 |     443 |      NA |     166 |      33 |      NA |      NA |          NA |      48 |      NA |   22844 |    6223 |    7155 |      NA |   12012 |   44472 |   10703 |      NA |    4677 |   10519 |      NA |      NA |          NA |   47835 |      NA |

``` r
# store temp res
check_v = check %>% 
  pivot_wider(., names_from = "df_name",
              values_from = c("col", "row"))
check_v[is.na(check_v)] = 0
```

# A simple example to call data

``` r
# call data for a specific country
list$Algeria
# call specific datasets of a given country(bh)
list$Algeria$bh
```

# A summary of the imported datasets

``` r
missing_dataset_count = colSums(check_v[,1:16] == 0)[-1]
merged_num = colSums(check_v[,17:31])
str_remove(names(missing_dataset_count),"col_")
```

    ##  [1] "bh"     "ch"     "fs"     "gm"     "hh"     "hl"     "wm"    
    ##  [8] "fg"     "mn"     "tn"     "ab"     "lt"     "lvalue" "mm"    
    ## [15] "pn"

Note:

  - 除去Thailand，共有38个国家/地区被纳入。

  - 理想条件下，38个地区共有15种类型的数据集可以合并，这15个数据集分别是bh, ch, fs, gm, hh, hl, wm, fg,
    mn, tn, ab, lt, lvalue, mm,
    pn

  - 由于并不是每个国家或地区都会全数含有这15个数据集（比如A国只有7个，B国只有4个，C国有15个），我们需要根据以下的基本结果考虑合并15种数据集中的哪些数据集:

<!-- end list -->

1.  38个国家/地区中，15类数据集的缺失数

<!-- end list -->

``` r
names(missing_dataset_count) = str_remove(names(missing_dataset_count),"col_")
missing_dataset_count %>% knitr::kable()
```

|        |  x |
| ------ | -: |
| bh     |  7 |
| ch     |  0 |
| fs     |  0 |
| gm     | 37 |
| hh     |  0 |
| hl     |  0 |
| wm     |  0 |
| fg     | 30 |
| mn     | 11 |
| tn     | 24 |
| ab     | 36 |
| lt     | 37 |
| lvalue | 37 |
| mm     | 34 |
| pn     | 37 |

2.  每类数据集合并后至多的样本数

<!-- end list -->

``` r
names(merged_num) = str_remove(names(merged_num),"row_")
merged_num %>% knitr::kable()
```

|        |       x |
| ------ | ------: |
| bh     | 1042494 |
| ch     |  294740 |
| fs     |  293499 |
| gm     |    2237 |
| hh     |  496167 |
| hl     | 2261650 |
| wm     |  538202 |
| fg     |  109948 |
| mn     |  151937 |
| tn     |  325410 |
| ab     |    1360 |
| lt     |    2633 |
| lvalue |    2633 |
| mm     |  725125 |
| pn     |   11906 |

综上，可以优先考虑合并 `bh(含有31/38), ch(38/38), fs(38/38), hh(38/38), hl(38/38),
wm(38/38), mn(27/38)`。 基本不用考虑合并 `gm(1/38), ab(2/38), lt(1/38),
lvalue(1/38), mm(4/38), pn(1/38)`。其他的数据集看具体需求酌情合并。
