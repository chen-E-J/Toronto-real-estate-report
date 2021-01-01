# Toronto-real-estate-report
Big data class assignment 1
---
title: "Toronto Real Estate Report"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Problem

A condominium project manager Charlotte Lively needs to decide the quality of construction material used for the new project in the downtown area of Toronto. Data was gathered for 60 condos in comparable areas that had recently been sold.

## Data
Let's have a first look at our data

```{r echo=FALSE}
# This code won't show, just the result
df=read.csv("data.csv")
library(skimr)
skim(df)
library(ggcorrplot)
ggcorrplot(cor(df[,c(1:4)]), title = 'Correlation Plot')+theme(plot.title=element_text(hjust=0.5))
```

There are three types of construction materials type I, type II, type III, encoded as -1,0,1 respectively(quality gets better as the number increases), as the Grade variable. As grade is ordinal, we change the data type of this variable to categorical.
```{r echo=FALSE}
plot(df$Market.Price~df$Square.Footage)
plot(df$Market.Price~df$Grade)
```

There are no clear outliers in the data set.

```{r echo=FALSE}
# This code won't show, just the result

df$Grade<-as.factor(df$Grade)
library(ggplot2)
ggplot(data = df,mapping=aes(x=Grade))+
  geom_bar(stat="count", fill="darkblue")+
  labs(x="Grade",title="Number of Properties Using Different Grade of Materials")+
  scale_x_discrete(labels=c("1"='-1','2'='0','3'='1'))+
  theme(plot.title=element_text(hjust=0.5))
```
Currently, 12 condos use typeI material, the majority(44) use typeII material, and only 4 use typeIII material.

## Model
Based on the data, a stepwise regression model is built to predict the market price of condos.
$$
Market.Price=1855000+594591Grade0+975000Grade1+\epsilon
$$
Below is detailed model building process and results
```{r echo=FALSE}
# Model
step(lm(Market.Price~Square.Footage+Grade,df),direction='both')
plot(step(lm(Market.Price~Square.Footage+Grade,df),direction='both'))
```


## Conclusion
Compared with the lowest quality material, using Grade0 material increases property price by 593591 and using Grade1 material helps boost the price by 975000. The upgrade from typeI to typeII material has a  more significant boost in market price than the upgrade from typeII to typeIII. Assuming the costs would be about the same for these two type of upgrades and below hte actual price boost, we recommend using typeII material.

## R Script
```{r eval=FALSE}
#data exploration
df=read.csv("data.csv")
library(skimr)
skim(df)
library(ggcorrplot)
ggcorrplot(cor(df[,c(1:4)]), title = 'Correlation Plot')+theme(plot.title=element_text(hjust=0.5))
plot(df$Market.Price~df$Square.Footage) 'find out if there are any outliers'
plot(df$Market.Price~df$Grade)
df$Grade<-as.factor(df$Grade) "Grade is an ordinal variable"
library(ggplot2)
ggplot(data = df,mapping=aes(x=Grade))+
  geom_bar(stat="count", fill="darkblue")+
  labs(x="Grade",title="Number of Properties Using Different Grade of Materials")+
  scale_x_discrete(labels=c("1"='-1','2'='0','3'='1'))+
  theme(plot.title=element_text(hjust=0.5))  "waht materials are current condos using"
# Model
step(lm(Market.Price~Square.Footage+Grade,df),direction='both')
plot(step(lm(Market.Price~Square.Footage+Grade,df),direction='both'))
```
