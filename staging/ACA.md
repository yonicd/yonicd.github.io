# ACA
Jonathan Sidi  
September 24, 2017  



![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/GettyImages_846510344.0.jpg)
![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_pie.png)

ACA and Graham-Cassidy federal spending by state. 

```r
kf_spending <- (xml2::read_html('http://www.kff.org/health-reform/issue-brief/state-by-state-estimates-of-changes-in-federal-spending-on-health-care-under-the-graham-cassidy-bill/')%>%
  rvest::html_table())[[2]][-c(1,2,3,55),]

names(kf_spending) <- c('Location','ACA','GC','DIFF','PCT')

kf_spending$Location[which(kf_spending$Location=='DC')]='District of Columbia'

kf_spending <- kf_spending%>%mutate_at(.vars=vars(ACA,GC,DIFF,PCT),.funs=funs(as.numeric(gsub('[,%]','',.))))
```

ACA medicare expansion by state


```r
#http://www.kff.org/health-reform/state-indicator/state-activity-around-expanding-medicaid-under-the-affordable-care-act/?currentTimeframe=0&sortModel=%7B%22colId%22:%22Location%22,%22sort%22:%22asc%22%7D
kf_expansion <- read.csv('~/Desktop/kf2_raw_data.csv',stringsAsFactors = FALSE,skip = 2)
kf_expansion <- kf_expansion[-c(1,53:61),-3]
names(kf_expansion)[2] <- 'Expansion'
```

Population of ACA enrollment by state.


```r
#http://www.kff.org/health-reform/state-indicator/total-marketplace-enrollment/?currentTimeframe=0&sortModel=%7B%22colId%22:%22Total%20Individuals%20Who%20Have%20Selected%20a%20Marketplace%20Plan%22,%22sort%22:%22asc%22%7D
kf_marketplace_pop <- read.csv('~/Desktop/kf_raw_data.csv',stringsAsFactors = FALSE,skip = 2)
kf_marketplace_pop <- kf_marketplace_pop[-c(1,53:59),]
names(kf_marketplace_pop)[c(2,3)] <- c('Marketplace_Type','N')
```

2016 general election popular vote by state, to get characteristic of political leaning of states. 


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

wiki_pop <- (xml2::read_html('https://en.wikipedia.org/wiki/List_of_U.S._states_and_territories_by_population')%>%
        rvest::xml_nodes(xpath='//*[@id="mw-content-text"]/div/table[1]')%>%
        rvest::html_table())[[1]][-c(30,53:64),c(3,4)]

names(wiki_pop) <- c('Location','Total_N')

wiki_pop$Total_N <- as.numeric(gsub('[,]','',wiki_pop$Total_N))
```

Join all the data sets


```r
kf <- kf_marketplace_pop%>%
  left_join(kf_expansion,by='Location')%>%
  left_join(wiki_pop,by='Location')

kf <- kf_spending%>%left_join(kf, by = c('Location'))%>%
  mutate(ratio_ACA=1000*ACA/N,ratio_GC=1000*GC/N,ratio_DIFF = ratio_GC-ratio_ACA,
         tot_ratio_ACA=1000*ACA/Total_N,tot_ratio_GC=1000*GC/Total_N,tot_ratio_DIFF = tot_ratio_GC-tot_ratio_ACA)%>%
  arrange(desc(ratio_DIFF))

kf <- kf%>%left_join(wiki_elections,by='Location')

kf$Expansion <- sprintf('Medicaid Expansion %s',kf$Expansion)

kf$Location <- factor(kf$Location,levels = kf$Location)

kf$Marketplace_Type <- factor(kf$Marketplace_Type,labels=c('Federally-Facilitated Market','State-based Marketplace','State-based Marketplace (using HealthCare.gov)'))
```


```r
boxplot_dat <- kf%>%
  dplyr::select(Expansion,Marketplace_Type,ratio_DIFF,tot_ratio_DIFF)%>%
  reshape2::melt(.,id=c('Marketplace_Type','Expansion'))

levels(boxplot_dat$variable) <- c('per 1,000 Individuals who have\nselected a market based plan','per 1,000 Individuals')

  boxplot_dat%>%
    ggplot(aes(x=Expansion,
             y=value,
             fill=Marketplace_Type))+
  geom_boxplot()+
  geom_hline(aes(yintercept=0),linetype=2)+
  facet_wrap(~variable,ncol=1,scales='free_y')+
  labs(title='Change in Federal Funds ACA vs Graham-Cassidy, 2020-2026',
         y='Change in Federal Funds ($ Millions) per 1,000 individuals')+
  theme_bw()+
  theme(legend.position = 'bottom')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


```r
p <- kf%>%ggplot(aes(x=Location,xend=Location,yend=ratio_GC,y=ratio_ACA,colour=ratio_DIFF))+
  geom_segment(arrow = arrow(length = unit(0.02, "npc")))+
  coord_flip()+
  scale_colour_gradient(low = 'red',high = 'blue',name='Difference')+
  labs(title='Change in Federal Funds per 1,000 Individuals who have\nselected a market based plan ACA vs Graham-Cassidy, 2020-2026',
       subtitle='Arrow pointing to movement from ACA to Graham-Cassidy',
       caption='Source: Kaiser Family Foundation',
       y='Federal Funds ($ Millions) per 1,000 individuals who have selected a market based plan')+
  theme_bw()+
  theme(legend.position = 'bottom')
```


```r
p + facet_wrap(~ TrumpWin , scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


```r
p + facet_wrap(~ Expansion , scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


```r
p + facet_wrap(~ Expansion + TrumpWin , scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


```r
p + facet_wrap(~ Marketplace_Type, scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


```r
p + facet_wrap(~ Expansion + Marketplace_Type , scales='free_y')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


```r
states_facet <- state_ranks%>%left_join(kf%>%rename(name=Location),by='name')
```

```
## Warning: Column `name` joining character vector and factor, coercing into
## character vector
```

```r
states_facet$Expansion <- factor(states_facet$Expansion,labels=c('Expansion','No Expansion'))

states_facet$tile_lbl <- sprintf('%s\n%s',states_facet$Expansion,states_facet$TrumpWin)
```


```r
states_facet%>%
  ggplot(aes(x='1', y='1',fill=tot_ratio_DIFF)) +
  geom_tile() +
  geom_text(aes(label=tile_lbl),size=2)+
  theme_bw() +
  facet_geo( ~ state)+
  scale_fill_gradient2(low='red',mid='white',high='green',name='Difference') +
  theme(legend.position = 'bottom',
        axis.text = element_blank(),
        axis.title = element_blank(),
        axis.ticks = element_blank())+
  labs(title='Change in Federal Funds per 1,000 Individuals, 2020-2026',
       caption='Source: Kaiser Family Foundation')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


```r
states_facet%>%
  ggplot(aes(x='1', y='1',fill=ratio_DIFF)) +
  geom_tile() +
  geom_text(aes(label=tile_lbl),size=2)+
  theme_bw() +
  facet_geo( ~ state)+
  scale_fill_gradient2(low='red',mid='white',high='green',name='Difference') +
  theme(legend.position = 'bottom',
        axis.text = element_blank(),
        axis.title = element_blank(),
        axis.ticks = element_blank())+
  labs(title='Change in Federal Funds per 1,000 Individuals who have\nselected a market based plan ACA vs Graham-Cassidy, 2020-2026',
       caption='Source: Kaiser Family Foundation')
```

![](https://raw.githubusercontent.com/yonicd/yonicd.github.io/master/staging/ACA_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
