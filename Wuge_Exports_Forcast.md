# Chulwalar_WugeExports
Marvin Scott  
July 25, 2016  

###**Introduction** 
Chulwalar is a an island located near the Pacific and is well known for producing unique flowers during the Winter Season. Overall there are around four types, IE ***Efak***, ***Wuge***, ***BlueEtel*** and ***RedEtel***, this document will focus on  the exports related the ***WUGE Flower*** and determine the best fit ***MODEL*** that predicts and forecast values beyond 2014. This analysis would help the government of Chulwalar compare their set plan/estimates for years ahead.

###**Packages And Datasets Required**

#####Below CSV files can be found under the ChulwalarCaseStudy/Data/ folder. Below steps to import datasets into R.

**ImportedAsIsDataChulwalar** File includes the "AS IS" data.

**ImportedPlanDataChulwalar** File includes the "Plan" data. 

**ImportedIndicatorsChulwalar** File includes the "indicators" data that may possibly influence exports.


```r
require(fpp)
```

```
## Loading required package: fpp
```

```
## Warning: package 'fpp' was built under R version 3.3.1
```

```
## Loading required package: forecast
```

```
## Warning: package 'forecast' was built under R version 3.3.1
```

```
## Loading required package: zoo
```

```
## Warning: package 'zoo' was built under R version 3.3.1
```

```
## 
## Attaching package: 'zoo'
```

```
## The following objects are masked from 'package:base':
## 
##     as.Date, as.Date.numeric
```

```
## Loading required package: timeDate
```

```
## This is forecast 7.1
```

```
## Loading required package: fma
```

```
## Warning: package 'fma' was built under R version 3.3.1
```

```
## Loading required package: tseries
```

```
## Warning: package 'tseries' was built under R version 3.3.1
```

```
## Loading required package: expsmooth
```

```
## Warning: package 'expsmooth' was built under R version 3.3.1
```

```
## Loading required package: lmtest
```

```
## Warning: package 'lmtest' was built under R version 3.3.1
```

```r
require(forecast)

ImportedAsIsData <- read.csv(file.choose(), header = F, sep=";", fill = T)#select ImportedAsIsDataChulwalar.csv 
ImportedPlanData <- read.csv(file.choose(), header = F, sep=";", fill = T)#select ImportedPlanDataChulwalar.csv 
ImportedIndicators <- read.csv(file.choose(), header = F, sep=";", fill = T)#select ImportedIndicatorsChulwalar.csv 
```

###**Steps for Clean Data**
**AS IS Dataset for Chulwalar**

* Monthly Total Exports from 2008-2013. 
* Monthly Total WUGE Exports from 2008-2013.
* Yearly Total Exports from 2008-2013.
* Monthly Total Exports from 2014. 

```r
##Monthly "Total As is" from 2008-2013
TotalAsIsVector <- c(ImportedAsIsData [2:13,2],ImportedAsIsData [2:13,3],ImportedAsIsData [2:13,4],ImportedAsIsData [2:13,5],ImportedAsIsData [2:13,6],ImportedAsIsData [2:13,7])
##Monthly "WugeAsIs" from 2008-2013
WugeAsIsVector <- c(ImportedAsIsData [30:41,2],ImportedAsIsData [30:41,3],ImportedAsIsData [30:41,4],ImportedAsIsData [30:41,5],ImportedAsIsData [30:41,6],ImportedAsIsData [30:41,7])
## "YearAsIs" from 2008-2013
YearAsIsVector <- c(ImportedAsIsData [86,2],ImportedAsIsData [86,3],ImportedAsIsData [86,4],ImportedAsIsData [86,5],ImportedAsIsData [86,6],ImportedAsIsData [86,7])
##Monthly "Total As is" for 2014
TotalAsIsVector_2014 <- c(ImportedAsIsData[2:13,8])

##Convert "AS IS" data to TIME Series Format
TotalAsIs<- ts(TotalAsIsVector , start=c(2008,1), end=c(2013,12), frequency=12)
WugeAsIs <- ts(WugeAsIsVector, start=c(2008,1), end=c(2013,12), frequency=12)
YearAsIs <- ts(YearAsIsVector, start=c(2008,1), end=c(2013,12), frequency=12)
TotalAsIs_2014 <- ts(TotalAsIsVector_2014, start=c(2014,1), end=c(2014,12), frequency=12)
```

**Plan Dataset for Chulwalar**

* Monthly Total Plan Exports from 2008-2013. 
* Monthly Total Plan WUGE Exports from 2008-2013.
* Yearly Total Plan Exports from 2008-2013.
* Monthly Total Plan Exports from 2014. 

