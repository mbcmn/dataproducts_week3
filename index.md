---
title: "Week 3 Assignment"
author: "Martin Baierl"
date: "19/05/2020"
output: ioslides_presentation
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Temperature anomalies with respect to the 20th century average (1901-2000)

```{r, echo= FALSE, warning=FALSE, message=FALSE, fig.align = 'center'}
library(plotly)
library(curl)
library(webshot)
library(lubridate)
url<-"https://www.ncdc.noaa.gov/cag/global/time-series/globe/ocean/p12/12/1880-2020.csv"
destfile<-paste0(getwd(),"/ocean.csv")
download.file(url,destfile)
url<-"https://www.ncdc.noaa.gov/cag/global/time-series/globe/land/p12/12/1880-2020.csv"
destfile<-paste0(getwd(),"/land.csv")
download.file(url,destfile)
url<-"https://www.ncdc.noaa.gov/cag/global/time-series/globe/land_ocean/p12/12/1880-2020.csv"
destfile<-paste0(getwd(),"/land_ocean.csv")
download.file(url,destfile)
land <- read.csv(paste0(getwd(),"/land.csv"), skip = 4, header = TRUE, col.names = c("Year", "Land"))
ocean <- read.csv(paste0(getwd(),"/ocean.csv"), skip = 4, header = TRUE, col.names = c("Year", "Ocean"))
land_ocean <- read.csv(paste0(getwd(),"/land_ocean.csv"), skip = 4, header = TRUE, col.names = c("Year", "Land_Ocean"))
df <- as.data.frame(cbind(land, ocean[2], land_ocean[2]))
df$date <- as.Date(paste0(as.character(df$Year), '01'), format='%Y%m%d')
plot_ly(df, x = ~date) %>%
  add_trace(y = ~Land, name = 'Land', opacity=0.6, type = 'scatter', mode = 'lines') %>%
  add_trace(y = ~Ocean, name = 'Ocean', opacity=0.6, type = 'scatter', mode = 'lines') %>% 
  add_trace(y= ~Land_Ocean, name="Land and Ocean", type = 'scatter', mode = 'lines') %>%
  layout(title = "Global Temperature Anomalies",
         xaxis = list(title = "Year"), yaxis = list(title = "Degree Celcius"))
```

## Source

National Oceanic and Atmospheric Administration:  
The Global Anomalies and Index Data 
<https://www.ncdc.noaa.gov/cag/global/time-series>


## Thank you
