---
title: "lab3_hw_SBUCD"
output: 
  html_document: 
    keep_md: yes
---



## Load the libraries

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


```r
fisheries <- readr::read_csv(file = "/Users/sophieborison/Desktop/class_files-master/data/FAO_1950to2012_111914.csv")
```

```
## Warning: Duplicated column names deduplicated: 'Species (ISSCAAP group)'
## => 'Species (ISSCAAP group)_1' [4], 'Species (ASFIS species)' => 'Species
## (ASFIS species)_1' [5], 'Species (ASFIS species)' => 'Species (ASFIS
## species)_2' [6]
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   `Species (ISSCAAP group)` = col_double(),
##   `Fishing area (FAO major fishing area)` = col_double()
## )
```

```
## See spec(...) for full column specifications.
```

# 1
### Do you see any potential problems with the column names? Does the error message now make more sense?  

##### The column names are duplicates. R "deduplicated" them.

# 2
### The `make.names()` command is helpful when there are issues with column names. Notice that although the names are still cumbersome, much of the problemtatic syntax is removed.

##### Make names.

```r
names(fisheries) = make.names(names(fisheries), unique=T)
names(fisheries)
```

```
##  [1] "Country..Country."                    
##  [2] "Species..ASFIS.species."              
##  [3] "Species..ISSCAAP.group."              
##  [4] "Species..ISSCAAP.group._1"            
##  [5] "Species..ASFIS.species._1"            
##  [6] "Species..ASFIS.species._2"            
##  [7] "Fishing.area..FAO.major.fishing.area."
##  [8] "Measure..Measure."                    
##  [9] "X1950"                                
## [10] "X1951"                                
## [11] "X1952"                                
## [12] "X1953"                                
## [13] "X1954"                                
## [14] "X1955"                                
## [15] "X1956"                                
## [16] "X1957"                                
## [17] "X1958"                                
## [18] "X1959"                                
## [19] "X1960"                                
## [20] "X1961"                                
## [21] "X1962"                                
## [22] "X1963"                                
## [23] "X1964"                                
## [24] "X1965"                                
## [25] "X1966"                                
## [26] "X1967"                                
## [27] "X1968"                                
## [28] "X1969"                                
## [29] "X1970"                                
## [30] "X1971"                                
## [31] "X1972"                                
## [32] "X1973"                                
## [33] "X1974"                                
## [34] "X1975"                                
## [35] "X1976"                                
## [36] "X1977"                                
## [37] "X1978"                                
## [38] "X1979"                                
## [39] "X1980"                                
## [40] "X1981"                                
## [41] "X1982"                                
## [42] "X1983"                                
## [43] "X1984"                                
## [44] "X1985"                                
## [45] "X1986"                                
## [46] "X1987"                                
## [47] "X1988"                                
## [48] "X1989"                                
## [49] "X1990"                                
## [50] "X1991"                                
## [51] "X1992"                                
## [52] "X1993"                                
## [53] "X1994"                                
## [54] "X1995"                                
## [55] "X1996"                                
## [56] "X1997"                                
## [57] "X1998"                                
## [58] "X1999"                                
## [59] "X2000"                                
## [60] "X2001"                                
## [61] "X2002"                                
## [62] "X2003"                                
## [63] "X2004"                                
## [64] "X2005"                                
## [65] "X2006"                                
## [66] "X2007"                                
## [67] "X2008"                                
## [68] "X2009"                                
## [69] "X2010"                                
## [70] "X2011"                                
## [71] "X2012"
```

# 3
###  Let's rename the columns. Use `rename()` to adjust the names as follows. Double check to make sure the rename worked correctly. Make sure to replace the old fisheries object with a new one so you can keep the column names.

##### Rename columns using a pipe. 

```r
renamed_fisheries <-
  fisheries %>%
  dplyr::rename(
    country = Country..Country.,
    commname = Species..ASFIS.species.,
    sciname = Species..ASFIS.species._2,
    spcode = Species..ASFIS.species._1,
    spgroup = Species..ISSCAAP.group.,
    spgroupname = Species..ISSCAAP.group._1,
    region = Fishing.area..FAO.major.fishing.area.,
    unit = Measure..Measure.
  )
```

```r
names(renamed_fisheries)
```

