---
title: "BrewBeers"
author: "Andy Nguyen, K. Theobald and Anthony Yeung"
date: "2/14/2019"
output: 
  html_document:
    keep_md: TRUE
---




## The analysis of 2410 different craft beers from 558 breweries throughout the United States.


```r
# Functions to import the data into R. The argument path files provided are relative.
Beers <- read.csv("Data/Beers.csv")
Breweries <- read.csv("Data/Breweries.csv")
```

### __Question 1__: How many breweries are in each state? (Include DC)
  * These are the following breweries per state including the District of Columbia: Alaska (AK) has 7, Alabama (AL) has 3, Arkansas (AR) has 2, Arizona (AZ) has 11, California (CA) has 39, Colorado (CO) has 47, Connecticut (CT) has 8, District of Columbia (DC) has 1, Delaware (DE) has 2, Florida (FL) has 15, Georgia (GA) has 7, Hawaii (HI) has 4, Iowa (IA) has 5, Indaho (ID) has 5, Illinois (IL) has 18, Indiana (IN) has 22, Kansas (KS) has 3, Kentucky (KY) has 4, Louisiana (LA) has 5, Massachusetts (MA) has 23, Maryland (MD) has 7, Maine (ME) has 9, Michigan (MI) has 32, Minnesota (MN) has 12, Missouri (MO) has 9, Mississippi (MS) has 2, Montana (MT) has 9, North Carolina (NC) has 19, North Dakota (ND) has 1, Nebraska (NE) has 5, New Hampshire (NH) has 3, New Jersey (NJ) has 3, New Mexico (NM) has 4, Nevada (NV) has 2, New York (NY) has 16, Ohio (OH) has 15, Oklahoma (OK) has 6, Oregon (OR) has 29, Pennsylvania (PA) has 25, Rhode Island (RI) has 5, South Carolina (SC) has 4, South Dakota (SD) has 1, Tennessee (TN) has 3, Texas (TX) has 28, Utah (UT) has 4, Virginia (VA) has 16, Vermont (VT) has 10, Washington (WA) has 23, Wisconsin (WI) has 20, West Virginia (WV) has 1, and Wyoming (WY) has 4.

```r
# table() function cross-classifies factors of the State variable in the Breweries dataset to output table of number of breweries in each state
## as.data.frame() function coerces the table into a dataframe type in R
BreweriesPerState <- as.data.frame(table(Breweries$State))

# colnames() function creates human-readable variable names for the BreweriesPerState dataframe
colnames(BreweriesPerState) <- c("State","Number of Breweries")
BreweriesPerState
```

```
##    State Number of Breweries
## 1     AK                   7
## 2     AL                   3
## 3     AR                   2
## 4     AZ                  11
## 5     CA                  39
## 6     CO                  47
## 7     CT                   8
## 8     DC                   1
## 9     DE                   2
## 10    FL                  15
## 11    GA                   7
## 12    HI                   4
## 13    IA                   5
## 14    ID                   5
## 15    IL                  18
## 16    IN                  22
## 17    KS                   3
## 18    KY                   4
## 19    LA                   5
## 20    MA                  23
## 21    MD                   7
## 22    ME                   9
## 23    MI                  32
## 24    MN                  12
## 25    MO                   9
## 26    MS                   2
## 27    MT                   9
## 28    NC                  19
## 29    ND                   1
## 30    NE                   5
## 31    NH                   3
## 32    NJ                   3
## 33    NM                   4
## 34    NV                   2
## 35    NY                  16
## 36    OH                  15
## 37    OK                   6
## 38    OR                  29
## 39    PA                  25
## 40    RI                   5
## 41    SC                   4
## 42    SD                   1
## 43    TN                   3
## 44    TX                  28
## 45    UT                   4
## 46    VA                  16
## 47    VT                  10
## 48    WA                  23
## 49    WI                  20
## 50    WV                   1
## 51    WY                   4
```

```r
# write.csv() creates a csv file from the BreweriesPerState dataframe
write.csv(BreweriesPerState, file = "BreweriesPerState.csv")
```

### __Question 2__: Merge beer data with the breweries data. Print the first 6 observations and the last 6 observations to check the merged file.

