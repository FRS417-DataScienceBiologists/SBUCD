---
title: "lab2_hw_SBUCD"
output: 
  html_document: 
    keep_md: yes
---




```r
getwd()
```

```
## [1] "/Users/sophieborison/Desktop/SBUCD"
```


```r
library("tidyverse")
```

```
## ── Attaching packages ──────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
## ✔ tibble  2.0.0     ✔ dplyr   0.7.8
## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
## ✔ readr   1.3.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ─────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

# 1
#### This data is taken from a PNAS publication by V.M. Savage and G.B. West.

```r
?msleep
```

# 2
#### Msleep has 11 columns detailing the sleep habits of 83 mammals, along with information about genus, "vore," order, and conservation status.

```r
names(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"       
##  [5] "conservation" "sleep_total"  "sleep_rem"    "sleep_cycle" 
##  [9] "awake"        "brainwt"      "bodywt"
```

```r
glimpse(msleep)
```

```
## Observations: 83
## Variables: 11
## $ name         <chr> "Cheetah", "Owl monkey", "Mountain beaver", "Greate…
## $ genus        <chr> "Acinonyx", "Aotus", "Aplodontia", "Blarina", "Bos"…
## $ vore         <chr> "carni", "omni", "herbi", "omni", "herbi", "herbi",…
## $ order        <chr> "Carnivora", "Primates", "Rodentia", "Soricomorpha"…
## $ conservation <chr> "lc", NA, "nt", "lc", "domesticated", NA, "vu", NA,…
## $ sleep_total  <dbl> 12.1, 17.0, 14.4, 14.9, 4.0, 14.4, 8.7, 7.0, 10.1, …
## $ sleep_rem    <dbl> NA, 1.8, 2.4, 2.3, 0.7, 2.2, 1.4, NA, 2.9, NA, 0.6,…
## $ sleep_cycle  <dbl> NA, NA, NA, 0.1333333, 0.6666667, 0.7666667, 0.3833…
## $ awake        <dbl> 11.9, 7.0, 9.6, 9.1, 20.0, 9.6, 15.3, 17.0, 13.9, 2…
## $ brainwt      <dbl> NA, 0.01550, NA, 0.00029, 0.42300, NA, NA, NA, 0.07…
## $ bodywt       <dbl> 50.000, 0.480, 1.350, 0.019, 600.000, 3.850, 20.490…
```

```r
head(msleep)
```

```
## # A tibble: 6 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
## 1 Chee… Acin… carni Carn… lc                  12.1      NA        NA    
## 2 Owl … Aotus omni  Prim… <NA>                17         1.8      NA    
## 3 Moun… Aplo… herbi Rode… nt                  14.4       2.4      NA    
## 4 Grea… Blar… omni  Sori… lc                  14.9       2.3       0.133
## 5 Cow   Bos   herbi Arti… domesticated         4         0.7       0.667
## 6 Thre… Brad… herbi Pilo… <NA>                14.4       2.2       0.767
## # … with 3 more variables: awake <dbl>, brainwt <dbl>, bodywt <dbl>
```

# 3

```r
new_dataframe <- subset(msleep, select=c("name", "genus", "bodywt"))
```

```r
new_dataframe <- arrange(new_dataframe, desc(bodywt))
```

```r
print(new_dataframe)
```

```
## # A tibble: 83 x 3
##    name                 genus         bodywt
##    <chr>                <chr>          <dbl>
##  1 African elephant     Loxodonta      6654 
##  2 Asian elephant       Elephas        2547 
##  3 Giraffe              Giraffa         900.
##  4 Pilot whale          Globicephalus   800 
##  5 Cow                  Bos             600 
##  6 Horse                Equus           521 
##  7 Brazilian tapir      Tapirus         208.
##  8 Donkey               Equus           187 
##  9 Bottle-nosed dolphin Tursiops        173.
## 10 Tiger                Panthera        163.
## # … with 73 more rows
```

# 4

```r
small_dataframe <- msleep %>%
  select(sleep_total, bodywt) %>%
  filter(bodywt<=200)
```

```r
print(small_dataframe)
```

```
## # A tibble: 76 x 2
##    sleep_total bodywt
##          <dbl>  <dbl>
##  1        12.1 50    
##  2        17    0.48 
##  3        14.4  1.35 
##  4        14.9  0.019
##  5        14.4  3.85 
##  6         8.7 20.5  
##  7         7    0.045
##  8        10.1 14    
##  9         3   14.8  
## 10         5.3 33.5  
## # … with 66 more rows
```

```r
large_dataframe <- msleep %>%
  select(sleep_total, bodywt) %>%
  filter(bodywt>=200)
```

```r
print(large_dataframe)
```

```
## # A tibble: 7 x 2
##   sleep_total bodywt
##         <dbl>  <dbl>
## 1         4     600 
## 2         3.9  2547 
## 3         2.9   521 
## 4         1.9   900.
## 5         2.7   800 
## 6         3.3  6654 
## 7         4.4   208.
```


# 5
#### The average sleep duration for large mammals is 3.3

```r
mean(large_dataframe$sleep_total)
```

```
## [1] 3.3
```

# 6
#### The average sleep duration for small mammals is 11.09079.

```r
mean(small_dataframe$sleep_total)
```

```
## [1] 11.09079
```

# 7
#### The little brown bat, big brown bat, thick-tailed opposum, giant armadillo, and North American Opposum sleep 18 hours or more.

```r
eighteen_hours <- msleep %>%
   select(name,genus, order, sleep_total) %>%
   filter(sleep_total>=18)
eighteen_hours <- arrange(eighteen_hours,desc(sleep_total))
```

```r
print(eighteen_hours)
```

```
## # A tibble: 5 x 4
##   name                   genus      order           sleep_total
##   <chr>                  <chr>      <chr>                 <dbl>
## 1 Little brown bat       Myotis     Chiroptera             19.9
## 2 Big brown bat          Eptesicus  Chiroptera             19.7
## 3 Thick-tailed opposum   Lutreolina Didelphimorphia        19.4
## 4 Giant armadillo        Priodontes Cingulata              18.1
## 5 North American Opossum Didelphis  Didelphimorphia        18
```

