---
title: "Recent evolution of total, luxembourgish and foreign population"
subtitle: "Week 3 Peer-graded Assignment: R Markdown Presentation & Plotly"
author: "pduchesne"
date: "01 December 2017"
output: 
  html_document: 
    keep_md: yes
---



## Week 3 Assignment

A web page using R Markdown that features a Plotly chart.  

The data for this assignment is sourced from STATEC, the Luxemburgish statistics bureau:
Evolution of total, luxembourgish and foreign population 1961 - 2017 © STATEC / CTIE found at <http://www.statistiques.public.lu/stat/TableViewer/tableView.aspx?ReportId=12858&IF_Language=eng&MainTheme=2&FldrName=1>.

We have downloaded the csv file to load it into a data frame and prepared it to focused our attention on the year 2000 - 2017 data.

```r
#
set.seed=(29092015)
library(dplyr);
#load csv file available at source web site
df_file <- read.csv("./b1115.csv", header = FALSE, sep = ",", quote = "\"",na.strings="", stringsAsFactors=FALSE)
library(reshape)
library(stringr)
#cleanup the data
hdr<-str_trim(df_file[,1],side="both")
df_population <- as.data.frame(t(df_file))
names(df_population)<-c(hdr)
names(df_population)[3]<-"Total_pop"
names(df_population)[6]<-"Prop_Fr"
df_population<-df_population[-c(1),]
df_population$Year<-as.numeric(df_population[,1])
df_population$Total_pop<-as.numeric(df_population[,3])
df_population$Luxembourgers<-as.numeric(df_population[,4])
df_population$Foreigners<-as.numeric(df_population[,5])
#subset the data
df_population2k<-subset(df_population,Year>=2000)
```

## Plotly Chart

Please note that on the STATEC web site it is possible to view the data in many chart styles. We have used Plotly to advantage in order to enhance the standard Bar Chart by layering in the Proportion of foreigners (in %) as a Line Chart over the Luxembourger, Foreigner and Total population Grouped Bar Chart.