```
##  [1] "country"     "commname"    "spgroup"     "spgroupname" "spcode"     
##  [6] "sciname"     "region"      "unit"        "X1950"       "X1951"      
## [11] "X1952"       "X1953"       "X1954"       "X1955"       "X1956"      
## [16] "X1957"       "X1958"       "X1959"       "X1960"       "X1961"      
## [21] "X1962"       "X1963"       "X1964"       "X1965"       "X1966"      
## [26] "X1967"       "X1968"       "X1969"       "X1970"       "X1971"      
## [31] "X1972"       "X1973"       "X1974"       "X1975"       "X1976"      
## [36] "X1977"       "X1978"       "X1979"       "X1980"       "X1981"      
## [41] "X1982"       "X1983"       "X1984"       "X1985"       "X1986"      
## [46] "X1987"       "X1988"       "X1989"       "X1990"       "X1991"      
## [51] "X1992"       "X1993"       "X1994"       "X1995"       "X1996"      
## [56] "X1997"       "X1998"       "X1999"       "X2000"       "X2001"      
## [61] "X2002"       "X2003"       "X2004"       "X2005"       "X2006"      
## [66] "X2007"       "X2008"       "X2009"       "X2010"       "X2011"      
## [71] "X2012"
```

#4
### Are these data tidy? Why or why not, and, how do you know?

##### These data are not yet tidy because instead of "year" being a column name, the years themselves are column names.

```r
print(renamed_fisheries)
```

```
## # A tibble: 17,692 x 71
##    country commname spgroup spgroupname spcode sciname region unit  X1950
##    <chr>   <chr>      <dbl> <chr>       <chr>  <chr>    <dbl> <chr> <chr>
##  1 Albania Angelsh…      38 Sharks, ra… 10903… Squati…     37 Quan… ...  
##  2 Albania Atlanti…      36 Tunas, bon… 17501… Sarda …     37 Quan… ...  
##  3 Albania Barracu…      37 Miscellane… 17710… Sphyra…     37 Quan… ...  
##  4 Albania Blue an…      45 Shrimps, p… 22802… Ariste…     37 Quan… ...  
##  5 Albania Blue wh…      32 Cods, hake… 14804… Microm…     37 Quan… ...  
##  6 Albania Bluefish      37 Miscellane… 17020… Pomato…     37 Quan… ...  
##  7 Albania Bogue         33 Miscellane… 17039… Boops …     37 Quan… ...  
##  8 Albania Caramot…      45 Shrimps, p… 22801… Penaeu…     37 Quan… ...  
##  9 Albania Catshar…      38 Sharks, ra… 10801… Scylio…     37 Quan… ...  
## 10 Albania Common …      57 Squids, cu… 32102… Sepia …     37 Quan… ...  
## # … with 17,682 more rows, and 62 more variables: X1951 <chr>,
## #   X1952 <chr>, X1953 <chr>, X1954 <chr>, X1955 <chr>, X1956 <chr>,
## #   X1957 <chr>, X1958 <chr>, X1959 <chr>, X1960 <chr>, X1961 <chr>,
## #   X1962 <chr>, X1963 <chr>, X1964 <chr>, X1965 <chr>, X1966 <chr>,
## #   X1967 <chr>, X1968 <chr>, X1969 <chr>, X1970 <chr>, X1971 <chr>,
## #   X1972 <chr>, X1973 <chr>, X1974 <chr>, X1975 <chr>, X1976 <chr>,
## #   X1977 <chr>, X1978 <chr>, X1979 <chr>, X1980 <chr>, X1981 <chr>,
## #   X1982 <chr>, X1983 <chr>, X1984 <chr>, X1985 <chr>, X1986 <chr>,
## #   X1987 <chr>, X1988 <chr>, X1989 <chr>, X1990 <chr>, X1991 <chr>,
## #   X1992 <chr>, X1993 <chr>, X1994 <chr>, X1995 <chr>, X1996 <chr>,
## #   X1997 <chr>, X1998 <chr>, X1999 <chr>, X2000 <chr>, X2001 <chr>,
## #   X2002 <chr>, X2003 <chr>, X2004 <chr>, X2005 <chr>, X2006 <chr>,
## #   X2007 <chr>, X2008 <chr>, X2009 <chr>, X2010 <chr>, X2011 <chr>,
## #   X2012 <chr>
```

# 5
###  We need to tidy the data using `gather()`. The code below will not run because it is commented (#) out. I have added a bit of code that will prevent you from needing to type in each year from 1950-2012 but you need to complete the remainder `QQQ` and remove the `#`.

##### Use gather to tidy the data.

```r
fisheries_tidy <- 
  renamed_fisheries %>% 
  gather(num_range('X',1950:2012), key='year', value='catch')
```

