---
title: "lab4_hw_rev_SBUCD"
output: 
  html_document: 
    keep_md: yes
---



# Load the tidyverse

```r
library(tidyverse)
```

```
## ── Attaching packages ────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
## ✔ tibble  2.0.0     ✔ dplyr   0.7.8
## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
## ✔ readr   1.3.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ───────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

# Load mammal data

```r
life_history <- readr::read_csv("/Users/sophieborison/Desktop/class_files-master/data/mammal_lifehistories_v2.csv")
```

```
## Parsed with column specification:
## cols(
##   order = col_character(),
##   family = col_character(),
##   Genus = col_character(),
##   species = col_character(),
##   mass = col_double(),
##   gestation = col_double(),
##   newborn = col_double(),
##   weaning = col_double(),
##   `wean mass` = col_double(),
##   AFR = col_double(),
##   `max. life` = col_double(),
##   `litter size` = col_double(),
##   `litters/year` = col_double()
## )
```

# Rename some of the variables

```r
life_history <- 
  life_history %>% 
  dplyr::rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year`
          )
```

# 1.
### Explore the data using the method that you prefer. Below, I am using a new package called `skimr`. If you want to use this, make sure that it is installed.

### Install `skimr`

```r
# install.packages("skimr")
```

### Load `skimr`

```r
library("skimr")
```

### Use `skimr`

```r
life_history %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## ── Variable type:character ────────────────────────────────────────────────────────────────────────
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────
##     variable missing complete    n      mean         sd   p0  p25     p50
##          AFR       0     1440 1440   -408.12     504.97 -999 -999    2.5 
##    gestation       0     1440 1440   -287.25     455.36 -999 -999    1.05
##  litter_size       0     1440 1440    -55.63     234.88 -999    1    2.27
##   litters_yr       0     1440 1440   -477.14     500.03 -999 -999    0.38
##         mass       0     1440 1440 383576.72 5055162.92 -999   50  403.02
##     max_life       0     1440 1440   -490.26     615.3  -999 -999 -999   
##      newborn       0     1440 1440   6703.15   90912.52 -999 -999    2.65
##    wean_mass       0     1440 1440  16048.93   5e+05    -999 -999 -999   
##      weaning       0     1440 1440   -427.17     496.71 -999 -999    0.73
##      p75          p100     hist
##    15.61     210       ▆▁▁▁▁▁▇▁
##     4.5       21.46    ▃▁▁▁▁▁▁▇
##     3.83      14.18    ▁▁▁▁▁▁▁▇
##     1.15       7.5     ▇▁▁▁▁▁▁▇
##  7009.17       1.5e+08 ▇▁▁▁▁▁▁▁
##   147.25    1368       ▇▁▁▃▂▁▁▁
##    98    2250000       ▇▁▁▁▁▁▁▁
##    10          1.9e+07 ▇▁▁▁▁▁▁▁
##     2         48       ▆▁▁▁▁▁▁▇
```

# 2.
### Run the code below. Are there any NA's in the data? Does this seem likely?

```r
life_history %>% 
  summarize(number_nas= sum(is.na(life_history)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```

### R reports that there are no NA's in the data. This is unlikely. 

# 3.
### Are there any missing data (NA's) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?

### There are 1440 NA's represented by -999. There are the most NA's in wean mass, max life, and litters year. These variables may be difficult to measure in a species, or the species may be difficult to find or hard to get to. What are the chances of finding a weaning endangered South Andean deer in the high elevation Andes?

### Count number of -999's

```r
life_history %>%
 count(-999)
```

```
## # A tibble: 1 x 2
##   `-999`     n
##    <dbl> <int>
## 1   -999  1440
```

### Replace -999 with NA

```r
life_history_na <- 
  life_history %>% 
  na_if(-999)
```

### Sort the NA's

