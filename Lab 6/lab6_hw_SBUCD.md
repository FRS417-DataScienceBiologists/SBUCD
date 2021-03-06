---
title: "lab6_hw_SBUCD"
output: 
  html_document: 
    keep_md: yes
---



# Load the libraries

```r
library(tidyverse)
library(skimr)
library("RColorBrewer")
```

# Install gapminder

```r
#install.packages("gapminder")
```


```r
library("gapminder")
```

# Load the data into a new object called gapminder

```r
gapminder <- 
  gapminder::gapminder
```

# 1
### Explore the data using the various function you have learned. Is it tidy, are there any NA's, what are its dimensions, what are the column names, etc.


```r
gapminder %>%
  purrr::map_df(~ sum(is.na(.))) %>%
  tidyr::gather(variables, num_nas) %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 6 x 2
##   variables num_nas
##   <chr>       <int>
## 1 country         0
## 2 continent       0
## 3 year            0
## 4 lifeExp         0
## 5 pop             0
## 6 gdpPercap       0
```


```r
names(gapminder)
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```


```r
glimpse(gapminder)
```

```
## Observations: 1,704
## Variables: 6
## $ country   <fct> Afghanistan, Afghanistan, Afghanistan, Afghanistan, Af…
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, …
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, …
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854…
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 148803…
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.…
```

# 2
### We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer on average. Make a quick plot below to visualize this relationship.


```r
gapminder %>%
  ggplot(aes(x=gdpPercap, y=lifeExp)) +
  geom_point()
```

![](lab6_hw_SBUCD_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

# 3
### There is extreme disparity in per capita GDP. Rescale the x axis to make this easier to interpret. How would you characterize the relationship?

### This is a positive correlation.

```r
gapminder %>%
  ggplot(aes(x=gdpPercap, y=lifeExp)) +
  geom_point() +
  scale_x_log10() +
  geom_smooth(method=lm, se=FALSE)
```

![](lab6_hw_SBUCD_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

# 4
### This should look pretty dense to you with significant overplotting. Try using a faceting approach to break this relationship down by year.


```r
gapminder %>%
  ggplot(aes(x=gdpPercap, y=lifeExp)) +
  geom_point() +
  scale_x_log10() +
  geom_smooth(method=lm, se=FALSE) +
  facet_wrap(~year)
```

![](lab6_hw_SBUCD_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

# 5
### Simplify the comparison by comparing only 1952 and 2007. Can you come to any conclusions?


```r
gapminder %>%
  filter(year == 1952 | year == 2007) %>%
  ggplot(aes(x=gdpPercap, y=lifeExp)) +
  geom_point() +
  scale_x_log10() +
  geom_smooth(method=lm, se=FALSE) +
  facet_wrap(~year)
```

![](lab6_hw_SBUCD_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

# 6
### Let's stick with the 1952 and 2007 comparison but make some aesthetic adjustments. First try to color by continent and adjust the size of the points by population. Add `+ scale_size(range = c(0.1, 10), guide = "none")` as a layer to clean things up a bit.


```r
gapminder %>%
  filter(year == 1952 | year == 2007) %>%
  ggplot(aes(x=gdpPercap, y=lifeExp, color=continent, size=pop)) +
  scale_size(range = c(0.1, 10), guide = "none") +
  geom_point() +
  scale_x_log10() +
  geom_smooth(method=lm, se=FALSE) +
  facet_wrap(~year)
```

![](lab6_hw_SBUCD_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

# 7
### Although we did not introduce them in lab, ggplot has a number of built-in themes that make things easier. I like the light theme for these data, but you can see lots of options. Apply one of these to your plot above.


```r
?theme_light
```


```r
gapminder %>%
  filter(year == 1952 | year == 2007) %>%
  ggplot(aes(x=gdpPercap, y=lifeExp, color=continent, size=pop)) +
  scale_size(range = c(0.1, 10), guide = "none") +
  geom_point() +
  scale_x_log10() +
  geom_smooth(method=lm, se=FALSE) +
  facet_wrap(~year) +
  theme_light()
```

![](lab6_hw_SBUCD_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

# 8
### What is the population for all countries on the Asian continent in 2007? Show this as a barplot.


```r
gapminder %>%
  filter(year == 2007, continent=="Asia") %>%
  ggplot(aes(x=reorder(country, pop), y=pop)) +
  geom_bar(stat="identity") +
  theme(axis.text.x = element_text(angle=90))
```

![](lab6_hw_SBUCD_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

#9
### You should see that China's population is the largest with India a close second. Let's focus on China only. Make a plot that shows how population has changed over the years.


```r
gapminder %>%
  filter(country=="China") %>%
  ggplot(aes(x=year, y=pop)) +
  geom_point()
```

![](lab6_hw_SBUCD_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

# 10
### Let's compare China and India. Make a barplot that shows population growth by year using `position=dodge`. Apply a custom color theme using RColorBrewer.


```r
gapminder %>%
  filter(country=="China" | country == "India") %>%
  ggplot(aes(x=year, y=pop, group=country, fill=country)) +
  geom_bar(stat="identity", position="dodge") +
  scale_fill_brewer(palette = "Accent")
```

![](lab6_hw_SBUCD_files/figure-html/unnamed-chunk-17-1.png)<!-- -->