```r
##Monthly "Total Plan" from 2008-2013
PlanVector <- c(ImportedPlanData[2:13,2],ImportedPlanData[2:13,3],ImportedPlanData[2:13,4],ImportedPlanData[2:13,5],ImportedPlanData[2:13,6],ImportedPlanData[2:13,7])
##Monthly "Wuge Plan" from 2008-2013
WugePlanVector <- c(ImportedPlanData[30:41,2],ImportedPlanData[30:41,3],ImportedPlanData[30:41,4],ImportedPlanData[30:41,5],ImportedPlanData[30:41,6],ImportedPlanData[30:41,7])
## "Year plan" from 2008-2013
YearPlanVector <- c(ImportedPlanData[86,2],ImportedPlanData[86,3],ImportedPlanData[86,4],ImportedPlanData[86,5],ImportedPlanData[86,6],ImportedPlanData[86,7])
##Monthly "Total Plan" for 2014
PlanVector_2014 <- c(ImportedPlanData[2:13,8])

##Convert "PLAN" data to TIME Series Format
TotalPlan <- ts(PlanVector , start=c(2008,1), end=c(2013,12), frequency=12)
WugePlan <- ts(WugePlanVector, start=c(2008,1), end=c(2013,12), frequency=12)
YearPlan <- ts(YearPlanVector, start=c(2008,1), end=c(2013,12), frequency=12)
TotalPlan_2014 <- ts(PlanVector_2014, start=c(2014,1), end=c(2014,12), frequency=12)
```

**Indicator Dataset for Chulwalar**

* Monthly Change in export prices from 2008-2013 (CEPI)
* Monthly Satisfaction Index(gov) from 2008-2013
* Monthly AverageTemperatures from 2008-2013
* Monthly Births from 2008-2013
* Monthly Satisfaction Index(independent) from 2008-2013 
* Total Exports from Urbano Yearly from 2008-2013
* Monthly GlobalisationPartyMembers from 2008-2013 
* Monthly Average Export Price from 2008-2013
* Monthly Etel Production price index 2008-2013
* Monthly Chulwalar Index from 2008-2013
* Monthly Inflation from 2008-2013
* Spending for Chulwalar days Yearly from 2008-2013
* Monthly Chulwalar days from 2008-2013
* Monthly Influence of Chulwalar days from 2008-2013

```r
CEPIVector <- c(ImportedIndicators[2:13,2],ImportedIndicators[2:13,3],ImportedIndicators[2:13,4],ImportedIndicators[2:13,5],ImportedIndicators[2:13,6],ImportedIndicators[2:13,7])
##Convert "Indicator" data to TIME Series Format
CEPI <- ts(CEPIVector , start=c(2008,1), end=c(2013,12), frequency=12)

SIGovVector <- c(ImportedIndicators[16:27,2],ImportedIndicators[16:27,3],ImportedIndicators[16:27,4],ImportedIndicators[16:27,5],ImportedIndicators[16:27,6],ImportedIndicators[16:27,7])
##Convert "Indicator" data to TIME Series Format
SIGov <- ts(SIGovVector , start=c(2008,1), end=c(2013,12), frequency=12)


TemperatureVector <- c(ImportedIndicators[30:41,2],ImportedIndicators[30:41,3],ImportedIndicators[30:41,4],ImportedIndicators[30:41,5],ImportedIndicators[30:41,6],ImportedIndicators[30:41,7])
##Convert "Indicator" data to TIME Series Format
Temperature <- ts(TemperatureVector, start=c(2008,1), end=c(2013,12), frequency=12)

BirthsVector <- c(ImportedIndicators[44:55,2],ImportedIndicators[44:55,3],ImportedIndicators[44:55,4],ImportedIndicators[44:55,5],ImportedIndicators[44:55,6],ImportedIndicators[44:55,7])
Births <- ts(BirthsVector, start=c(2008,1), end=c(2013,12), frequency=12) ##Convert "Indicator" data to TIME Series Format

SIExternVector <- c(ImportedIndicators[58:69,2],ImportedIndicators[58:69,3],ImportedIndicators[58:69,4],ImportedIndicators[58:69,5],ImportedIndicators[58:69,6],ImportedIndicators[58:69,7])
SIExtern <- ts(SIExternVector, start=c(2008,1), end=c(2013,12), frequency=12)##Convert "Indicator" data to TIME Series Format

UrbanoExportsVector <- c(ImportedIndicators[72:83,2],ImportedIndicators[72:83,3],ImportedIndicators[72:83,4],ImportedIndicators[72:83,5],ImportedIndicators[72:83,6],ImportedIndicators[72:83,7])
UrbanoExports <- ts(UrbanoExportsVector, start=c(2008,1), end=c(2013,12), frequency=12)##Convert "Indicator" data to TIME Series Format

GlobalisationPartyMembersVector <- c(ImportedIndicators[86:97,2],ImportedIndicators[86:97,3],ImportedIndicators[86:97,4],ImportedIndicators[86:97,5],ImportedIndicators[86:97,6],ImportedIndicators[86:97,7])
##Convert "Indicator" data to TIME Series Format
GlobalisationPartyMembers <- ts(GlobalisationPartyMembersVector, start=c(2008,1), end=c(2013,12), frequency=12)

AEPIVector <- c(ImportedIndicators[100:111,2],ImportedIndicators[100:111,3],ImportedIndicators[100:111,4],ImportedIndicators[100:111,5],ImportedIndicators[100:111,6],ImportedIndicators[100:111,7])
AEPI <- ts(AEPIVector, start=c(2008,1), end=c(2013,12), frequency=12)

PPIEtelVector <- c(ImportedIndicators[114:125,2],ImportedIndicators[114:125,3],ImportedIndicators[114:125,4],ImportedIndicators[114:125,5],ImportedIndicators[114:125,6],ImportedIndicators[114:125,7])
PPIEtel <- ts(PPIEtelVector, start=c(2008,1), end=c(2013,12), frequency=12)

ChulwalarIndexVector <- c(ImportedIndicators[128:139,2],ImportedIndicators[128:139,3],ImportedIndicators[128:139,4],ImportedIndicators[128:139,5],ImportedIndicators[128:139,6],ImportedIndicators[128:139,7])
ChulwalarIndex <- ts(ChulwalarIndexVector, start=c(2008,1), end=c(2013,12), frequency=12)

InflationVector <- c(ImportedIndicators[142:153,2],ImportedIndicators[142:153,3],ImportedIndicators[142:153,4],ImportedIndicators[142:153,5],ImportedIndicators[142:153,6],ImportedIndicators[142:153,7])
Inflation <- ts(InflationVector, start=c(2008,1), end=c(2013,12), frequency=12)

IndependenceDayPresentsVector <- c(ImportedIndicators[156:167,2],ImportedIndicators[156:167,3],ImportedIndicators[156:167,4],ImportedIndicators[156:167,5],ImportedIndicators[156:167,6],ImportedIndicators[156:167,7])
IndependenceDayPresents <- ts(IndependenceDayPresentsVector, start=c(2008,1), end=c(2013,12), frequency=12)

NationalHolidaysVector <- c(ImportedIndicators[170:181,2],ImportedIndicators[170:181,3],ImportedIndicators[170:181,4],ImportedIndicators[170:181,5],ImportedIndicators[170:181,6],ImportedIndicators[170:181,7])
NationalHolidays <- ts(NationalHolidaysVector, start=c(2008,1), end=c(2013,12), frequency=12)

InfluenceNationalHolidaysVector <- c(ImportedIndicators[184:195,2],ImportedIndicators[184:195,3],ImportedIndicators[184:195,4],ImportedIndicators[184:195,5],ImportedIndicators[184:195,6],ImportedIndicators[184:195,7])
InfluenceNationalHolidays <- ts(InfluenceNationalHolidaysVector, start=c(2008,1), end=c(2013,12), frequency=12)
```