```r
life_history_na %>% 
  purrr::map_df(~sum(is.na(.))) %>% 
  tidyr::gather(variables, num_nas) %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 13 x 2
##    variables   num_nas
##    <chr>         <int>
##  1 wean_mass      1039
##  2 max_life        841
##  3 litters_yr      689
##  4 weaning         619
##  5 AFR             607
##  6 newborn         595
##  7 gestation       418
##  8 mass             85
##  9 litter_size      84
## 10 order             0
## 11 family            0
## 12 genus             0
## 13 species           0
```

# 4.
### Compared to the `msleep` data, we have better representation among taxa. Produce a summary that shows the number of observations by taxonomic order.


```r
life_history_na %>%
  group_by(order) %>%
  summarize(total=n())
```

```
## # A tibble: 17 x 2
##    order          total
##    <chr>          <int>
##  1 Artiodactyla     161
##  2 Carnivora        197
##  3 Cetacea           55
##  4 Dermoptera         2
##  5 Hyracoidea         4
##  6 Insectivora       91
##  7 Lagomorpha        42
##  8 Macroscelidea     10
##  9 Perissodactyla    15
## 10 Pholidota          7
## 11 Primates         156
## 12 Proboscidea        2
## 13 Rodentia         665
## 14 Scandentia         7
## 15 Sirenia            5
## 16 Tubulidentata      1
## 17 Xenarthra         20
```

# 5
### Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.


```r
life_history_na %>%
  group_by(max_life) %>%
  summarize(min_max_life=min(max_life, na.rm=TRUE),
          max_max_life=max(max_life, na.rm=TRUE),
          mean_max_life=mean(max_life, na.rm=TRUE),
          stdev_max_life=sd(max_life, na.rm=TRUE),
          total=n())
```

```
## # A tibble: 246 x 6
##    max_life min_max_life max_max_life mean_max_life stdev_max_life total
##       <dbl>        <dbl>        <dbl>         <dbl>          <dbl> <int>
##  1       12           12           12            12             NA     1
##  2       14           14           14            14              0     2
##  3       16           16           16            16              0     2
##  4       17           17           17            17             NA     1
##  5       18           18           18            18              0     7
##  6       20           20           20            20             NA     1
##  7       21           21           21            21              0     2
##  8       22           22           22            22              0     2
##  9       24           24           24            24              0     9
## 10       25           25           25            25             NA     1
## # … with 236 more rows
```

# 6
### Let's look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?

### Proboscidea has the longest mean gestation.

```r
life_history_na %>%
  select(order, gestation, newborn, wean_mass) %>%
  group_by(order) %>%
  summarize(mean_gestation=mean(gestation, na.rm=TRUE),
            mean_newborn=mean(newborn, na.rm=TRUE),
            mean_wean_mass=mean(wean_mass, na.rm=TRUE)) %>%
  arrange(desc(mean_gestation))
```

```
## # A tibble: 17 x 4
##    order          mean_gestation mean_newborn mean_wean_mass
##    <chr>                   <dbl>        <dbl>          <dbl>
##  1 Proboscidea             21.3      99523.         600000  
##  2 Perissodactyla          13.0      27015.         382191. 
##  3 Cetacea                 11.8     343077.        4796125  
##  4 Sirenia                 10.8      22878.          67500  
##  5 Hyracoidea               7.4        231.            500  
##  6 Artiodactyla             7.26      7082.          51025. 
##  7 Tubulidentata            7.08      1734            6250  
##  8 Primates                 5.47       287.           2115. 
##  9 Xenarthra                4.95       314.            420  
## 10 Carnivora                3.69      3657.          21020. 
## 11 Pholidota                3.63       276.           2006. 
## 12 Dermoptera               2.75        35.9           NaN  
## 13 Macroscelidea            1.91        24.5           104. 
## 14 Scandentia               1.63        12.8           102. 
## 15 Rodentia                 1.31        35.5           135. 
## 16 Lagomorpha               1.18        57.0           715. 
## 17 Insectivora              1.15         6.06           33.1
```




