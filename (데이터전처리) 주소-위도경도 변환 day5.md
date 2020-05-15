## 주소->위도경도 변환 (전처리)

---

```
install.packages("data.table")
library(data.table)
library(ggmap)
library(dplyr)
register_google(key='AIzaSyD8k2DWC_7yFHCrH6LDR3RfITsmWMEqC8c')

####CGV
df<-read.csv("C:/Users/student/Desktop/project_data/cgv.csv") 
df<-as.data.table(df)


mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df3<-NULL


for(i in 1:length(df$도로명)){
  lanlat <- geocode(enc2utf8(as.character(df$도로명[i])), source = "google")
  mk <- lanlat
  cen <- c(mk$lon, mk$lat)
  print(cen)
  
  #위도
  lan<-mk$lon
  #경도
  lat<-mk$lat
  
  df2<-data.frame(lan,lat)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
total1<-total[,c(2:5)]
View(total1)

write.csv(total1,"cgv_lanlat.csv")


#### 메가박스
df<-read.csv("C:/Users/student/Desktop/project_data/megabox.csv") 
df<-as.data.table(df)
mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df2<-NULL
df3<-NULL

head(df$store_add)
for(i in 1:length(df$store_add)){
  lanlat <- geocode(enc2utf8(as.character(df$store_add[i])), source = "google")
  mk <- lanlat
  cen <- c(mk$lon, mk$lat)
  print(cen)
  
  #위도
  lan<-mk$lon
  #경도
  lat<-mk$lat
  
  df2<-data.frame(lan,lat)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
View(total)
total1<-total[,c(2:5)]
View(total1)

write.csv(total1,"megabox_lanlat.csv")

#### 롯데시네마
df<-read.csv("C:/Users/student/Desktop/project_data/lotteC.csv") 
df<-as.data.table(df)
mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df2<-NULL
df3<-NULL

head(df$도로명)
for(i in 1:length(df$도로명)){
  lanlat <- geocode(enc2utf8(as.character(df$도로명[i])), source = "google")
  mk <- lanlat
  cen <- c(mk$lon, mk$lat)
  print(cen)
  
  #위도
  lan<-mk$lon
  #경도
  lat<-mk$lat
  
  df2<-data.frame(lan,lat)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
View(total)
total1<-total[,c(2:5)]
View(total1)

write.csv(total1,"lottecinema_lanlat.csv")


#### CU
df<-read.csv("C:/Users/student/Desktop/project_data/cu.csv") 
df<-as.data.table(df)
mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df2<-NULL
df3<-NULL

head(df$store_add)
for(i in 1:length(df$store_add)){
  latlon <- geocode(enc2utf8(as.character(df$store_add[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat, mk$lon)
  print(cen)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lat,lon)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
View(total)
total1<-total[,c(2:5)]
View(total1)

write.csv(total1,"latlon_cu.csv")

#### GS편의점
df<-read.csv("C:/Users/student/Desktop/project_data/gs.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lat<-NULL
lon<-NULL
df2<-NULL
df3<-NULL

head(df$addr)
for(i in 1:length(df$addr)){
  latlon <- geocode(enc2utf8(as.character(df$addr[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat, mk$lon)
  print(cen)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lat,lon)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
View(total)
colnames(total) = c("storename","storeaddr","storelat","storelng")


write.csv(total,"latlng_gs.csv")


#### GS편의점 _ 개별확인용 
install.packages("readxl")
library(readxl)
df<-readxl::read_excel("C:/Users/student/Desktop/project_data/gs_check.xlsx") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df2<-NULL
df3<-NULL

head(df$addr)
for(i in 1:length(df$addr)){
  latlon <- geocode(enc2utf8(as.character(df$addr[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat, mk$lon)
  print(cen)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lat,lon)
}


#### 백화점통합
df<-read.csv("C:/Users/student/Desktop/project_data/백화점통합.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lat<-NULL
lon<-NULL
df2<-NULL
df3<-NULL
colnames(df) = c("storename","storeaddr")
View(df)
head(df$storeaddr)
df$storeaddr[20]

for(i in 1:length(df$storeaddr)){
  latlon <- geocode(enc2utf8(as.character(df$storeaddr[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat, mk$lon)
  print(cen)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lat,lan)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
View(total)
colnames(total) = c("storename","storeaddr","storelat","storelng")


write.csv(total,"latlng_백화점통합.csv")
getwd()
setwd("C:/Users/student/Desktop/프로젝트크롤링")

#### 마트통합
df<-readxl::read_excel("마트통합.xlsx") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lat<-NULL
lon<-NULL
df2<-NULL
df3<-NULL
colnames(df) = c("storename","storeaddr")
View(df)
head(df$storeaddr)

for(i in 1:length(df$storeaddr)){
  latlon <- geocode(enc2utf8(as.character(df$storeaddr[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat, mk$lon)
  print(cen)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lat,lan)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
View(total)
colnames(total) = c("storename","storeaddr","storelat","storelng")


write.csv(total,"latlng_마트통합.csv")

#### 폴바셋통합
df<-read.csv("paul.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lat<-NULL
lon<-NULL
df2<-NULL
df3<-NULL
colnames(df) = c("","storename","storeaddr")
View(df)
head(df$storeaddr)


for(i in 1:length(df$storeaddr)){
  latlon <- geocode(enc2utf8(as.character(df$storeaddr[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat, mk$lon)
  print(cen)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lat,lan)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
total<-total[,2:5]
View(total)
colnames(total) = c("storename","storeaddr","storelat","storelng")


write.csv(total,"latlng_paul.csv")

#### 탐탐통합
df<-read.csv("tomntoms_final.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lat<-NULL
lon<-NULL
df2<-NULL
df3<-NULL
colnames(df) = c("","storename","storeaddr")
View(df)
head(df$storeaddr)


for(i in 1:length(df$storeaddr)){
  latlon <- geocode(enc2utf8(as.character(df$storeaddr[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat, mk$lon)
  print(cen)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lat,lan)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
total<-total[,2:5]
View(total)
colnames(total) = c("storename","storeaddr","storelat","storelng")


write.csv(total,"latlng_tomntoms.csv")

#### 투썸통합
df<-read.csv("twosome.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lat<-NULL
lon<-NULL
df2<-NULL
df3<-NULL
colnames(df) = c("","storename","storeaddr")
View(df)
head(df$storeaddr)


for(i in 1:length(df$storeaddr)){
  latlon <- geocode(enc2utf8(as.character(df$storeaddr[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat, mk$lon)
  print(cen)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lat,lan)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
total<-total[,2:5]
View(total)
colnames(total) = c("storename","storeaddr","storelat","storelng")
write.csv(total,"latlng_twosome.csv")

#### 뚜레쥬르
df<-read.csv("tourlesjour_final.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lat<-NULL
lon<-NULL
df2<-NULL
df3<-NULL
colnames(df) = c("","storename","storeaddr")
View(df)
head(df$storeaddr)


for(i in 1:length(df$storeaddr)){
  latlon <- geocode(enc2utf8(as.character(df$storeaddr[i])), source = "google")
  mk <- latlon
  cen <- c(mk$lat, mk$lon)
  print(cen)
  
  #위도
  lan<-mk$lat
  #경도
  lat<-mk$lon
  
  df2<-data.frame(lat,lan)
  df3<-rbind(df3, df2)
}

total<-bind_cols(df,df3)
total<-total[,2:5]
View(total)
colnames(total) = c("storename","storeaddr","storelat","storelng")
write.csv(total,"latlng_tourlesjour.csv")

```