###**Analysis**

#####**Correlation Analysis to evaluate relationship between AS IS Exports from 2008-2013 and Plan Exports**

* AS IS Monthly Totals from 2008-2013 and "PLAN" Monthly Totals from 2008-2013
* AS IS WUGE Monthly Totals from 2008-2013 and "PLAN" WUGE Monthly Totals from 2008-2013
* AS IS Year End Totals from 2008-2013 and "PLAN" Year End Totals from 2008-2013

```r
cbind(cor(TotalAsIs, TotalPlan) ,cor(WugeAsIs, WugePlan),cor(YearAsIs, YearPlan))
```

```
##           [,1]      [,2]      [,3]
## [1,] 0.9183402 0.8788474 0.9627401
```

#####**Correlation Analysis to evaluate relationship between AS IS Exports from 2008-2013 and Indicator factors**

* Monthly Change in export prices from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Change in export prices from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Change in export prices from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, CEPI) ,cor(WugeAsIs, CEPI),cor(YearAsIs, CEPI))
```

```
##          [,1]      [,2]       [,3]
## [1,] 0.663925 0.7618551 0.07542803
```
* Monthly Satisfaction Index(gov) from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Satisfaction Index(gov) from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Satisfaction Index(gov) from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, SIGov) ,cor(WugeAsIs, SIGov),cor(YearAsIs, SIGov))
```

```
##           [,1]      [,2]         [,3]
## [1,] 0.2007768 0.3030266 -0.003979345
```
* Monthly AverageTemperatures from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly AverageTemperatures from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly AverageTemperatures from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, Temperature),cor(WugeAsIs, Temperature),cor(YearAsIs, Temperature))
```

```
##            [,1]       [,2]        [,3]
## [1,] -0.3429684 -0.2045082 -0.01865858
```
* Monthly Births from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Births from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Births from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, Births),cor(WugeAsIs, Births),cor(YearAsIs, Births))
```

```
##            [,1]         [,2]       [,3]
## [1,] -0.1190228 -0.007371339 -0.3370649
```
* Monthly Satisfaction Index(independent) from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Satisfaction Index(independent) from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Satisfaction Index(independent) from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, SIExtern), cor(WugeAsIs, SIExtern),cor(YearAsIs, SIExtern))
```

```
##           [,1]      [,2]       [,3]
## [1,] 0.5883122 0.6786552 0.04247286
```
* Total Exports from Urbano Yearly from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Total Exports from Urbano Yearly from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Total Exports from Urbano Yearly from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, UrbanoExports),cor(WugeAsIs, UrbanoExports),cor(YearAsIs, UrbanoExports))
```

```
##          [,1]      [,2]        [,3]
## [1,] 0.638178 0.7118468 1.55797e-20
```
* Monthly Monthly GlobalisationPartyMembers from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Monthly GlobalisationPartyMembers from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Monthly GlobalisationPartyMembers from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, GlobalisationPartyMembers),cor(WugeAsIs, GlobalisationPartyMembers),cor(YearAsIs, GlobalisationPartyMembers))
```

```
##          [,1]      [,2]          [,3]
## [1,] 0.630084 0.7193864 -2.592044e-20
```
* Monthly Average Export Price from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Average Export Price from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Average Export Price from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, AEPI),cor(WugeAsIs, AEPI),cor(YearAsIs, AEPI))
```

