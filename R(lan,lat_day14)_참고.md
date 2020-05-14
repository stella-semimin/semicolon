## R수업(day14) _  주소 -> 위도 경도 받아오는거 참고 

- java로 구현하면 데이터프레임 그대로 받아오기 힘듬

- R은 데이터 프레임 자체로 받아와서 실행하는데 특화된 언어

- 오히려 위도경도는 R로 받아오고, 나중에 저장해서 java로 끌어온뒤에 시각화하는게 좋음 _by.쌤

  

```
# 지도 시각화
install.packages("ggmap")
library(ggmap)
register_google(key='AIzaSyD8k2DWC_7yFHCrH6LDR3RfITsmWMEqC8c')

lon <- 126.9221322
lat <- 37.5268831
cen <- c(lon,lat)
mk <- data.frame(lon=lon, lat=lat)
map <- get_googlemap(center=cen, maptype="roadmap",zoom=1, marker=mk)
# center에 위도,경도 정보를 vector로/일반적인지도=roadmap/marker에는 dataframe으로
# zoom = 1이면 세계지도를 기준으로
Sys.sleep(1)
ggmap(map)
map <- get_googlemap(center=cen, maptype="roadmap",zoom=5, marker=mk)
# zoom =5이면 우리나라 전체
Sys.sleep(1)
ggmap(map)
# ggmap() = 실제 지도를 그려주는 함수임
map <- get_googlemap(center=cen, maptype="roadmap",zoom=10, marker=mk)
# zoom = 10이면, 서울+경기권정도
Sys.sleep(1)
ggmap(map)
map <- get_googlemap(center=cen, maptype="roadmap",zoom=15, marker=mk)
Sys.sleep(1)
ggmap(map)
map <- get_googlemap(center=cen, maptype="satellite",zoom=16, marker=mk)
# maptype = "satellite" ; 위성지도
Sys.sleep(1)
ggmap(map)
map <- get_googlemap(center=cen, maptype="terrain",zoom=8, marker=mk)
# maptype="terrain" ; 지형지도
Sys.sleep(1)
ggmap(map)
map <- get_googlemap(center=cen, maptype="terrain",zoom=12, marker=mk)
Sys.sleep(1)
ggmap(map)
map <- get_googlemap(center=cen, maptype="hybrid",zoom=16, marker=mk)
Sys.sleep(1)
ggmap(map)+labs(title="테스트임", x="경도", y="위도")

# get_map지도 그리는데 필요한 자원을 요청해서 받아오는 함수로서
# 지도 서비스하는 기관한테 지도서비스를 받아오는걸 확장하는 함수
map <- get_map(location=cen, maptype="toner",zoom=12, marker=mk, source="google")
ggmap(map)
map <- get_map(location=cen, maptype="watercolor",zoom=12, marker=mk, source="stamen")
ggmap(map)
map <- get_map(location=cen, maptype="terrain-background",zoom=12, marker=mk, source="stamen")
# source="stamen"
ggmap(map)
map <- get_map(location=cen, maptype="roadmap",zoom=12, marker=mk, source="stamen")
ggmap(map)
map <- get_map(location=cen, maptype="terrain",zoom=12, marker=mk, source="stamen")
ggmap(map)
#map <- get_map(location=cen, maptype="roadmap",zoom=12, marker=mk, source="naver")
#ggmap(map)
#map <- get_map(location=cen, maptype="roadmap",zoom=12, marker=mk, source="osm")
#ggmap(map)


mk <- geocode("seoul", source = "google")
# source="google" 생략해도 되고, 생략하면 default= 구글임.
print(mk) # 티블객체(=데이터프레임)로 리턴, 적당하게 반올림해서 출력함
cen <- c(mk$lon, mk$lat) # 왜 벡터로 만들어? 중심점 정보는 벡터로 줘야되니까
map <- get_googlemap(center=cen, maptype="roadmap",zoom=11, marker=mk)
ggmap(map)
mk <- geocode(enc2utf8("부산"), source = "google")
# 한글로 쓰면 인식못하니까 enc2utf8("부산") 이렇게 해줘야함
cen <- c(mk$lon, mk$lat)
map <- get_googlemap(center=cen, maptype="roadmap",zoom=15, marker=mk)
ggmap(map)
multi_lonlat <- geocode(enc2utf8("강남구 역삼동 테헤란로 212"), source = "google")
mk <- multi_lonlat
cen <- c(mk$lon, mk$lat)
map <- get_googlemap(center=cen, maptype="roadmap",zoom=16)
ggmap(map) + 
  geom_point(aes(x=mk$lon, y=mk$lat), alpha=0.4, size=5, color="pink") +
  geom_text(aes(x=mk$lon, y=mk$lat, label="우리가 공부하는 곳", vjust=0, hjust=0))
# marker정보 안주면 지도만 달랑 그림
# marker정보 줄 수도 있고, 아니면 ggmap수행후 + geom_point로 나타낼수도있음
# alpha = 투명도, vjust(위아래이동) , hjust (좌우이동)


# 제주도
names <- c("용두암","성산일출봉","정방폭포",
           "중문관광단지","한라산1100고지","차귀도")
addr <- c("제주시 용두암길 15",
          "서귀포시 성산읍 성산리",
          "서귀포시 동홍동 299-3",
          "서귀포시 중문동 2624-1",
          "서귀포시 색달동 산1-2",
          "제주시 한경면 고산리 125")
gc <- geocode(enc2utf8(addr))
gc

df <- data.frame(name=names,
                 lon=gc$lon,
                 lat=gc$lat)
cen <- c(mean(df$lon),mean(df$lat))
#중심을 잡아야 zoom레벨조정이 용이함
map <- get_googlemap(center=cen,
                     maptype="roadmap",
                     zoom=10,
                     size=c(640,640),
                     marker=gc)
ggmap(map) 

ggmap(map) + geom_text(data=df,               
                       aes(x=lon,y=lat,colour="magenta"),               
                       size=3,                
                       label=df$name) + guides(color=F)
#  guides(color=F) 범례없애달라고!



# 공공 DB 활용 

install.packages("XML")
library(XML)
API_key  <- "%2BjzsSyNtwmcqxUsGnflvs3rW2oceFvhHR8AFkM3ao%2Fw50hwHXgGyPVutXw04uAXvrkoWgkoScvvhlH7jgD4%2FRQ%3D%3D"
bus_No <- "360"
url <- paste("http://ws.bus.go.kr/api/rest/busRouteInfo/getBusRouteList?ServiceKey=", API_key, "&strSrch=", bus_No, sep="")
doc <- xmlParse(url)
top <- xmlRoot(doc) ; top
df <- xmlToDataFrame(getNodeSet(doc, "//itemList[1]")); df
busRouteId <- df$busRouteId
busRouteId
url <- paste("http://ws.bus.go.kr/api/rest/buspos/getBusPosByRtid?ServiceKey=", API_key, "&busRouteId=", busRouteId, sep="")
doc <- xmlParse(url)
top <- xmlRoot(doc); top
df <- xmlToDataFrame(getNodeSet(doc, "//itemList")); df
# 구글 맵에 버스 위치 출력
df$gpsX <- as.numeric(as.character(df$gpsX))
df$gpsY <- as.numeric(as.character(df$gpsY))
gc <- data.frame(lon=df$gpsX, lat=df$gpsY);gc
cen <- c(mean(gc$lon), mean(gc$lat))
map <- get_googlemap(center=cen, maptype="roadmap",zoom=12, marker=gc)
ggmap(map)


library(dplyr)
library(ggmap)
library(ggplot2)

geocode('Seoul', source = 'google')
geocode('Seoul', source = 'google', output = 'latlona')
# output의 default= 'latlon'(위도경도만),  'latlona'에서 a는 adress(주소)
geocode(enc2utf8('서울'), source = 'google')
geocode(enc2utf8('서울'), source = 'google', output = 'latlona')
geocode(enc2utf8('서울&language=ko'), source = 'google', output = 'latlona')

#mutate_geocode(data.frame, address_column_name, source = 'google')
station_list = c('시청역', '을지로입구역', '을지로3가역', '을지로4가역', 
                 '동대문역사문화공원역', '신당역', '상왕십리역', '왕십리역', '한양대역', 
                 '뚝섬역', '성수역', '건대입구역', '구의역', '강변역', '잠실나루역', 
                 '잠실역', '신천역', '종합운동장역', '삼성역', '선릉역', '역삼역', 
                 '강남역', '2호선 교대역', '서초역', '방배역', '사당역', '낙성대역', 
                 '서울대입구역', '봉천역', '신림역', '신대방역', '구로디지털단지역', 
                 '대림역', '신도림역', '문래역', '영등포구청역', '당산역', '합정역', 
                 '홍대입구역', '신촌역', '이대역', '아현역', '충정로역')
station_df = data.frame(station_list, stringsAsFactors = FALSE)
# 데이터프레임을 만들고, euc2utf8 수행해서 넣어줘
station_df$station_list = enc2utf8(station_df$station_list)
# 다음 행은 한번만 수행시키셩
station_lonlat = mutate_geocode(station_df, station_list, source = 'google')
# geocode는 위도경도 리턴, mutate는 데이터프레임을 아규먼트로 줘야함. 
# 두번째는 주소벡터
station_lonlat
save(station_lonlat, file="station_lonlat.rda")
#load("station_lonlat.rda")

seoul_lonlat = unlist(geocode('seoul', source = 'google'))
# 데이터프레임도 list계열이라 unlist하면 네임드벡터가됨
# 원래 geocode가 리턴하는건 티블객체(= 데이터프레임의 일종)
?qmap
# qmap은 ggmap + get_map을 합쳐놓은거. 순수하게 지도만 그려줌

qmap('seoul', zoom = 11)
qmap(seoul_lonlat, zoom = 11, source = 'stamen', maptype = 'toner')
seoul_map <- qmap('Seoul', zoom = 11, source = 'stamen', maptype = 'toner')
seoul_map + geom_point(data = station_lonlat, aes(x = lon, y = lat), colour = 'green',size = 4)


```

