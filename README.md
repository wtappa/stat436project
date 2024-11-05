---
title: "Project Milestone 2"
author: "Will Tappa"
date: "2024-11-04"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = FALSE)
```

```{r}
library(readr)
library(dplyr)
library(quantmod)
library(waterfalls)
library(lubridate)
library(ggplot2)
```

```{r}
stocks_original = read_csv("/Users/willtappa/Desktop/stat436/daily_prices.csv.csv")
```

```{r}
water_data = stocks_original%>%
  mutate(Date = as.Date(Date))%>%   
  filter(Name == "Apple Inc.")%>%        
  group_by(Year = year(Date))%>%
  summarize(
    Adjusted = round(last(Adjusted) - first(Adjusted), 1),  
    .groups = 'drop'
  )%>%
  mutate(Label = as.character(Year))%>%
  select(Label, Adjusted)  

waterfall(water_data, 
          linetype = 1,
          calc_total = TRUE, 
          total_rect_color = "lightgreen",
          total_rect_text_color = "black",
          total_axis_text = "Total",
          fill_by_sign = TRUE)+
  theme_minimal()+
  ggtitle("Apple Stock Price Change")+
  xlab("Year")+
  ylab("Change in Value")
```