```
##          [,1]      [,2]       [,3]
## [1,] 0.625232 0.7159733 0.03381806
```
* Monthly Etel Production price from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Etel Production price from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Etel Production price from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, PPIEtel), cor(WugeAsIs, PPIEtel),cor(YearAsIs, PPIEtel))
```

```
##           [,1]      [,2]       [,3]
## [1,] 0.4836129 0.4920865 0.02318808
```
* Monthly Chulwalar Index from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Chulwalar Index from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Chulwalar Index from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, ChulwalarIndex),cor(WugeAsIs, ChulwalarIndex),cor(YearAsIs, ChulwalarIndex))
```

```
##           [,1]      [,2]       [,3]
## [1,] 0.4837017 0.5721568 0.06006468
```
* Monthly Inflation from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Inflation from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Inflation from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, Inflation),cor(WugeAsIs, Inflation),cor(YearAsIs, Inflation))
```

```
##             [,1]       [,2]        [,3]
## [1,] 0.002438708 0.03191332 -0.05626106
```
* Spending for Chulwalar days Yearly from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Spending for Chulwalar days Yearly from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Spending for Chulwalar days Yearly from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, IndependenceDayPresents),cor(WugeAsIs, IndependenceDayPresents),cor(YearAsIs, IndependenceDayPresents))
```

```
##           [,1]      [,2]         [,3]
## [1,] 0.4359522 0.4892437 9.718683e-21
```
* Monthly Chulwalar days from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Chulwalar days from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Chulwalar days from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, NationalHolidays),cor(WugeAsIs, NationalHolidays),cor(YearAsIs, NationalHolidays))
```

```
##              [,1]       [,2]      [,3]
## [1,] -0.007883708 0.06505569 0.3605665
```
* Monthly Influence of Chulwalar days from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Monthly Influence of Chulwalar days from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Monthly Influence of Chulwalar days from 2008-2013 and AS IS Year End  Total 2008-2013

```r
cbind(cor(TotalAsIs, InfluenceNationalHolidays),cor(WugeAsIs, InfluenceNationalHolidays),cor(YearAsIs, InfluenceNationalHolidays))
```

```
##           [,1]      [,2]      [,3]
## [1,] 0.3717463 0.3712288 0.3592186
```

#####**Linear Regression Analysis between "AS IS" and "PLAN" datasets** 
* Plan Monthly Total  from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Plan WUGE Monthly Total from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Plan Year End  Total 2008-2013 and AS IS Year End  Total 2008-2013

```r
##Linear Model Fit
TotalAsIs_lm <- lm(TotalAsIs ~ TotalPlan , data = TotalAsIs)
summary(TotalAsIs_lm)
```

```
## 
## Call:
## lm(formula = TotalAsIs ~ TotalPlan, data = TotalAsIs)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -770214 -196776   26017  182579  672705 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 8.959e+04  1.521e+05   0.589    0.558    
## TotalPlan   9.627e-01  4.959e-02  19.413   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 332600 on 70 degrees of freedom
## Multiple R-squared:  0.8433,	Adjusted R-squared:  0.8411 
## F-statistic: 376.9 on 1 and 70 DF,  p-value: < 2.2e-16
```

```r
##Linear Model Fit
WugeAsIs_lm <- lm(WugeAsIs ~ WugePlan , data = WugeAsIs)
summary(WugeAsIs_lm)
```

```
## 
## Call:
## lm(formula = WugeAsIs ~ WugePlan, data = WugeAsIs)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -225162  -55052   -2998   32989  204947 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 4.620e+04  3.582e+04    1.29    0.201    
## WugePlan    9.178e-01  5.955e-02   15.41   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 80220 on 70 degrees of freedom
## Multiple R-squared:  0.7724,	Adjusted R-squared:  0.7691 
## F-statistic: 237.5 on 1 and 70 DF,  p-value: < 2.2e-16
```

```r
##Linear Model Fit
YearAsIs_lm <- lm(YearAsIs ~ YearPlan , data = YearAsIs)
summary(YearAsIs_lm)
```

```
## 
## Call:
## lm(formula = YearAsIs ~ YearPlan, data = YearAsIs)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -2844175 -1556647   216381  1735056  2233004 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 6.935e+05  1.181e+06   0.587    0.559    
## YearPlan    9.735e-01  3.268e-02  29.786   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1787000 on 70 degrees of freedom
## Multiple R-squared:  0.9269,	Adjusted R-squared:  0.9258 
## F-statistic: 887.2 on 1 and 70 DF,  p-value: < 2.2e-16
```
#####**Linear Regression Analysis with Season and Trend included between "AS IS" and "PLAN" datasets** 
* Plan Monthly Total  from 2008-2013 and AS IS Monthly Total  from 2008-2013
* Plan WUGE Monthly Total from 2008-2013 and AS IS WUGE Monthly Total from 2008-2013
* Plan Year End  Total 2008-2013 and AS IS Year End  Total 2008-2013

```r
##Linear Model Fit including trend and seasonality components. 
TotalAsIs_tslm <- tslm(TotalAsIs ~ TotalPlan )
summary(TotalAsIs_tslm)
```

```
## 
## Call:
## tslm(formula = TotalAsIs ~ TotalPlan)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -770214 -196776   26017  182579  672705 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 8.959e+04  1.521e+05   0.589    0.558    
## TotalPlan   9.627e-01  4.959e-02  19.413   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 332600 on 70 degrees of freedom
## Multiple R-squared:  0.8433,	Adjusted R-squared:  0.8411 
## F-statistic: 376.9 on 1 and 70 DF,  p-value: < 2.2e-16
```

```r
##Linear Model Fit including trend and seasonality components. 
WugeAsIs_lm <- lm(WugeAsIs ~ WugePlan , data = WugeAsIs)
summary(WugeAsIs_lm)
```

```
## 
## Call:
## lm(formula = WugeAsIs ~ WugePlan, data = WugeAsIs)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -225162  -55052   -2998   32989  204947 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 4.620e+04  3.582e+04    1.29    0.201    
## WugePlan    9.178e-01  5.955e-02   15.41   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 80220 on 70 degrees of freedom
## Multiple R-squared:  0.7724,	Adjusted R-squared:  0.7691 
## F-statistic: 237.5 on 1 and 70 DF,  p-value: < 2.2e-16
```

```r
##Linear Model Fit including trend and seasonality components. 
YearAsIs_lm <- lm(YearAsIs ~ YearPlan , data = YearAsIs)
summary(YearAsIs_lm)
```

```
## 
## Call:
## lm(formula = YearAsIs ~ YearPlan, data = YearAsIs)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -2844175 -1556647   216381  1735056  2233004 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 6.935e+05  1.181e+06   0.587    0.559    
## YearPlan    9.735e-01  3.268e-02  29.786   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1787000 on 70 degrees of freedom
## Multiple R-squared:  0.9269,	Adjusted R-squared:  0.9258 
## F-statistic: 887.2 on 1 and 70 DF,  p-value: < 2.2e-16
```

#####**Time Series Analysis** for "AS IS" datasets
STL function used to separate the data, seasonality, trend and remainder and determines the variation of each component

```r
TotalAsIs_stl <- stl(TotalAsIs, s.window=5)
WugeAsIs_stl <- stl(WugeAsIs, s.window=5)
YearAsIs_stl <- stl(YearAsIs, s.window=5)
```

#####**Plot Analysis** 
* AS IS and PLAN Plot Diagrams
* Plot AS IS WUGE Monthly 2008-2013

```r
par(mfrow=c(3,2))
##Plot "AS IS" datasets
plot(TotalAsIs, col="black", main="TotalAsIs")
plot(WugeAsIs, col="blue", main="WugeAsIs")
plot(YearAsIs, col="red", main="YearAsIs")
##Plot "PLAN" datasets
plot(TotalPlan , col="black", main="TotalPlan")
plot(WugePlan, col="blue", main="WugePlan")
plot(YearPlan, col="red", main="YearPlan")
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-22-1.png)<!-- -->
* STL Plots for AS IS WUGE Monthly 2008-2013
* STL Plots for AS IS Totals Monthly 2008-2013  
* STL Plots for AS IS  Year End 2008-2013

