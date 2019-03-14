---
title: "lab5_hw_SBUCD"
output: 
  html_document: 
    keep_md: yes
---



## Load the tidyverse

```r
library(tidyverse)
library(skimr)
```

## Mammals Life History
Let's revisit the mammal life history data to practice our `ggplot` skills. Some of the tidy steps will be a repeat from the homework, but it is good practice. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*

# 1. Load the data.

```r
lifehistories <- readr::read_csv("~/Desktop/SBUCD/Lab 5/mammal_lifehistories_v2.csv")
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

# 2. Use your preferred function to have a look. Do you notice any problems?

### There are many NAs.

```r
skimr::skim(lifehistories)
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## ── Variable type:character ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     Genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## ── Variable type:numeric ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────
##      variable missing complete    n      mean         sd   p0  p25     p50
##           AFR       0     1440 1440   -408.12     504.97 -999 -999    2.5 
##     gestation       0     1440 1440   -287.25     455.36 -999 -999    1.05
##   litter size       0     1440 1440    -55.63     234.88 -999    1    2.27
##  litters/year       0     1440 1440   -477.14     500.03 -999 -999    0.38
##          mass       0     1440 1440 383576.72 5055162.92 -999   50  403.02
##     max. life       0     1440 1440   -490.26     615.3  -999 -999 -999   
##       newborn       0     1440 1440   6703.15   90912.52 -999 -999    2.65
##     wean mass       0     1440 1440  16048.93   5e+05    -999 -999 -999   
##       weaning       0     1440 1440   -427.17     496.71 -999 -999    0.73
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

# 3. There are NA's. How are you going to deal with them?

### Repalace -999 with "NA"

```r
lifehistories_NA <- 
  lifehistories %>% 
  na_if(-999.00)
lifehistories_NA
```

```
## # A tibble: 1,440 x 13
##    order family Genus species   mass gestation newborn weaning `wean mass`
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>       <dbl>
##  1 Arti… Antil… Anti… americ… 4.54e4      8.13   3246.    3           8900
##  2 Arti… Bovid… Addax nasoma… 1.82e5      9.39   5480     6.5           NA
##  3 Arti… Bovid… Aepy… melamp… 4.15e4      6.35   5093     5.63       15900
##  4 Arti… Bovid… Alce… busela… 1.50e5      7.9   10167.    6.5           NA
##  5 Arti… Bovid… Ammo… clarkei 2.85e4      6.8      NA    NA             NA
##  6 Arti… Bovid… Ammo… lervia  5.55e4      5.08   3810     4             NA
##  7 Arti… Bovid… Anti… marsup… 3.00e4      5.72   3910     4.04          NA
##  8 Arti… Bovid… Anti… cervic… 3.75e4      5.5    3846     2.13          NA
##  9 Arti… Bovid… Bison bison   4.98e5      8.93  20000    10.7       157500
## 10 Arti… Bovid… Bison bonasus 5.00e5      9.14  23000.    6.6           NA
## # … with 1,430 more rows, and 4 more variables: AFR <dbl>, `max.
## #   life` <dbl>, `litter size` <dbl>, `litters/year` <dbl>
```

# 4. Where are the NA's? This is important to keep in mind as you build plots.

### Most of the NA's are in wean mass, max. life, and litters/year.

```r
lifehistories_NA %>%
  purrr::map_df(~ sum(is.na(.))) %>%
  tidyr::gather(variables, num_nas) %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 13 x 2
##    variables    num_nas
##    <chr>          <int>
##  1 wean mass       1039
##  2 max. life        841
##  3 litters/year     689
##  4 weaning          619
##  5 AFR              607
##  6 newborn          595
##  7 gestation        418
##  8 mass              85
##  9 litter size       84
## 10 order              0
## 11 family             0
## 12 Genus              0
## 13 species            0
```

# 5. Some of the variable names will be problematic. Let's rename them here as a final tidy step.


```r
lifehistories_renamed <- lifehistories_NA %>%
    dplyr::rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year`
          )
lifehistories_renamed
```

```
## # A tibble: 1,440 x 13
##    order family genus species   mass gestation newborn weaning wean_mass
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>
##  1 Arti… Antil… Anti… americ… 4.54e4      8.13   3246.    3         8900
##  2 Arti… Bovid… Addax nasoma… 1.82e5      9.39   5480     6.5         NA
##  3 Arti… Bovid… Aepy… melamp… 4.15e4      6.35   5093     5.63     15900
##  4 Arti… Bovid… Alce… busela… 1.50e5      7.9   10167.    6.5         NA
##  5 Arti… Bovid… Ammo… clarkei 2.85e4      6.8      NA    NA           NA
##  6 Arti… Bovid… Ammo… lervia  5.55e4      5.08   3810     4           NA
##  7 Arti… Bovid… Anti… marsup… 3.00e4      5.72   3910     4.04        NA
##  8 Arti… Bovid… Anti… cervic… 3.75e4      5.5    3846     2.13        NA
##  9 Arti… Bovid… Bison bison   4.98e5      8.93  20000    10.7     157500
## 10 Arti… Bovid… Bison bonasus 5.00e5      9.14  23000.    6.6         NA
## # … with 1,430 more rows, and 4 more variables: AFR <dbl>, max_life <dbl>,
## #   litter_size <dbl>, litters_yr <dbl>
```

##`ggplot()`
For the questions below, try to use the aesthetics you have learned to make visually appealing and informative plots. Make sure to include labels for the axes and titles.

```r
options(scipen=999) #cancels the use of scientific notation for the session
```

# 6. What is the relationship between newborn body mass and gestation? Make a scatterplot that shows this relationship. 


```r
ggplot(data=lifehistories_renamed, mapping=aes(x=newborn, y=gestation)) +
  geom_point()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](lab5_hw_SBUCD_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```r
  #geom_smooth(method=lm, se=FALSE)
```

# 7. You should notice that because of the outliers in newborn mass, we need to make some changes. We didn't talk about this in lab, but you can use `scale_x_log10()` as a layer to correct for this issue. This will log transform the y-axis values.


```r
ggplot(data=lifehistories_renamed, mapping=aes(x=newborn, y=gestation)) +
  geom_point() +
  scale_x_log10()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](lab5_hw_SBUCD_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

# 8. Now that you have the basic plot, color the points by taxonomic order.


```r
ggplot(data=lifehistories_renamed, mapping=aes(x=newborn, y=gestation)) +
  geom_point(aes(colour=order)) +
  scale_x_log10()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](lab5_hw_SBUCD_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

# 9. Lastly, make the size of the points proportional to body mass.


```r
ggplot(data=lifehistories_renamed, mapping=aes(x=newborn, y=gestation)) +
  geom_point(aes(colour=order, size=mass)) +
  scale_x_log10()
```

```
## Warning: Removed 691 rows containing missing values (geom_point).
```

![](lab5_hw_SBUCD_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

# 10. Make a plot that shows the range of lifespan by order.


```r
lifehistories_renamed %>%
  ggplot(aes(x=order, y=max_life, fill=order))+
  geom_boxplot() +
  scale_y_log10() +
  labs(title = "Lifespan by Order",
       x = "Order",
       y = "Lifespan") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
```

```
## Warning: Removed 841 rows containing non-finite values (stat_boxplot).
```

![](lab5_hw_SBUCD_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