```r
# colnames() function changes the name of the column at the designated column index
## First and second lines are executed so that the column label "Name" will not be confused as Beer or Brewery name once the two data sets are merged
## The third line is executed so that the two data sets can be merged by the same variable name "Brewery_id"
colnames(Beers)[colnames(Beers)=="Name"] <- "Beer Name"
colnames(Breweries)[colnames(Breweries)=="Name"] <- "Brewery Name"
colnames(Breweries)[colnames(Breweries)=="Brew_ID"] <- "Brewery_id"

# CraftBeers is the object created from the merge() function of the Beers and Breweries data on the variable "Brewery_id"
CraftBeers <- merge(x = Beers, y = Breweries, by = "Brewery_id", all = TRUE)

# The head() function displays the first 6 observations and the tail() function displays the last 6 observations of the merged file
head(CraftBeers,n=6)
```

```
##   Brewery_id     Beer Name Beer_ID   ABV IBU
## 1          1  Get Together    2692 0.045  50
## 2          1 Maggie's Leap    2691 0.049  26
## 3          1    Wall's End    2690 0.048  19
## 4          1       Pumpion    2689 0.060  38
## 5          1    Stronghold    2688 0.060  25
## 6          1   Parapet ESB    2687 0.056  47
##                                 Style Ounces       Brewery Name
## 1                        American IPA     16 NorthGate Brewing 
## 2                  Milk / Sweet Stout     16 NorthGate Brewing 
## 3                   English Brown Ale     16 NorthGate Brewing 
## 4                         Pumpkin Ale     16 NorthGate Brewing 
## 5                     American Porter     16 NorthGate Brewing 
## 6 Extra Special / Strong Bitter (ESB)     16 NorthGate Brewing 
##          City State
## 1 Minneapolis    MN
## 2 Minneapolis    MN
## 3 Minneapolis    MN
## 4 Minneapolis    MN
## 5 Minneapolis    MN
## 6 Minneapolis    MN
```

```r
tail(CraftBeers,n=6)
```

```
##      Brewery_id                 Beer Name Beer_ID   ABV IBU
## 2405        556             Pilsner Ukiah      98 0.055  NA
## 2406        557  Heinnieweisse Weissebier      52 0.049  NA
## 2407        557           Snapperhead IPA      51 0.068  NA
## 2408        557         Moo Thunder Stout      50 0.049  NA
## 2409        557         Porkslap Pale Ale      49 0.043  NA
## 2410        558 Urban Wilderness Pale Ale      30 0.049  NA
##                        Style Ounces                  Brewery Name
## 2405         German Pilsener     12         Ukiah Brewing Company
## 2406              Hefeweizen     12       Butternuts Beer and Ale
## 2407            American IPA     12       Butternuts Beer and Ale
## 2408      Milk / Sweet Stout     12       Butternuts Beer and Ale
## 2409 American Pale Ale (APA)     12       Butternuts Beer and Ale
## 2410        English Pale Ale     12 Sleeping Lady Brewing Company
##               City State
## 2405         Ukiah    CA
## 2406 Garrattsville    NY
## 2407 Garrattsville    NY
## 2408 Garrattsville    NY
## 2409 Garrattsville    NY
## 2410     Anchorage    AK
```


### __Question 3__: Report the number of NA's in each column.
  * Conclusion: There are 1005 NA values in the IBU column and 62 NA values in the ABV column for a total of 1067 NA values.

```r
# N.ColTotal is an object that stores the sum of NA values for each column in the CraftBeers dataset
NA.ColTotal<-colSums(is.na(CraftBeers))
NA.ColTotal
```

```
##   Brewery_id    Beer Name      Beer_ID          ABV          IBU 
##            0            0            0           62         1005 
##        Style       Ounces Brewery Name         City        State 
##            0            0            0            0            0
```
  
  
### __Question 4__: Compute the median alcohol content and international bitterness unit for each state. Plot a bar chart to compare.
  * The median alchol content and international bitterness unit for each state is shown in the table below.

```r
# medians is a data frame that stores the median values of alcohol content (ABV) and international bitterness unit (IBU) for beers in each state
## aggregate() function subsets the two variables "ABV" and "IBU" in CraftBeers by State and computes the median summary statistic for each while excluding the NA values
## colnames() function appropriately renames the subsetting factor (States) for the variables "ABV" and "IBU" from the CraftBeers data
medians <- aggregate(CraftBeers[,c("ABV","IBU")],list(CraftBeers$State),median,na.rm=TRUE)
colnames(medians)[1] <- "State"

# complete.cases() function will subset the rows in the medians data that contain no missing values (NAs)
medians <- medians[complete.cases(medians),]
medians
```