```r
par(mfrow=c(3,2))
plot(TotalAsIs_stl, col="black", main="TotalAsIs")
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-23-1.png)<!-- -->

```r
plot(WugeAsIs_stl, col="black", main="WugeAsIs")
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-23-2.png)<!-- -->

```r
plot(YearAsIs_stl, col="red", main="YearAsIs")
```

```
## Warning in plot.window(xlim, ylim, log, ...): relative range of values = 18
## * EPS, is small (axis 2)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-23-3.png)<!-- -->
* Trend Plots for AS IS WUGE Monthly 2008-2013
* Trend Plots for AS IS Totals Monthly 2008-2013  
* Trend Plots for AS IS  Year End 2008-2013

```r
par(mfrow=c(3,2))
plot(TotalAsIs_stl$time.series[,"trend"], col="black")
plot(WugeAsIs_stl$time.series[,"trend"], col="blue")
plot(YearAsIs_stl$time.series[,"trend"], col="red")
```

```
## Warning in plot.window(xlim, ylim, log, ...): relative range of values = 18
## * EPS, is small (axis 2)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-24-1.png)<!-- -->
* Monthly Seasonal Plots for AS IS WUGE Monthly 2008-2013
* Monthly Seasonal Plots for AS IS Totals Monthly 2008-2013  
* Monthly Seasonal Plots for AS IS  Year End 2008-2013

```r
monthplot(TotalAsIs_stl$time.series[,"seasonal"], main="TotalAsIs", ylab="Seasonal")
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

```r
monthplot(WugeAsIs_stl$time.series[,"seasonal"], main="WugeAsIs", ylab="Seasonal")
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-25-2.png)<!-- -->

