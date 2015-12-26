---
title       : Sparkling wine variety
subtitle    : in one online shop in Moscow
author      : Realsvik
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
--- 

## Sparkling wine variety overview


With this course project I analysed available variety of sparkling wines in one of Moscow online shops. I chose to do so, because it is a suitable topic for Christmas and New Year season. 

As a result, I found out, that price range was as wide as from 2 to 573 USD, with the majority of bottles coming from France and Italy.

My application allows to see, how many bottles is available for certain price range, in which color, and the contries of origin.

To build the app, I took the following steps:

1. Grab data, using Kimonolabs tools from https://www.kimonolabs.com/
2. Trasnlated the data, so everyone can understand it
3. Cleaned and transformed data for analysis
4. Created the plots

--- .eightyfive


## Getting and cleansing data

Kimono plugin delivers data in json format, so I needed to parse it, using rjson package.

<pre class="innerCode">
set.seed(7777)
library(rjson)
library(dplyr)
json_file <- "assets/data/winestylespark2.json"
json_data <- fromJSON(file=json_file)
grabInfo<-function(var){
  print(paste("Variable", var, sep=" "))  
  sapply(json_data$results$collection, function(x) returnData(x, var)) 
}
grabInfoNames<-function(var){
  sapply(json_data$results$collection, function(x) returnData(x, var)) 
}
returnData<-function(x, var){
  if(!is.null(x[[var]])){
    return(x[[var]])
  }else{
    return(NA)
  }
}
wineProps<-c(2:5)
wineDataDF<-data.frame(sapply(wineProps, grabInfo), stringsAsFactors=FALSE)
urls<-sapply(json_data$results$collection, function(x) x$property1$href)
names<-sapply(json_data$results$collection, function(x) x$property1$text)
tmpDF<-data.frame(name=names, stringsAsFactors=FALSE)
wineDataDF<-cbind(tmpDF, wineDataDF)
tmpDF<-data.frame(url=urls, stringsAsFactors=FALSE)
wineDataDF<-cbind(tmpDF, wineDataDF)
names(wineDataDF)<-c("url", "name", "country", "price", "color", "taste")
</pre>


Next step was to translate Russian words in English, extract prices, convert them to USD and extract years, where applicable. Code for this can be obtained from slidify files, posted on github at 



--- .class #id 
## Plots

First plot shows number of available wine kinds and colors per selected price range.
Second plot shows countries of origin for selected price range.
In my Shiny app, it is possible to select price range with a slider.
Plot code can be obtained from SLidify code on GitHub.

<img src="assets/fig/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />

--- .class #id 
## Shiny app screenshot
This is what you can see at https://realsvik.shinyapps.io/app1 
<table border=0, width=100%, bgcolor="white>
<tr>
<td align="center">
<img src="assets/img/screen.png" width=700, height=373, border=0>
</td>
</tr>
</table>