# 6
### Use `glimpse()` to look at the categories of the variables. Pay particular attention to `year` and `catch`. What do you notice?

##### There are a lot of missing data in `catch`.

```r
glimpse(fisheries_tidy)
```

```
## Observations: 1,114,596
## Variables: 10
## $ country     <chr> "Albania", "Albania", "Albania", "Albania", "Albania…
## $ commname    <chr> "Angelsharks, sand devils nei", "Atlantic bonito", "…
## $ spgroup     <dbl> 38, 36, 37, 45, 32, 37, 33, 45, 38, 57, 33, 57, 31, …
## $ spgroupname <chr> "Sharks, rays, chimaeras", "Tunas, bonitos, billfish…
## $ spcode      <chr> "10903XXXXX", "1750100101", "17710001XX", "228020310…
## $ sciname     <chr> "Squatinidae", "Sarda sarda", "Sphyraena spp", "Aris…
## $ region      <dbl> 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, …
## $ unit        <chr> "Quantity (tonnes)", "Quantity (tonnes)", "Quantity …
## $ year        <chr> "X1950", "X1950", "X1950", "X1950", "X1950", "X1950"…
## $ catch       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
```

#7
### From question 6 you should see that there are a lot of entries that are missing. In R, these are referred to as NA's but they can be coded in different ways by the scientists in a given study. In order to make the data tidy, we need to deal with them. As a preview to our next lab, run the following code by removing the `#`. It removes the 'X' from the years and changes the `catch` column from a character into a numeric. This forces the blank entries to become NAs. The error "NAs introduced by coercion" indicates their replacement.


```r
fisheries_tidy <- 
  fisheries_tidy %>% 
  mutate(
    year= as.numeric(str_replace(year, 'X', '')), #remove the X from year
    catch= as.numeric(str_replace(catch, c(' F','...','-'), replacement = '')) #replace character strings with NA
    )
```

```
## Warning in evalq(as.numeric(str_replace(catch, c(" F", "...", "-"),
## replacement = "")), : NAs introduced by coercion
```

# 8
### Are the data tidy? Why?  

##### The data are tidy because there is one observation per row, and each column name is a category rather than a variable.

# 9
###  You are a fisheries scientist studying cephalopod catch during 2008-2012. Identify the top five consumers (by country) of cephalopods (don't worry about species for now). Restrict the data frame only to our variables of interest.

##### The top five consumers of cephalopods are India, China, Madagascar, Mozambique, and Taiwan.

```r
fisheries_tidy %>% 
  select(country, sciname, year, catch) %>% 
  filter(sciname=="Cephalopoda") %>%
  filter(year== 2012) %>%
  arrange(desc(catch))
```

```
## # A tibble: 23 x 4
##    country                  sciname      year catch
##    <chr>                    <chr>       <dbl> <dbl>
##  1 India                    Cephalopoda  2012 82456
##  2 China                    Cephalopoda  2012 61821
##  3 India                    Cephalopoda  2012 14388
##  4 Madagascar               Cephalopoda  2012  6206
##  5 Mozambique               Cephalopoda  2012  2150
##  6 Taiwan Province of China Cephalopoda  2012  1050
##  7 Somalia                  Cephalopoda  2012   600
##  8 Mauritania               Cephalopoda  2012   107
##  9 Italy                    Cephalopoda  2012    66
## 10 France                   Cephalopoda  2012    27
## # … with 13 more rows
```

# 10
### Let's be more specific. Who consumes the most `Common cuttlefish`? Store this as a new object `cuttle`.

##### France consumes the most Common cuttlefish.

```r
cuttle <-
  fisheries_tidy %>%
  filter(commname=="Common cuttlefish") %>%
  select(country, commname, catch) %>%
  arrange(desc(catch))
```

```r
print(cuttle)
```

```
## # A tibble: 1,323 x 3
##    country commname          catch
##    <chr>   <chr>             <dbl>
##  1 France  Common cuttlefish 13217
##  2 France  Common cuttlefish 12966
##  3 France  Common cuttlefish  8076
##  4 Tunisia Common cuttlefish  7995
##  5 Tunisia Common cuttlefish  7717
##  6 Greece  Common cuttlefish  7200
##  7 Tunisia Common cuttlefish  7188
##  8 Tunisia Common cuttlefish  6622
##  9 Tunisia Common cuttlefish  6479
## 10 Tunisia Common cuttlefish  6404
## # … with 1,313 more rows
```