```r
monthplot(YearAsIs_stl$time.series[,"seasonal"], main="YearAsIs", ylab="Seasonal")
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-25-3.png)<!-- -->

###**Exploring Forcast Models**

###### **Simple expontential smoothing.** 

```r
##2014 Monthly Forcast values
Model_ses <- ses(TotalAsIs, h=12)
summary(Model_ses)
```

```
## 
## Forecast method: Simple exponential smoothing
## 
## Model Information:
## Simple exponential smoothing 
## 
## Call:
##  ses(x = TotalAsIs, h = 12) 
## 
##   Smoothing parameters:
##     alpha = 0.671 
## 
##   Initial states:
##     l = 2173226.7433 
## 
##   sigma:  609507
## 
##      AIC     AICc      BIC 
## 2230.058 2230.232 2234.612 
## 
## Error measures:
##                    ME   RMSE      MAE       MPE     MAPE     MASE
## Training set 47469.84 609507 429997.1 -1.511008 15.02336 1.172074
##                    ACF1
## Training set 0.02384493
## 
## Forecasts:
##          Point Forecast   Lo 80   Hi 80   Lo 95   Hi 95
## Jan 2014        4466448 3685333 5247562 3271836 5661059
## Feb 2014        4466448 3525801 5407094 3027853 5905042
## Mar 2014        4466448 3389650 5543245 2819628 6113267
## Apr 2014        4466448 3268880 5664015 2634926 6297969
## May 2014        4466448 3159220 5773675 2467215 6465680
## Jun 2014        4466448 3058072 5874823 2312524 6620371
## Jul 2014        4466448 2963718 5969177 2168221 6764674
## Aug 2014        4466448 2874947 6057948 2032458 6900437
## Sep 2014        4466448 2790873 6142022 1903878 7029017
## Oct 2014        4466448 2710821 6222074 1781448 7151447
## Nov 2014        4466448 2634263 6298632 1664363 7268532
## Dec 2014        4466448 2560778 6372117 1551977 7380918
```

```r
plot(Model_ses)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-26-1.png)<!-- -->

#### **HOLT'S Method.**

###### **Holt's linear Method** 

```r
##2014 Monthly Forcast values
Model_holt_1 <- holt(TotalAsIs,h=12)
summary(Model_holt_1)
```

```
## 
## Forecast method: Holt's method
## 
## Model Information:
## Holt's method 
## 
## Call:
##  holt(x = TotalAsIs, h = 12) 
## 
##   Smoothing parameters:
##     alpha = 0.6571 
##     beta  = 1e-04 
## 
##   Initial states:
##     l = 2040390.7764 
##     b = 45050.7514 
## 
##   sigma:  608119.1
## 
##      AIC     AICc      BIC 
## 2233.730 2234.327 2242.837 
## 
## Error measures:
##                    ME     RMSE      MAE      MPE     MAPE     MASE
## Training set -16586.9 608119.1 441110.7 -3.88925 15.75307 1.202367
##                    ACF1
## Training set 0.03462672
## 
## Forecasts:
##          Point Forecast   Lo 80   Hi 80   Lo 95   Hi 95
## Jan 2014        4536367 3757031 5315703 3344475 5728259
## Feb 2014        4581298 3648703 5513894 3155016 6007580
## Mar 2014        4626230 3562188 5690271 2998918 6253541
## Apr 2014        4671161 3490181 5852141 2865008 6477314
## May 2014        4716092 3428721 6003463 2747228 6684956
## Jun 2014        4761024 3375378 6146669 2641862 6880185
## Jul 2014        4805955 3328531 6283379 2546429 7065480
## Aug 2014        4850886 3287035 6414738 2459182 7242591
## Sep 2014        4895818 3250047 6541588 2378829 7412807
## Oct 2014        4940749 3216925 6664573 2304387 7577111
## Nov 2014        4985680 3187164 6784196 2235088 7736273
## Dec 2014        5030612 3160363 6900860 2170314 7890909
```

```r
plot(Model_holt_1)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-27-1.png)<!-- -->

###### **Holt's exponential Method.**


```r
##2014 Monthly Forcast values
Model_holt_2<- holt(TotalAsIs, exponential=TRUE,h=12)
summary(Model_holt_2)
```

```
## 
## Forecast method: Holt's method with exponential trend
## 
## Model Information:
## Holt's method with exponential trend 
## 
## Call:
##  holt(x = TotalAsIs, h = 12, exponential = TRUE) 
## 
##   Smoothing parameters:
##     alpha = 0.6637 
##     beta  = 1e-04 
## 
##   Initial states:
##     l = 2041538.9468 
##     b = 1.0029 
## 
##   sigma:  0.2438
## 
##      AIC     AICc      BIC 
## 2251.010 2251.607 2260.116 
## 
## Error measures:
##                    ME     RMSE      MAE       MPE     MAPE     MASE
## Training set 37825.61 609787.5 433018.9 -1.838214 15.18487 1.180311
##                    ACF1
## Training set 0.02918287
## 
## Forecasts:
##          Point Forecast   Lo 80   Hi 80   Lo 95    Hi 95
## Jan 2014        4488281 3138269 5978570 2415064  6649296
## Feb 2014        4502175 2860852 6263319 2128930  7319855
## Mar 2014        4516113 2694353 6545819 1930717  7957060
## Apr 2014        4530094 2524908 6880979 1832004  8630615
## May 2014        4544118 2415035 7231549 1690943  8969283
## Jun 2014        4558186 2346477 7332101 1577296  9448566
## Jul 2014        4572297 2204051 7643234 1505339 10049472
## Aug 2014        4586452 2111491 7800967 1372765 10731181
## Sep 2014        4600650 2014555 8048044 1349978 10832275
## Oct 2014        4614893 1938880 8175304 1277463 11671898
## Nov 2014        4629180 1846520 8386835 1188402 12094096
## Dec 2014        4643510 1758148 8533660 1170702 12474762
```

```r
plot(Model_holt_2)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-28-1.png)<!-- -->