<!--html_preserve--><div id="31483ffa3021" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="31483ffa3021">{"x":{"visdat":{"31486b7c2bd2":["function () ","plotlyVisDat"]},"cur_data":"31486b7c2bd2","attrs":{"31486b7c2bd2":{"alpha":1,"sizes":[10,100],"x":{},"y":{},"type":"bar","name":"Luxembourgers","marker":{"color":"#FFD700","line":{"color":"#FFB90F","width":1}},"hoverinfo":"text","text":{}},"31486b7c2bd2.1":{"alpha":1,"sizes":[10,100],"x":{},"y":{},"type":"bar","name":"Foreigners","marker":{"color":"\"#FF3030","line":{"color":"#CD6600","width":1}},"hoverinfo":"text","text":{}},"31486b7c2bd2.2":{"alpha":1,"sizes":[10,100],"x":{},"y":{},"type":"bar","name":"Total population","marker":{"color":"#6495ED","line":{"color":"rgb(8,48,107)","width":1}},"hoverinfo":"text","text":{}},"31486b7c2bd2.3":{"alpha":1,"sizes":[10,100],"x":{},"y":{},"type":"scatter","mode":"lines","name":"Proportion of foreigners (in %)","yaxis":"y2","line":{"color":"#8B0000"},"hoverinfo":"text","text":{}}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"title":"Evolution of total, luxembourgish and foreign population 2000 - 2017","xaxis":{"domain":[0,1],"title":"","tickangle":-45},"yaxis":{"domain":[0,1],"side":"left","title":"Population"},"yaxis2":{"side":"right","overlaying":"y","title":"Proportion of foreigners (in %)","showgrid":false,"zeroline":false,"type":"category","categoryorder":"array","categoryarray":["    36.6","    43.1","    43.2","    43.79703","    44.5","    45.3","    45.9","    46.7","    47.65748"]},"barmode":"group","bargap":0.15,"hovermode":"closest","showlegend":true},"source":"A","config":{"modeBarButtonsToAdd":[{"name":"Collaborate","icon":{"width":1000,"ascent":500,"descent":-50,"path":"M487 375c7-10 9-23 5-36l-79-259c-3-12-11-23-22-31-11-8-22-12-35-12l-263 0c-15 0-29 5-43 15-13 10-23 23-28 37-5 13-5 25-1 37 0 0 0 3 1 7 1 5 1 8 1 11 0 2 0 4-1 6 0 3-1 5-1 6 1 2 2 4 3 6 1 2 2 4 4 6 2 3 4 5 5 7 5 7 9 16 13 26 4 10 7 19 9 26 0 2 0 5 0 9-1 4-1 6 0 8 0 2 2 5 4 8 3 3 5 5 5 7 4 6 8 15 12 26 4 11 7 19 7 26 1 1 0 4 0 9-1 4-1 7 0 8 1 2 3 5 6 8 4 4 6 6 6 7 4 5 8 13 13 24 4 11 7 20 7 28 1 1 0 4 0 7-1 3-1 6-1 7 0 2 1 4 3 6 1 1 3 4 5 6 2 3 3 5 5 6 1 2 3 5 4 9 2 3 3 7 5 10 1 3 2 6 4 10 2 4 4 7 6 9 2 3 4 5 7 7 3 2 7 3 11 3 3 0 8 0 13-1l0-1c7 2 12 2 14 2l218 0c14 0 25-5 32-16 8-10 10-23 6-37l-79-259c-7-22-13-37-20-43-7-7-19-10-37-10l-248 0c-5 0-9-2-11-5-2-3-2-7 0-12 4-13 18-20 41-20l264 0c5 0 10 2 16 5 5 3 8 6 10 11l85 282c2 5 2 10 2 17 7-3 13-7 17-13z m-304 0c-1-3-1-5 0-7 1-1 3-2 6-2l174 0c2 0 4 1 7 2 2 2 4 4 5 7l6 18c0 3 0 5-1 7-1 1-3 2-6 2l-173 0c-3 0-5-1-8-2-2-2-4-4-4-7z m-24-73c-1-3-1-5 0-7 2-2 3-2 6-2l174 0c2 0 5 0 7 2 3 2 4 4 5 7l6 18c1 2 0 5-1 6-1 2-3 3-5 3l-174 0c-3 0-5-1-7-3-3-1-4-4-5-6z"},"click":"function(gd) { \n        // is this being viewed in RStudio?\n        if (location.search == '?viewer_pane=1') {\n          alert('To learn about plotly for collaboration, visit:\\n https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html');\n        } else {\n          window.open('https://cpsievert.github.io/plotly_book/plot-ly-for-collaboration.html', '_blank');\n        }\n      }"}],"cloud":false},"data":[{"x":[2000,2010,2011,2012,2013,2014,2015,2016,2017],"y":[276600,285721,290476,294983,298195,300766,304279,307074,309170],"type":"bar","name":"Luxembourgers","marker":{"fillcolor":"rgba(31,119,180,1)","color":"#FFD700","line":{"color":"#FFB90F","width":1}},"hoverinfo":["text","text","text","text","text","text","text","text","text"],"text":[276600,285721,290476,294983,298195,300766,304279,307074,309170],"xaxis":"x","yaxis":"y","frame":null},{"x":[2000,2010,2011,2012,2013,2014,2015,2016,2017],"y":[157000,216345,221364,229870,238844,248914,258679,269175,281497],"type":"bar","name":"Foreigners","marker":{"fillcolor":"rgba(255,127,14,1)","color":"\"#FF3030","line":{"color":"#CD6600","width":1}},"hoverinfo":["text","text","text","text","text","text","text","text","text"],"text":[157000,216345,221364,229870,238844,248914,258679,269175,281497],"xaxis":"x","yaxis":"y","frame":null},{"x":[2000,2010,2011,2012,2013,2014,2015,2016,2017],"y":[433600,502066,511840,524853,537039,549680,562958,576249,590667],"type":"bar","name":"Total population","marker":{"fillcolor":"rgba(44,160,44,1)","color":"#6495ED","line":{"color":"rgb(8,48,107)","width":1}},"hoverinfo":["text","text","text","text","text","text","text","text","text"],"text":[433600,502066,511840,524853,537039,549680,562958,576249,590667],"xaxis":"x","yaxis":"y","frame":null},{"x":[2000,2010,2011,2012,2013,2014,2015,2016,2017],"y":["    36.6","    43.1","    43.2","    43.79703","    44.5","    45.3","    45.9","    46.7","    47.65748"],"type":"scatter","mode":"lines","name":"Proportion of foreigners (in %)","yaxis":"y2","line":{"fillcolor":"rgba(214,39,40,1)","color":"#8B0000"},"hoverinfo":["text","text","text","text","text","text","text","text","text"],"text":["    36.6","    43.1","    43.2","    43.79703","    44.5","    45.3","    45.9","    46.7","    47.65748"],"xaxis":"x","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1}},"base_url":"https://plot.ly"},"evals":["config.modeBarButtonsToAdd.0.click"],"jsHooks":{"render":[{"code":"function(el, x) { var ctConfig = crosstalk.var('plotlyCrosstalkOpts').set({\"on\":\"plotly_click\",\"persistent\":false,\"dynamic\":false,\"selectize\":false,\"opacityDim\":0.2,\"selected\":{\"opacity\":1}}); }","data":null}]}}</script><!--/html_preserve-->
© STATEC / CTIE (http://www.statistiques.public.lu/en/support/notice/index.html#copyright)

