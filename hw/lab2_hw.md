---
title: "Lab 2 Homework"
author: "Isha Kundra"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse

```r
library("tidyverse")
```

```
## -- Attaching packages --------------------------------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0     v purrr   0.3.0
## v tibble  2.0.1     v dplyr   0.7.8
## v tidyr   0.8.2     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.3.0
```

```
## -- Conflicts ------------------------------------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

## Mammals Sleep
For this assignment, we are going to use built-in data on mammal sleep patterns.  

```r
msleep
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
##  1 Chee~ Acin~ carni Carn~ lc                  12.1      NA        NA    
##  2 Owl ~ Aotus omni  Prim~ <NA>                17         1.8      NA    
##  3 Moun~ Aplo~ herbi Rode~ nt                  14.4       2.4      NA    
##  4 Grea~ Blar~ omni  Sori~ lc                  14.9       2.3       0.133
##  5 Cow   Bos   herbi Arti~ domesticated         4         0.7       0.667
##  6 Thre~ Brad~ herbi Pilo~ <NA>                14.4       2.2       0.767
##  7 Nort~ Call~ carni Carn~ vu                   8.7       1.4       0.383
##  8 Vesp~ Calo~ <NA>  Rode~ <NA>                 7        NA        NA    
##  9 Dog   Canis carni Carn~ domesticated        10.1       2.9       0.333
## 10 Roe ~ Capr~ herbi Arti~ lc                   3        NA        NA    
## # ... with 73 more rows, and 3 more variables: awake <dbl>, brainwt <dbl>,
## #   bodywt <dbl>
```

1. From which publication are these data taken from? Don't do an internet search; show the code that you would use to find out in R.
## msleep. This prints the mammal data set.


2. Provide some summary information about the data to get you started; feel free to use the functions that you find most helpful.


```r
summary(msleep)
```

```
##      name              genus               vore          
##  Length:83          Length:83          Length:83         
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##     order           conservation        sleep_total      sleep_rem    
##  Length:83          Length:83          Min.   : 1.90   Min.   :0.100  
##  Class :character   Class :character   1st Qu.: 7.85   1st Qu.:0.900  
##  Mode  :character   Mode  :character   Median :10.10   Median :1.500  
##                                        Mean   :10.43   Mean   :1.875  
##                                        3rd Qu.:13.75   3rd Qu.:2.400  
##                                        Max.   :19.90   Max.   :6.600  
##                                                        NA's   :22     
##   sleep_cycle         awake          brainwt            bodywt        
##  Min.   :0.1167   Min.   : 4.10   Min.   :0.00014   Min.   :   0.005  
##  1st Qu.:0.1833   1st Qu.:10.25   1st Qu.:0.00290   1st Qu.:   0.174  
##  Median :0.3333   Median :13.90   Median :0.01240   Median :   1.670  
##  Mean   :0.4396   Mean   :13.57   Mean   :0.28158   Mean   : 166.136  
##  3rd Qu.:0.5792   3rd Qu.:16.15   3rd Qu.:0.12550   3rd Qu.:  41.750  
##  Max.   :1.5000   Max.   :22.10   Max.   :5.71200   Max.   :6654.000  
##  NA's   :51                       NA's   :27
```


```r
glimpse(msleep)
```

```
## Observations: 83
## Variables: 11
## $ name         <chr> "Cheetah", "Owl monkey", "Mountain beaver", "Grea...
## $ genus        <chr> "Acinonyx", "Aotus", "Aplodontia", "Blarina", "Bo...
## $ vore         <chr> "carni", "omni", "herbi", "omni", "herbi", "herbi...
## $ order        <chr> "Carnivora", "Primates", "Rodentia", "Soricomorph...
## $ conservation <chr> "lc", NA, "nt", "lc", "domesticated", NA, "vu", N...
## $ sleep_total  <dbl> 12.1, 17.0, 14.4, 14.9, 4.0, 14.4, 8.7, 7.0, 10.1...
## $ sleep_rem    <dbl> NA, 1.8, 2.4, 2.3, 0.7, 2.2, 1.4, NA, 2.9, NA, 0....
## $ sleep_cycle  <dbl> NA, NA, NA, 0.1333333, 0.6666667, 0.7666667, 0.38...
## $ awake        <dbl> 11.9, 7.0, 9.6, 9.1, 20.0, 9.6, 15.3, 17.0, 13.9,...
## $ brainwt      <dbl> NA, 0.01550, NA, 0.00029, 0.42300, NA, NA, NA, 0....
## $ bodywt       <dbl> 50.000, 0.480, 1.350, 0.019, 600.000, 3.850, 20.4...
```


```r
colnames(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"       
##  [5] "conservation" "sleep_total"  "sleep_rem"    "sleep_cycle" 
##  [9] "awake"        "brainwt"      "bodywt"
```


3. Make a new data frame focused on body weight, but be sure to indicate the common name and genus of each mammal. Sort the data in descending order by body weight.



```r
mammaldata<- select (msleep, name, genus, bodywt)
mammaldata %>% 
arrange(desc(bodywt))
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
## # ... with 73 more rows
```




4. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. For our study, we are interested in body weight and sleep total Make two new dataframes (large and small) based on these parameters. Sort the data in descending order by body weight.



```r
small <- filter(msleep, bodywt<=1, sleep_total) 
small %>% arrange(desc(bodywt))
```

```
## # A tibble: 36 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
##  1 Afri~ Cric~ omni  Rode~ <NA>                 8.3       2        NA    
##  2 Arct~ Sper~ herbi Rode~ lc                  16.6      NA        NA    
##  3 Tenr~ Tenr~ omni  Afro~ <NA>                15.6       2.3      NA    
##  4 Euro~ Erin~ omni  Erin~ lc                  10.1       3.5       0.283
##  5 Squi~ Saim~ omni  Prim~ <NA>                 9.6       1.4      NA    
##  6 Guin~ Cavis herbi Rode~ domesticated         9.4       0.8       0.217
##  7 Dese~ Para~ <NA>  Erin~ lc                  10.3       2.7      NA    
##  8 Owl ~ Aotus omni  Prim~ <NA>                17         1.8      NA    
##  9 Chin~ Chin~ herbi Rode~ domesticated        12.5       1.5       0.117
## 10 Thic~ Lutr~ carni Dide~ lc                  19.4       6.6      NA    
## # ... with 26 more rows, and 3 more variables: awake <dbl>, brainwt <dbl>,
## #   bodywt <dbl>
```


```r
large <- filter (msleep, bodywt>=200, sleep_total) 
large %>% arrange(desc(bodywt))
```

```
## # A tibble: 7 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
## 1 Afri~ Loxo~ herbi Prob~ vu                   3.3      NA        NA    
## 2 Asia~ Elep~ herbi Prob~ en                   3.9      NA        NA    
## 3 Gira~ Gira~ herbi Arti~ cd                   1.9       0.4      NA    
## 4 Pilo~ Glob~ carni Ceta~ cd                   2.7       0.1      NA    
## 5 Cow   Bos   herbi Arti~ domesticated         4         0.7       0.667
## 6 Horse Equus herbi Peri~ domesticated         2.9       0.6       1    
## 7 Braz~ Tapi~ herbi Peri~ vu                   4.4       1         0.9  
## # ... with 3 more variables: awake <dbl>, brainwt <dbl>, bodywt <dbl>
```


5. Let's try to figure out if large mammals sleep, on average, longer than small mammals. What is the average sleep duration for large mammals as we have defined them?

```r
#mean(large$sleep_total)
```


```r
mean(large$sleep_total)
```

```
## [1] 3.3
```

6. What is the average sleep duration for small mammals as we have defined them?



```r
mean(small$sleep_total)
```

```
## [1] 12.65833
```

7. Which animals sleep at least 18 hours per day? Be sure to show the name, genus, order, and sleep total. Sort by order and sleep total.


```r
msleep %>%
select (name, genus, order, sleep_total)%>% 
filter (sleep_total>=18) %>%
arrange (order, sleep_total)
```

```
## # A tibble: 5 x 4
##   name                   genus      order           sleep_total
##   <chr>                  <chr>      <chr>                 <dbl>
## 1 Big brown bat          Eptesicus  Chiroptera             19.7
## 2 Little brown bat       Myotis     Chiroptera             19.9
## 3 Giant armadillo        Priodontes Cingulata              18.1
## 4 North American Opossum Didelphis  Didelphimorphia        18  
## 5 Thick-tailed opposum   Lutreolina Didelphimorphia        19.4
```



## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