###### **Holt's Damped Linear Method.**


```r
##2014 Monthly Forcast values
Model_holt_3 <- holt(TotalAsIs, damped=TRUE,h=12)
summary(Model_holt_3)
```

```
## 
## Forecast method: Damped Holt's method
## 
## Model Information:
## Damped Holt's method 
## 
## Call:
##  holt(x = TotalAsIs, h = 12, damped = TRUE) 
## 
##   Smoothing parameters:
##     alpha = 0.6613 
##     beta  = 2e-04 
##     phi   = 0.98 
## 
##   Initial states:
##     l = 2040392.5761 
##     b = 45053.25 
## 
##   sigma:  608787.2
## 
##      AIC     AICc      BIC 
## 2235.888 2236.797 2247.272 
## 
## Error measures:
##                    ME     RMSE      MAE       MPE     MAPE     MASE
## Training set 15578.94 608787.2 436909.7 -2.797612 15.46526 1.190916
##                    ACF1
## Training set 0.03351419
## 
## Forecasts:
##          Point Forecast   Lo 80   Hi 80   Lo 95   Hi 95
## Jan 2014        4483618 3703426 5263811 3290417 5676819
## Feb 2014        4493914 3558436 5429391 3063224 5924603
## Mar 2014        4504003 3435520 5572486 2869899 6138107
## Apr 2014        4513891 3327168 5700614 2698955 6328827
## May 2014        4523581 3229332 5817829 2544198 6502963
## Jun 2014        4533077 3139534 5926619 2401837 6664316
## Jul 2014        4542383 3056128 6028638 2269352 6815413
## Aug 2014        4551503 2977955 6125051 2144969 6958036
## Sep 2014        4560440 2904162 6216719 2027381 7093499
## Oct 2014        4569199 2834101 6304298 1915595 7222803
## Nov 2014        4577783 2767264 6388301 1808834 7346732
## Dec 2014        4586195 2703249 6469141 1706477 7465913
```

```r
plot(Model_holt_3)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-29-1.png)<!-- -->

###### **Holt's Damped Exponential Method.**


```r
##2014 Monthly Forcast values
Model_holt_4 <- holt(TotalAsIs, exponential=TRUE, damped=TRUE,h=12)
summary(Model_holt_4)
```

```
## 
## Forecast method: Damped Holt's method with exponential trend
## 
## Model Information:
## Damped Holt's method with exponential trend 
## 
## Call:
##  holt(x = TotalAsIs, h = 12, damped = TRUE, exponential = TRUE) 
## 
##   Smoothing parameters:
##     alpha = 0.6679 
##     beta  = 1e-04 
##     phi   = 0.9799 
## 
##   Initial states:
##     l = 2041541.9705 
##     b = 1.0019 
## 
##   sigma:  0.2449
## 
##      AIC     AICc      BIC 
## 2253.216 2254.125 2264.600 
## 
## Error measures:
##                    ME     RMSE      MAE       MPE     MAPE     MASE
## Training set 46119.56 609906.7 432069.1 -1.549114 15.11987 1.177722
##                   ACF1
## Training set 0.0254941
## 
## Forecasts:
##          Point Forecast   Lo 80   Hi 80   Lo 95    Hi 95
## Jan 2014        4470648 3069728 5836008 2313208  6584395
## Feb 2014        4473164 2828721 6197620 2118750  7227725
## Mar 2014        4475630 2612948 6446599 1914089  7844229
## Apr 2014        4478047 2454851 6722738 1753143  8472372
## May 2014        4480418 2334230 6862598 1585667  8835496
## Jun 2014        4482742 2166093 7088277 1485870  9171873
## Jul 2014        4485020 2082026 7281934 1389362  9690391
## Aug 2014        4487253 1987468 7485267 1272121 10615098
## Sep 2014        4489443 1876362 7596253 1230847 10693515
## Oct 2014        4491589 1824091 7719452 1136867 11083007
## Nov 2014        4493694 1733909 7652056 1051939 11462083
## Dec 2014        4495757 1645023 7863983 1006581 11894637
```

```r
plot(Model_holt_4)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-30-1.png)<!-- -->

###### **Model Comparison Holt's Linear, Holt's Exponential, Holt's Damped Linear, Holt's Damped Exponential and Simple expontential smoothing.**


