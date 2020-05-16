### 위,경도 변환 작업_day6

---

```r
getwd()
library(RSelenium)
library(data.table)
install.packages("ggmap")
install.packages("dplyr")
library(ggmap)
library(dplyr)
register_google(key='AIzaSyD8k2DWC_7yFHCrH6LDR3RfITsmWMEqC8c')
## 약국
df<-read.csv("pharmacy.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df2<-NULL
df3<-NULL
df<-df[1:2]
colnames(df)=c("name","addr")
head(df$addr)

for(i in 1:length(df$addr)){
  latlon <- geocode(enc2utf8(as.character(df$addr[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat,mk$lon)
  print(cen)
  print(i)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lan,lat)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
View(total)
total<-total[,c(2:5)]

colnames(total)=c("storename","storeaddr","storelat","storelng")
View(total)

write.csv(total,"lanlat_pharmacy.csv")
```

