---
title: "BrewBeers_Codebook"
author: "K. Theobald, Andy Nguyen, Anthony Yeung"
date: "2/23/2019"
output:
  html_document:
    keep_md: true
---

## Codebook for BrewBeers CaseStudy in R Markdown.

The case study makes use of 2 text files. The 2 text files combined, document 2410 craft beers and 558 breweries. The first text file, Beers.csv contains 7 columns (Beer Names, Beer ID, ABV, IBU, Brewery ID, Style of Beer and Ounces). The second text file, Breweries.csv contains 4 columns (Brewery ID, Name of Company, City and State of Brewery). This code provides a table of the 50 states along with the District of Columbia the number of breweries in each state.  This code will merge the 2 text files together by their shared content in the variable Brewery ID.  The merging of the text files (Beer and Breweries) reveal a total of 1005 NA's for IBU and 62 NA's for ABV. The NA's do not hinder our ability to proceed with reasonable conclusions with the exhibiting information. In this study we reveal the median ABV (Alcohol by Volume) and IBU (International Bitterness Unit) for each state. The state with the maximum IBU is Oregon and the state with the maximum ABV is Colorado. We can conclude the ABV minimum is 0.001, 0.05 for the 1st Quartile, 0.056 Median, 0.05977 Mean, 0.06700 for the 3rd Quartile and 0.128 for the Maximum percentage in all 50 states and the District of Columbia. Overall, we can draw a linear relationship between the IBU and the ABV for the breweries in each state and DC. We conclude there is not a 1 to 1 relationship but the majority of the beers contain low ABV and low IBU. We also deduce by the information presented the highest ABV does not mean the highest IBU. In addition, the beer with the highest bitter taste (IBU) does not mean it will contain the strongest alcohol contents (ABV).  
 

### __VARIABLES:__ 

  * Beers :  Data frame used to hold the import of the text file Beers.csv.
  * Breweries : Data frame used to hold the import of the text file Breweries.csv.
  * MedianIBU : Barchart of the Median IBU for each state including DC, with the NA's excluded. Data set pulled from medians data frame. 
  * MedianABV : Barchart of the Median ABV for each state including DC, witht the NA's excluded. Data set pulled from medians data frame.
  * BreweriesPerState : Data frame holding the Number of Breweries per State and the District of Columbia.  
  * medians : Data frame used to hold the median values of ABV and IBU for each state and the District of Columbia pulled from data frame CraftBeers. 
  * CraftBeers : Data frame with the merged data frames of Beers and Breweries by the common column Brewery ID.
  * NA.ColTotal : Object used to store the sum of NA's demonstrated from the data frame CraftBeers. 
  * IBUvsABV : Scatter Plot of the relationship between IBU and ABV in the US and DC. Dataset pulled from Craftbeers data frame. 
  * LinearCorrelation : Linear fit model for the scatter plot of IBU and ABV pulled from the data frame, CraftBeers.
  * R_squared : Object containing the adjusted R (residual standard error) squared from the LinearCorrelation model. 
  * Correlation.ABV_IBU : Object containing the linear correlation coefficient for ABV and IBU (square root of the R_squared). 
  
### __UNITS__

ABV : Percentage of the Alcohol Content by Volume.

IBU : International Beer Unit, bitter taste in beer. 

Ounces : Liquid volume for identified beers.

### **INSTRUCTION LIST**
#### **This code was created on a UNIX OS. Begin with creating 2 data frames holding the Beers.csv file and the other holding Breweries.csv.** 

Beers <- read.csv("Data/Beers.csv")
Breweries <- read.csv("Data/Breweries.csv")

#### **Create a table to demonstrate the total number of breweries per state using the Breweries data frame.**

BreweriesPerState <- as.data.frame(table(Breweries$State))

#### **Tidy data by rename columns in the data frames BreweriesPerState, Beers and Breweries.**

colnames(BreweriesPerState) <- c("State","Number of Breweries")

colnames(Beers)[colnames(Beers)=="Name"] <- "Beer Name"

colnames(Breweries)[colnames(Breweries)=="Name"] <- "Brewery Name"

colnames(Breweries)[colnames(Breweries)=="Brew_ID"] <- "Brewery_id"

