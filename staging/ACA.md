---
layout: post
title: Understanding Graham-Cassidy
tags: [ggplot2, webscraping, leaflets]
---

```r
kf_spending <- (xml2::read_html('http://www.kff.org/health-reform/issue-brief/state-by-state-estimates-of-changes-in-federal-spending-on-health-care-under-the-graham-cassidy-bill/')%>%
  rvest::html_table())[[2]][-c(1,2,3,55),]

kf_spending$X1[which(kf_spending$X1=='DC')]='District of Columbia'

names(kf_spending)[1] <- 'Location'

kf_spending <- kf_spending%>%mutate_at(.vars=vars(X2,X3,X4,X5),.funs=funs(as.numeric(gsub('[,%]','',.))))
```


```r
kf_expansion <- read.csv('~/Desktop/kf2_raw_data.csv',stringsAsFactors = FALSE,skip = 2)
kf_expansion <- kf_expansion[-c(1,53:61),-3]
names(kf_expansion)[2] <- 'Expansion'
```


```r
kf_marketplace_pop <- read.csv('~/Desktop/kf_raw_data.csv',stringsAsFactors = FALSE,skip = 2)
kf_marketplace_pop <- kf_marketplace_pop[-c(1,53:59),]
names(kf_marketplace_pop)[c(2,3)] <- c('Marketplace_Type','N')
```


```r
wiki_elections <- (xml2::read_html('https://en.wikipedia.org/wiki/United_States_presidential_election,_2016')%>%
        rvest::xml_nodes(xpath='//*[@id="mw-content-text"]/div/div[40]/table')%>%
        rvest::html_table())[[1]][-c(1,58),c(1,3,6,23)]

names(wiki_elections) <- c('Location','Clinton','Trump','Total')

wiki_elections$Location[grep('^Nebraska',wiki_elections$Location)] <- 'Nebraska'
wiki_elections$Location[grep('^Maine',wiki_elections$Location)] <- 'Maine'

wiki_elections <- wiki_elections%>%
  mutate_at(.vars = vars(Clinton,Trump,Total),.funs=funs(as.numeric(gsub('[,]','',.))))%>%
  group_by(Location)%>%summarise_at(.vars = vars(Clinton,Trump,Total),.funs = funs(sum))%>%
  mutate(ClintonPct=Clinton/Total,TrumpPct=Trump/Total,TrumpWin=ifelse(TrumpPct>ClintonPct,'Trump Win','Clinton Win'))
```


```r
kf <- kf_marketplace_pop%>%left_join(kf_expansion,by='Location')

kf <- kf_spending%>%left_join(kf, by = c('Location'))%>%
  mutate(ratio_X2=1000*X2/N,ratio_X3=1000*X3/N)%>%
  mutate(ratio_X4 = ratio_X3-ratio_X2)%>%arrange(desc(ratio_X4))

kf <- kf%>%left_join(wiki_elections,by='Location')

kf$Expansion <- sprintf('Medicaid Expansion %s',kf$Expansion)
kf$Location <- factor(kf$Location,levels = kf$Location)

kf$Marketplace_Type <- factor(kf$Marketplace_Type,labels=c('Federally-Facilitated Market','State-based Marketplace','State-based Marketplace (using HealthCare.gov)'))
```


```r
p <- kf%>%ggplot(aes(x=Location,xend=Location,yend=ratio_X3,y=ratio_X2,colour=ratio_X4))+
  geom_segment(arrow = arrow(length = unit(0.02, "npc")))+
  coord_flip()+
  scale_colour_gradient(low = 'red',high = 'blue',name='Difference')+
  labs(title='Change in Federal Funds per 1,000 Individuals who have\nselected a market based plan ACA vs Graham-Cassidy, 2020-2026',
       subtitle='Arrow pointing to movement from ACA to Graham-Cassidy',
       caption='Source: Kaiser Family Foundation',
       y='Federal Funds ($ Millions) per 1,000 individuals who have selected a market based plan')+
  theme_bw()+
  theme(legend.position = 'bottom')

p + facet_wrap(~ TrumpWin , scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
p + facet_wrap(~ Expansion , scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-7-2.png)<!-- -->

```r
p + facet_wrap(~ Expansion + TrumpWin , scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-7-3.png)<!-- -->

```r
p + facet_wrap(~ Marketplace_Type, scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-7-4.png)<!-- -->

```r
p + facet_wrap(~ Expansion + Marketplace_Type , scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-7-5.png)<!-- -->


```r
states <- geojsonio::geojson_read("https://raw.githubusercontent.com/rstudio/leaflet/gh-pages/json/us-states.geojson", what = "sp")

states@data <- states@data%>%left_join(kf%>%rename(name=Location),by='name')%>%filter(name!='Puerto Rico')
```

```
## Warning: Column `name` joining factors with different levels, coercing to
## character vector
```


```r
bins <- seq(-250,250,length.out = 11)
pal <- colorBin("RdYlGn", domain = states$ratio_X4, bins = bins)

labels <- sprintf(
  "<strong>%s: $%g (Millions) </strong><br/> ACA: $%g (Millions) Funding per 1,000 individuals<br/>Graham-Cassidy: $%g (Millions) Funding per 1,000 individuals<br/>%s<br/>ACA Marketplace Type: %s<br/>Voted for Trump: %s%%",
  states$name, 
  round(states$ratio_X4,1),
  round(states$ratio_X2,1),
  round(states$ratio_X3,1),
  states$Expansion,
  states$Marketplace_Type,
  round(100*states$TrumpPct,1)
) %>% lapply(htmltools::HTML)
```


```r
leaflet(states) %>%
  setView(-96, 37.8, 4) %>%
  addProviderTiles("MapBox", options = providerTileOptions(
    id = "mapbox.light",
    accessToken = Sys.getenv('MAPBOX_ACCESS_TOKEN'))) %>%
  addPolygons(
    fillColor = ~pal(ratio_X4),
    weight = 2,
    opacity = 1,
    color = "white",
    dashArray = "3",
    fillOpacity = 0.7,
    highlight = highlightOptions(
      weight = 5,
      color = "#666",
      dashArray = "",
      fillOpacity = 0.7,
      bringToFront = TRUE),
    label = labels,
    labelOptions = labelOptions(
      style = list("font-weight" = "normal", padding = "3px 8px"),
      textsize = "10px",
      direction = "auto")) %>%
  addLegend(pal = pal, values = ~ratio_X4, opacity = 0.7, title = 'Difference',
            position = "bottomright")
```

<iframe width="854" height="480" src="https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/aca_map.html" frameborder="0" allowfullscreen></iframe>