```
##    State    ABV  IBU
## 1     AK 0.0560 46.0
## 2     AL 0.0600 43.0
## 3     AR 0.0520 39.0
## 4     AZ 0.0550 20.5
## 5     CA 0.0580 42.0
## 6     CO 0.0605 40.0
## 7     CT 0.0600 29.0
## 8     DC 0.0625 47.5
## 9     DE 0.0550 52.0
## 10    FL 0.0570 55.0
## 11    GA 0.0550 55.0
## 12    HI 0.0540 22.5
## 13    IA 0.0555 26.0
## 14    ID 0.0565 39.0
## 15    IL 0.0580 30.0
## 16    IN 0.0580 33.0
## 17    KS 0.0500 20.0
## 18    KY 0.0625 31.5
## 19    LA 0.0520 31.5
## 20    MA 0.0540 35.0
## 21    MD 0.0580 29.0
## 22    ME 0.0510 61.0
## 23    MI 0.0620 35.0
## 24    MN 0.0560 44.5
## 25    MO 0.0520 24.0
## 26    MS 0.0580 45.0
## 27    MT 0.0550 40.0
## 28    NC 0.0570 33.5
## 29    ND 0.0500 32.0
## 30    NE 0.0560 35.0
## 31    NH 0.0550 48.5
## 32    NJ 0.0460 34.5
## 33    NM 0.0620 51.0
## 34    NV 0.0600 41.0
## 35    NY 0.0550 47.0
## 36    OH 0.0580 40.0
## 37    OK 0.0600 35.0
## 38    OR 0.0560 40.0
## 39    PA 0.0570 30.0
## 40    RI 0.0550 24.0
## 41    SC 0.0550 30.0
## 43    TN 0.0570 37.0
## 44    TX 0.0550 33.0
## 45    UT 0.0400 34.0
## 46    VA 0.0565 42.0
## 47    VT 0.0550 30.0
## 48    WA 0.0555 38.0
## 49    WI 0.0520 19.0
## 50    WV 0.0620 57.5
## 51    WY 0.0500 21.0
```

```r
library(ggplot2)
MedianIBU <- ggplot(data = medians, aes(x = State, y = IBU, fill = State, na.rm = TRUE)) +
                   geom_bar(stat = "identity", position="dodge") + 
                   labs(title = "Median International Bitterness Units of Beers in Each State", x = "State", y = "Median International Bitterness Units (IBU)") +
                   theme(plot.title = element_text(hjust = 0.5)) + coord_flip()
MedianABV <- ggplot(data = medians, aes(x = State, y = ABV, fill = State, na.rm = TRUE)) +
                   geom_bar(stat = "identity", position="dodge") + 
                   labs(title = "Median Alcohol Content By Volume of Beers in Each State", x = "State", y = "Alcohol By Volume (ABV)") +
                   theme(plot.title = element_text(hjust = 0.5)) + coord_flip()
MedianIBU
```

![](BrewBeers_files/figure-html/Medians-1.png)<!-- -->

```r
MedianABV
```

![](BrewBeers_files/figure-html/Medians-2.png)<!-- -->

### __Question 5__: Which state has the maximum alcoholic (ABV) beer? Which state has the most bitter (IBU) beer?
  * Conclusion: The state with the maximum alcoholic (ABV) beer is Colorado (CO).The state with the most bitter (IBU) beer is Oregon (OR).

```r
# which.max() function returns the index of the maximum value of the specified column in the CraftBeers dataset
## the result of the which.max() function placed inside the extraction operators will return the observation (State) that had the max value
CraftBeers$State[which.max(CraftBeers$ABV)]
```

```
## [1]  CO
## 51 Levels:  AK  AL  AR  AZ  CA  CO  CT  DC  DE  FL  GA  HI  IA  ID ...  WY
```

```r
CraftBeers$State[which.max(CraftBeers$IBU)]
```

```
## [1]  OR
## 51 Levels:  AK  AL  AR  AZ  CA  CO  CT  DC  DE  FL  GA  HI  IA  ID ...  WY
```


### __Question 6__: Summary statistics for the ABV variable.
  * Minimum ABV is 0.001, 1st Quartile 0.05, Median 0.056, Mean 0.05977, 3rd Quartile 0.067 and Maximumm ABV is 0.128. The total NA's in the ABV data frame CraftBeers are 62.