#### **Below is a brief display of the table in how it will generate the 2 columns States/DC with the total breweries.**

BreweriesPerState

|    State   |      Number of Breweries   |
|:----------:|:--------------------------:|
|  AK        |            7               |
|  AL        |            3               |
|  AR        |            2               |
|  AZ        |            11              |

#### **Create a Comma Separated  File and call it BreweriesPerState.csv.**
write.csv(BreweriesPerState, file = "BreweriesPerState.csv")
  
  
#### **Merge data frame Beers and Breweries by Brewery_ID and keep all NA's.**

CraftBeers <- merge(x = Beers, y = Breweries, by = "Brewery_id", all = TRUE)
  
  
#### **Show the header and tail for merged data frames, now called Craftbeers.** 

head(CraftBeers, n=6)


|   |Brewery_id |    Beer Name | Beer_ID  | ABV | IBU |   Style | Ounces  |     Brewery Name  |  City    |   State |
|---|:---------:|:------------:|:--------:|:---:|:---:|:--------:|:-------:|:-----------------:|:---------:|:-------:|
| 1 |         1 | Get Together |    2692  | 0.045 |  50|   American IPA   |  16 | NorthGate Brewing | Minneapolis | MN|
| 2 |         1 | Maggie's Leap|    2691  | 0.049 |  26 |Milk / Sweet Stout  |   16 |NorthGate Brewing | Minneapolis | MN| 
| 3 |         1 |   Wall's End |    2690  | 0.048 |  19 | English Brown Ale |    16|NorthGate Brewing  | Minneapolis | MN| 
| 4 |         1 |      Pumpion |    2689  | 0.060 | 38 | Pumpkin Ale   |  16| NorthGate Brewing|  Minneapolis | MN| 
| 5 |         1 |   Stronghold |    2688  | 0.060 | 25 | American Porter  |   16 | NorthGate Brewing | Minneapolis | MN | 
| 6 |         1 |  Parapet ESB |    2687  | 0.056 | 47 | Extra Special / Strong Bitter (ESB) |    16 | NorthGate Brewing | Minneapolis | MN | 

tail(CraftBeers, n=6)


|     |Brewery_id |    Beer Name | Beer_ID  | ABV | IBU |   Style | Ounces  |     Brewery Name  |  City    |   State |
|-----|:---------:|:------------:|:--------:|:---:|:---:|:--------:|:-------:|:-----------------:|:---------:|:-------:|
| 2405|        556|             Pilsner Ukiah |     98  | 0.055 |  NA | German Pilsener  |   12  |       Ukiah Brewing Company | Ukiah  |  CA|
| 2406|        557|  Heinnieweisse Weissebier |      52 | 0.049 |  NA |Hefeweizen  |   12   |    Butternuts Beer and Ale | Garrattsville |    NY |
| 2407|        557|           Snapperhead IPA |      51 | 0.068  | NA   |American IPA  |   12  |     Butternuts Beer and Ale | Garrattsville |    NY |
| 2408|        557|         Moo Thunder Stout |      50 | 0.049 | NA  | Milk / Sweet Stout   |  12   |    Butternuts Beer and Ale   | Garrattsville |    NY |
| 2409|        557|         Porkslap Pale Ale |      49 | 0.043 | NA  |American Pale Ale (APA) |    12    |   Butternuts Beer and Ale | Garrattsville |    NY |
| 2410|        558| Urban Wilderness Pale Ale |      30 | 0.049 | NA  |English Pale Ale  |   12 |  Sleeping Lady Brewing Company| Anchorage  |  AK |

  
#### **Next demonstrate the number of NA's in the data frame CraftBeers.**

NA.ColTotal<-colSums(is.na(CraftBeers))

|  Brewery_id  |    Beer Name |     Beer_ID     |     ABV |         IBU | Style   |    Ounces | Brewery Name |         City|        State| 
|:------------:|:------------:|:---------------:|:-------:|:-----------:|:-------:|:---------:|:------------:|:-----------:|:-----------:|
|           0  |          0   |         0       |    62   |      1005   |     0    |      0    |        0    |        0    |        0    |      
  
