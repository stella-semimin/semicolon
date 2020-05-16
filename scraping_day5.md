## 스크래핑 & 위도경도변환

---

```
# 롯데리아 페이지 들어가기
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("https://map.naver.com/v5/search/%EC%84%9C%EC%9A%B8%EC%8B%9C%20%EB%A1%AF%EB%8D%B0%EB%A6%AC%EC%95%84?c=14122820.6487108,4513159.8413556,14,0,0,0,dh")

seoul<-NULL
search<-NULL
city<-NULL
node1<-NULL
node2<-NULL
total<-NULL
total1<-NULL
total2<-NULL
limit<-20
region<-NULL
nextbutton<-NULL


#팝업 닫기 클릭
Sys.sleep(3)
popup<- remDr$findElement(using='css',"#intro_popup_close > span")
popup$clickElement()
Sys.sleep(3)



for(j in 1:10){
  for(i in 1:20){ # 1 page
    Sys.sleep(1)
    #name_css <- paste0("#container > div.router-output > shrinkable-layout > search-layout > search-list > search-list-contents > perfect-scrollbar > div > div.ps-content > div > div > div > search-item-place:nth-child(",i,") > div")
    name_css <- paste0("#container > div.router-output > shrinkable-layout > search-layout > search-list > search-list-contents > perfect-scrollbar > div > div.ps-content > div > div > div > search-item-place:nth-child(",i,") > div > div.search_box > div.title_box > strong > span")
    dom_name<-remDr$findElements(using='css', name_css)
    node1<-sapply(dom_name,function(x){x$getElementText()})
    print(node1)
    
    add_css <- paste0("#container > div.router-output > shrinkable-layout > search-layout > search-list > search-list-contents > perfect-scrollbar > div > div.ps-content > div > div > div > search-item-place:nth-child(",i,") > div > div.search_box > div:nth-child(3) > span")
    dom_add <-remDr$findElements(using='css',add_css)
    node2<-sapply(dom_add,function(x){x$getElementText()})
    print(node2)
    
    total<-data.frame(rbind(total, cbind(node1,node2)))
    
    # 스크롤발생
    if(i %%5 ==0 && i!=limit){
      remDr$executeScript(
        "var dom = document.querySelectorAll('#container > div.router-output > shrinkable-layout > search-layout > search-list > search-list-contents > perfect-scrollbar > div > div.ps-content > div > div > div > search-item-place')[arguments[0]];
    dom.scrollIntoView();",list(i))
      Sys.sleep(1)
    }
    else if(i==limit){
      break;
    }
    
  }
  
  View(total)
  
  #다음 버튼 클릭
  nextbutton<- remDr$findElement(using='css',"#container > div.router-output > shrinkable-layout > search-layout > search-list > search-list-contents > div > div.pagination_area.search_pagination.ng-tns-c133-4 > button.btn_next.ng-tns-c133-4")
  nextbutton$clickElement()
  Sys.sleep(2)
}



write.csv(data.frame(unlist(total$node1),unlist(total$node2)),"lotteria.csv")


## 버거킹 위도경도 

df<-read.csv("lotteria.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df2<-NULL
df3<-NULL
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

write.csv(total,"lanlat_lotteria.csv")




# 맘스터치 페이지 들어가기
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("http://www.momstouch.co.kr/sub/store/store_01_list.html?pg=1&area=%BC%AD%BF%EF&ss=")

seoul<-NULL
search<-NULL
city<-NULL
node1<-NULL
node2<-NULL
total<-NULL
region<-NULL
name_css<-NULL
add_css<-NULL

for(j in 1:13){
  for(i in 2:11){ # 1 page
    Sys.sleep(1)
    name_css <- paste0("#contents > div.content > table > tbody > tr:nth-child(",i,") > td:nth-child(2) > a")
    #contents > div.content > table > tbody > tr:nth-child(11) > td:nth-child(2) > a
    dom_name<-remDr$findElements(using='css', name_css)
    node1<-sapply(dom_name,function(x){x$getElementText()})
    print(node1)
    
    add_css <- paste0("#contents > div.content > table > tbody > tr:nth-child(",i,") > td.td_Left > a")
    dom_add <-remDr$findElements(using='css',add_css)
    node2<-sapply(dom_add,function(x){x$getElementText()})
    print(node2)
    
    total<-data.frame(rbind(total, cbind(node1,node2)))
    
    
  }
  #다음 버튼 클릭
  nextbutton<- remDr$findElement(using='css',"#contents > div.content > div.paginate > a.page_next ")
  nextbutton$clickElement()
  Sys.sleep(2)
}

#str(total)
View(total)

write.csv(data.frame(unlist(total$node1),unlist(total$node2)),"momstouch.csv")

## 맘스터치 위도경도 

df<-read.csv("momstouch.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df2<-NULL
df3<-NULL
colnames(df)=c("","name","addr")
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

write.csv(total,"lanlat_momstouch.csv")


# 버거킹 페이지 들어가기
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("https://www.burgerking.co.kr/#/store")
remDr$navigate("https://www.burgerking.co.kr/#/store")

seoul<-NULL
search<-NULL
city<-NULL
node<-NULL
node1<-NULL
node2<-NULL
total<-NULL
region<-NULL
name_css<-NULL
add_css<-NULL

#지역 버튼 클릭
region<- remDr$findElement(using='css',"#app > div > div.contentsWrap > div.contentsBox01.nopadding > div > div.map_searchWrap > div.map_search_head > div.tab01 > ul > li:nth-child(3) > button > span")
region$clickElement()

#서울 버튼 클릭
seoul<- remDr$findElement(using='css',"#app > div > div.contentsWrap > div.contentsBox01.nopadding > div > div.map_searchWrap > div.map_search_head > div.searchWrap > div:nth-child(5) > div > select:nth-child(1) > option:nth-child(2)")
seoul$clickElement()


for(i in 1:111){ # 1 page
    #node 버튼 클릭
    print(i)
    nodeclick <- paste0("#app > div > div.contentsWrap > div.contentsBox01.nopadding > div > div.map_searchWrap > div.container01.search_result > ul > li:nth-child(",i,") > button")
    node<- remDr$findElement(using='css', nodeclick)
    node$clickElement()
    
    Sys.sleep(2)
    name_css <- paste0("#app > div > div.contentsWrap > div.popWrap.m_FullpopWrap > div > div.M_headerWrap > div > h1 > span")
    #contents > div.content > table > tbody > tr:nth-child(11) > td:nth-child(2) > a
    dom_name<-remDr$findElements(using='css', name_css)
    node1<-sapply(dom_name,function(x){x$getElementText()})
    print(node1)
    
    add_css <- paste0("#app > div > div.contentsWrap > div.popWrap.m_FullpopWrap > div > div.popcont.pd03 > div > div > dl:nth-child(1) > dd > span:nth-child(1)")
    dom_add <-remDr$findElements(using='css',add_css)
    node2<-sapply(dom_add,function(x){x$getElementText()})
    print(node2)
    
    #확인 버튼 클릭
    node<- remDr$findElement(using='css',"#app > div > div.contentsWrap > div.popWrap.m_FullpopWrap > div > div.pop_btn.full_type > button")
    node$clickElement()
    
    Sys.sleep(2)
    
    total<-data.frame(rbind(total, cbind(node1,node2)))
    
    
  }


#str(total)
View(total)
colnames(total)=c("storename","storeaddr")

write.csv(data.frame(unlist(total$storename),unlist(total$storeaddr)),"burgerking.csv")

## 버거킹 위도경도 

df<-read.csv("burgerking.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df2<-NULL
df3<-NULL
colnames(df)=c("","name","addr")
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

write.csv(total,"lanlat_burgerking.csv")


## 맥도날드 위도경도 

df<-read.csv("mc.csv") 
df<-as.data.table(df)
head(df)
mk <-NULL
total<-NULL
lan<-NULL
lat<-NULL
df2<-NULL
df3<-NULL
colnames(df)=c("","name","addr")
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

write.csv(total,"lanlat_mc.csv")


```