```r
# summary() function returns the summaries of the various model fitting functions for the ABV variable (minimum, first quartile, median, mean, third quartile, maximum, # of NAs) 
summary(CraftBeers$ABV)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
## 0.00100 0.05000 0.05600 0.05977 0.06700 0.12800      62
```


### __Question 7__: Is there an apparent relationship between the bitterness of the beer and its alcoholic content? Draw a scatter plot.
  * Conclusion: There is a linear relationship between ABV and IBU. The linear correlation coefficient between ABV and IBU is 0.67, indicating a strong relationship in which high values of IBU are generally correlated with ABV. However, this is not a direct correlation. The data demonstrates there are more beers containing both lower Alcohol by Volume (ABV) and with a less bitter taste (IBU) within the US. The strongest bitter taste (IBU) with over 125 IBU's does not contain the highest alcohol content (ABV). In fact, the highest Alcohol by Volume (ABV) with a near .125 ABV contains a moderate bitterness taste (IBU) with a little over 75 IBU's.

```r
# complete.cases() function will subset the rows so that they contain no missing values (NAs)
## ggplot() function creates a scatter plot of IBU and ABV using geom_smooth() function to create a linear regression line
IBUvsABV <- ggplot(data=CraftBeers[complete.cases(CraftBeers),],aes(x=IBU,y=ABV)) + 
            geom_point(color = "burlywood") + 
            geom_smooth(method = lm, se = FALSE, color = "black") +
            labs(title = "Correlation between Bitterness and Alcohol Content for Craft Beers in the United States", x = "International Bitterness Units (IBU)", y = "Alcohol by Volume (ABV)") +
            theme(plot.title = element_text(hjust = 0.5))
IBUvsABV
```

![](BrewBeers_files/figure-html/Correlation-1.png)<!-- -->

```r
# lm() function fits the ABV and IBU variables of the CraftBeers dataset with a linear model using ABV as the response variable and IBU as the explanatory variable
# summary() function produces the result summaries of the linear model fit
# the square root of the R^2 value provides the correlation coefficient for the linear fit model of ABV and IBU
LinearCorrelation <- lm(ABV ~ IBU, data = CraftBeers)
summary(LinearCorrelation)
```

```
## 
## Call:
## lm(formula = ABV ~ IBU, data = CraftBeers)
## 
## Residuals:
##       Min        1Q    Median        3Q       Max 
## -0.033288 -0.005946 -0.001595  0.004022  0.052006 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 4.493e-02  5.177e-04   86.79   <2e-16 ***
## IBU         3.508e-04  1.036e-05   33.86   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.01007 on 1403 degrees of freedom
##   (1005 observations deleted due to missingness)
## Multiple R-squared:  0.4497,	Adjusted R-squared:  0.4493 
## F-statistic:  1147 on 1 and 1403 DF,  p-value: < 2.2e-16
```

```r
R_squared <- 0.4493
Correlation.ABV_IBU <- sqrt(R_squared)
Correlation.ABV_IBU
```

```
## [1] 0.6702984
```


### __Session Info.__


```r
sessionInfo()
```

```
## R version 3.5.1 (2018-07-02)
## Platform: x86_64-apple-darwin15.6.0 (64-bit)
## Running under: macOS Sierra 10.12.6
## 
## Matrix products: default
## BLAS: /Library/Frameworks/R.framework/Versions/3.5/Resources/lib/libRblas.0.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/3.5/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] ggplot2_3.1.0
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_1.0.0       bindr_0.1.1      knitr_1.21       magrittr_1.5    
##  [5] tidyselect_0.2.5 munsell_0.5.0    colorspace_1.3-2 R6_2.3.0        
##  [9] rlang_0.3.1      stringr_1.3.1    plyr_1.8.4       dplyr_0.7.8     
## [13] tools_3.5.1      grid_3.5.1       gtable_0.2.0     xfun_0.4        
## [17] withr_2.1.2      htmltools_0.3.6  assertthat_0.2.0 yaml_2.2.0      
## [21] lazyeval_0.2.1   digest_0.6.18    tibble_1.4.2     crayon_1.3.4    
## [25] bindrcpp_0.2.2   purrr_0.3.0      glue_1.3.0       evaluate_0.12   
## [29] rmarkdown_1.11   labeling_0.3     stringi_1.2.4    compiler_3.5.1  
## [33] pillar_1.3.1     scales_1.0.0     pkgconfig_2.0.2
```