#### **Create a table and name the data frame medians. The data frame will contain all 50 states and DC along with the median values for ABV and IBU. Remove all NA's.  We tidy the data with calling the first columns as States (50 + DC). The last step is a subset function complete.case for any missing values. A brief example of the table is available.**

medians <- aggregate(CraftBeers[,c("ABV","IBU")],list(CraftBeers$State),median,na.rm=TRUE)

colnames(medians)[1] <- "State"

medians <- medians[complete.cases(medians),]


|   State |   ABV |  IBU |
|:-------:|:-----:|:----:|
|     AK | 0.0560 | 46.0 |
|     AL | 0.0600 |43.0 |
|     AR|  0.0520 | 39.0 |

#### **Create 2 Bar Plots. First will be the Median value of IBU compared to the 50 States plus DC.  Flip the axes, remove NA's, center the title, label the x axis State (50 states and DC), y axis is labels either IBU or ABV relationship accordingly. The fill color is according to the States column found in the data frame medians. Assign the bar plot for the median IBU to MedianIBU. Same steps followed above but assigning the scatter plot for the median ABV compared to states & DC to MedianABV.**

MedianIBU <- ggplot(data = medians, aes(x = State, y = IBU, fill = State, na.rm = TRUE)) +
                   geom_bar(stat = "identity", position="dodge") + 
                   labs(title = "Median International Bitterness Units of Beers in Each State", x = "State", y = "Median International Bitterness Units (IBU)") +
                   theme(plot.title = element_text(hjust = 0.5)) + coord_flip()

MedianABV <- ggplot(data = medians, aes(x = State, y = ABV, fill = State, na.rm = TRUE)) +
                   geom_bar(stat = "identity", position="dodge") + 
                   labs(title = "Median Alcohol Content By Volume of Beers in Each State", x = "State", y = "Alcohol By Volume (ABV)") +
                   theme(plot.title = element_text(hjust = 0.5)) + coord_flip()


#### **Next, we determine the maximum ABV and IBV for each beer. We sort the CraftBeers data frame according to ABV or IBU respectively and we then report back the State that holds this max value.** 

CraftBeers$State[which.max(CraftBeers$ABV)]

CraftBeers$State[which.max(CraftBeers$IBU)]



#### **Summary statistics  on ABV is pulled using the summary function out of the CraftBeers data frame. The results of this request are demonstrated in the following table.**

summary(CraftBeers$ABV)


|  Min.| 1st Qu.|  Median |   Mean  | 3rd Qu. |   Max. |    NA's |
|:----:|:------:|:-------:|:-------:|:-------:|:------:|:-------:|
|0.00100 | 0.05000 |0.05600 |0.05977 |0.06700| 0.12800  |    62|


  
#### **The following generates a scatter plot to demonstrate the linear relationship between IBU and ABV from the CraftBeers data frame. Name this plot IBUvsABV. The points are colored, burlywood. Generating a smooth black line. The x axis is IBU and will be labeled "International Bitterness Units." The y axis is ABV and will be labeled "Alcohol by Volume (ABV)".  The title to the scattered plot is "Correlation between Bitterness and Alcohol Content for Craft Beers in the United States". This will encompass all 50 states and DC data. Adjust the scale of the plot title with a height adjustment to 0.5.**

IBUvsABV <- ggplot(data=CraftBeers[complete.cases(CraftBeers),],aes(x=IBU,y=ABV)) + 
            geom_point(color = "burlywood") + 
            geom_smooth(method = lm, se = FALSE, color = "black") +
            labs(title = "Correlation between Bitterness and Alcohol Content for Craft Beers in the United States", x = "International Bitterness Units (IBU)", y = "Alcohol by Volume (ABV)") +
            theme(plot.title = element_text(hjust = 0.5))
     
       
#### **Reviewing the linear correlation of the ABV to IBU data out of the data frame CraftBeers. Obtaining the Adjusted R Squared value we take the square root of this correlation and set it equal to the object Correlation.ABV_IBU.**

LinearCorrelation <- lm(ABV ~ IBU, data = CraftBeers)

summary(LinearCorrelation)

R_squared <- 0.4493

Correlation.ABV_IBU <- sqrt(R_squared)

#### **Last, the session info for the current R studio.**

sessionInfo()
