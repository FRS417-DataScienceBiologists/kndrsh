---
title: "Lab 4 Homework"
author: "Isha Kundra"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Life History
Aren't mammals fun? Let's load up some more mammal data. This will be life history data for mammals. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*  

```r
getwd()
```

```
## [1] "C:/Users/Isha/Desktop/class_files-master"
```


```r
life_history <- readr::read_csv("C:/Users/Isha/Desktop/class_files-master/mammal_lifehistories_v2.csv")
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

Rename some of the variables. Notice that I am replacing the old `life_history` data.

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

1. Explore the data using the method that you prefer. Below, I am using a new package called `skimr`. If you want to use this, make sure that it is installed.

```r
#install.packages("skimr")
```


```r
library("skimr")
```


```r
life_history %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## -- Variable type:character -----------------------------------------------------------------------------------
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## -- Variable type:numeric -------------------------------------------------------------------------------------
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
##    15.61     210       <U+2586><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587><U+2581>
##     4.5       21.46    <U+2583><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
##     3.83      14.18    <U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
##     1.15       7.5     <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
##  7009.17       1.5e+08 <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##   147.25    1368       <U+2587><U+2581><U+2581><U+2583><U+2582><U+2581><U+2581><U+2581>
##    98    2250000       <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##    10          1.9e+07 <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##     2         48       <U+2586><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
```

2. Run the code below. Are there any NA's in the data? Does this seem likely?

```r
msleep %>% 
  summarize(number_nas= sum(is.na(life_history)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```

3. Are there any missing data (NA's) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?
# doesn't know there are na's up to -999.0


```r
life_history_na <- 
  life_history %>% 
  na_if("-999") 
life_history_na %>% 
  summarize(number_nas= sum(is.na(life_history)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```


4. Compared to the `msleep` data, we have better representation among taxa. Produce a summary that shows the number of observations by taxonomic order.


```r
t_summary <- life_history %>% 
  group_by(order) %>% 
  summarize(n_total = n())

t_summary
```

```
## # A tibble: 17 x 2
##    order          n_total
##    <chr>            <int>
##  1 Artiodactyla       161
##  2 Carnivora          197
##  3 Cetacea             55
##  4 Dermoptera           2
##  5 Hyracoidea           4
##  6 Insectivora         91
##  7 Lagomorpha          42
##  8 Macroscelidea       10
##  9 Perissodactyla      15
## 10 Pholidota            7
## 11 Primates           156
## 12 Proboscidea          2
## 13 Rodentia           665
## 14 Scandentia           7
## 15 Sirenia              5
## 16 Tubulidentata        1
## 17 Xenarthra           20
```



5. Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.


```r
summary_life <- 
  life_history_na %>%
  mutate(yr_max_life = max_life/12) %>%
  group_by(order) %>%
  summarise(min_lifespan_yrs = min(yr_max_life, na.rm = TRUE),
   max_lifespan_yrs = max(yr_max_life, na.rm = TRUE),
   mean_lifespan_yrs = mean(yr_max_life, na.rm = TRUE),
   sd_lifespan_yrs = sd(yr_max_life, na.rm = TRUE),
   n_total = n()) 
# 
summary_life
```

```
## # A tibble: 17 x 6
##    order min_lifespan_yrs max_lifespan_yrs mean_lifespan_y~ sd_lifespan_yrs
##    <chr>            <dbl>            <dbl>            <dbl>           <dbl>
##  1 Arti~             6.75            61               20.7            7.70 
##  2 Carn~             5               56               21.1            9.42 
##  3 Ceta~            13              114               48.8           27.7  
##  4 Derm~            17.5             17.5             17.5          NaN    
##  5 Hyra~            11               12.2             11.6            0.884
##  6 Inse~             1.17            13                3.85           2.90 
##  7 Lago~             6               18                9.02           3.85 
##  8 Macr~             3                8.75             5.69           2.39 
##  9 Peri~            21               50               35.5           10.2  
## 10 Phol~            20               20               20            NaN    
## 11 Prim~             8.83            60.5             25.7           11.0  
## 12 Prob~            70               80               75              7.07 
## 13 Rode~             1               35                6.99           5.30 
## 14 Scan~             2.67            12.4              8.86           5.38 
## 15 Sire~            12.5             73               43.2           30.3  
## 16 Tubu~            24               24               24            NaN    
## 17 Xena~             9               40               21.3            9.93 
## # ... with 1 more variable: n_total <int>
```


6. Let's look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?


```r
life_history %>% 
  group_by(order) %>% 
  summarize(mean_gestation=mean(gestation, na.rm=TRUE),
            mean_newborn_mass=mean(newborn, na.rm=TRUE),
            mean_wean_mass=mean(wean_mass, na.rm=TRUE),
            total=n()) %>% 
  arrange(desc(mean_gestation))
```

```
## # A tibble: 17 x 5
##    order          mean_gestation mean_newborn_mass mean_wean_mass total
##    <chr>                   <dbl>             <dbl>          <dbl> <int>
##  1 Proboscidea             21.3            99523.        299500.      2
##  2 Perissodactyla          13.0            21412.         75639.     15
##  3 Hyracoidea               7.4              -76.5         -624.      4
##  4 Tubulidentata            7.08            1734           6250       1
##  5 Dermoptera               2.75            -482.          -999       2
##  6 Artiodactyla           -61.5             5125.          7725.    161
##  7 Carnivora             -124.              2262.          7607.    197
##  8 Cetacea               -135.            149143.        347883.     55
##  9 Xenarthra             -196.              -342.          -928.     20
## 10 Lagomorpha            -237.              -345.          -428.     42
## 11 Primates              -259.               -83.8          -40.9   156
## 12 Macroscelidea         -298.              -385.          -778.     10
## 13 Rodentia              -391.              -484.          -673.    665
## 14 Sirenia               -393.             13327.         12701.      5
## 15 Scandentia            -427.              -276.          -684.      7
## 16 Insectivora           -504.              -502.          -727.     91
## 17 Pholidota             -569.               -88.0          718.      7
```



## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