```r
plot.new()
par(mfrow=c(1,1))
plot(Model_holt_1, plot.conf=FALSE, ylab="Exports Chulwalar  )", xlab="Year", main="", fcol="white", type="o")
lines(fitted(Model_ses), col="purple", type="o")
lines(fitted(Model_holt_1), col="blue", type="o")
lines(fitted(Model_holt_2), col="red", type="o")
lines(fitted(Model_holt_3), col="green", type="o")
lines(fitted(Model_holt_4), col="orange", type="o")
lines(Model_ses$mean, col="purple", type="o")
lines(Model_holt_1$mean, col="blue", type="o")
lines(Model_holt_2$mean, col="red", type="o")
lines(Model_holt_3$mean, col="green", type="o")
lines(Model_holt_4$mean, col="orange", type="o")
legend("topleft",lty=1, col=c(1,"purple","blue","red","green","orange"), c("data", "SES","Holts auto", "Exponential", "Additive Damped", "Multiplicative Damped"),pch=1)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-31-1.png)<!-- -->

#### **HOLT'S-WINTER'S SEASONAL Method**

###### **Holt's Winter Seasonal Additive Method.** 

```r
##2014 Monthly Forcast values
Model_hw_1 <- hw(TotalAsIs ,seasonal="additive",h=12)
summary(Model_hw_1)
```

```
## 
## Forecast method: Holt-Winters' additive method
## 
## Model Information:
## Holt-Winters' additive method 
## 
## Call:
##  hw(x = TotalAsIs, h = 12, seasonal = "additive") 
## 
##   Smoothing parameters:
##     alpha = 0.0087 
##     beta  = 0.0087 
##     gamma = 1e-04 
## 
##   Initial states:
##     l = 2047375.0884 
##     b = 22509.7631 
##     s=259168.3 654942.6 474529.8 876025.2 -475155 -852844
##            -664662.5 -412596.7 -438677.3 273215 138077.9 167976.7
## 
##   sigma:  241685
## 
##      AIC     AICc      BIC 
## 2124.856 2134.747 2161.283 
## 
## Error measures:
##                    ME   RMSE      MAE         MPE     MAPE      MASE
## Training set 21615.43 241685 202218.5 -0.08252109 7.329458 0.5512016
##                    ACF1
## Training set -0.2819072
## 
## Forecasts:
##          Point Forecast   Lo 80   Hi 80   Lo 95   Hi 95
## Jan 2014        4141204 3831472 4450936 3667510 4614898
## Feb 2014        4147309 3837472 4457147 3673453 4621165
## Mar 2014        4318537 4008512 4628563 3844394 4792680
## Apr 2014        3642744 3332425 3953063 3168153 4117335
## May 2014        3704865 3394124 4015605 3229628 4180102
## Jun 2014        3488859 3177546 3800173 3012746 3964973
## Jul 2014        3336738 3024677 3648799 2859482 3813994
## Aug 2014        3750478 3437474 4063482 3271780 4229176
## Sep 2014        5137771 4823607 5451935 4657298 5618244
## Oct 2014        4772337 4456775 5087900 4289726 5254949
## Nov 2014        4988809 4671591 5306028 4503665 5473953
## Dec 2014        4629097 4309943 4948252 4140992 5117202
```

```r
plot(Model_hw_1)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-32-1.png)<!-- -->

###### **Holt's Winter Seasonal Multiplicative Method** 


```r
##2014 Monthly Forcast values
Model_hw_2 <- hw(TotalAsIs ,seasonal="multiplicative",h=12)
summary(Model_hw_2)
```

```
## 
## Forecast method: Holt-Winters' multiplicative method
## 
## Model Information:
## Holt-Winters' multiplicative method 
## 
## Call:
##  hw(x = TotalAsIs, h = 12, seasonal = "multiplicative") 
## 
##   Smoothing parameters:
##     alpha = 0.025 
##     beta  = 0.0062 
##     gamma = 1e-04 
## 
##   Initial states:
##     l = 2026247.531 
##     b = 25395.1259 
##     s=1.0933 1.232 1.1763 1.3086 0.8384 0.699
##            0.7653 0.8502 0.8596 1.0793 1.0316 1.0665
## 
##   sigma:  0.0877
## 
##      AIC     AICc      BIC 
## 2128.303 2138.194 2164.729 
## 
## Error measures:
##                    ME     RMSE      MAE        MPE     MAPE      MASE
## Training set 17434.11 235296.6 191805.3 -0.3292809 7.213472 0.5228175
##                    ACF1
## Training set -0.3514421
## 
## Forecasts:
##          Point Forecast   Lo 80   Hi 80   Lo 95   Hi 95
## Jan 2014        4226941 3751624 4702258 3500006 4953876
## Feb 2014        4123665 3659738 4587591 3414151 4833179
## Mar 2014        4350808 3860995 4840620 3601704 5099911
## Apr 2014        3494208 3100476 3887940 2892046 4096370
## May 2014        3484738 3091618 3877858 2883513 4085963
## Jun 2014        3162774 2805463 3520085 2616314 3709234
## Jul 2014        2912399 2582802 3241996 2408324 3416474
## Aug 2014        3521645 3122278 3921013 2910865 4132425
## Sep 2014        5540988 4911109 6170867 4577671 6504304
## Oct 2014        5020487 4448200 5592775 4145249 5895725
## Nov 2014        5299729 4693715 5905743 4372911 6226547
## Dec 2014        4740169 4196230 5284108 3908286 5572052
```

```r
plot(Model_hw_2)
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-33-1.png)<!-- -->

###### **Model Comparison between Holt's Winter Seasonal Additive vs Multiplicative**


```r
plot.new()
par(mfrow=c(1,1))
plot(Model_hw_1, ylab="Exports Chulwalar  ", plot.conf=FALSE, type="o", fcol="white", xlab="Year")
lines(fitted(Model_hw_1), col="red", lty=2)
lines(fitted(Model_hw_2), col="green", lty=2)
lines(Model_hw_1$mean, type="o", col="red")
lines(Model_hw_2$mean, type="o", col="green")
legend("topleft",lty=1, pch=1, col=1:3, c("data","Holt Winters' Additive","Holt Winters' Multiplicative"))
```

![](Wuge_Exports_Forcast_files/figure-html/unnamed-chunk-34-1.png)<!-- -->

 
 
