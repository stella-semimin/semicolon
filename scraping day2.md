### scraping day2

---

1. 폴바셋

```
# 폴바셋 페이지 들어가기
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("https://www.baristapaulbassett.co.kr/store/Store.pb")

seoul<-NULL
search<-NULL
city<-NULL
node1<-NULL
node2<-NULL
total<-NULL
region<-NULL
name_css<-NULL
add_css<-NULL

## 지역 클릭
region <- remDr$findElement(using='css',"#storeLocationEl > a")
region$clickElement()
Sys.sleep(1)

## 서울 클릭
seoul <- remDr$findElement(using='css',"#areaList > li:nth-child(1) > a")
seoul$clickElement()
Sys.sleep(1)

## 전체 클릭
city <- remDr$findElement(using='css',"#countyList > li:nth-child(1) > a")
city$clickElement()
Sys.sleep(1)


#한 페이지 노드 추출; 매장명 + 도로명 주소
  name_css <- paste0("#searchLayer")
  dom_name<-remDr$findElements(using='css',name_css )
  node1<-sapply(dom_name,function(x){x$getElementText()})
  print(node1)
  Sys.sleep(1)

  strsplit(unlist(node1), '\n') -> elist
  
  unlist(elist) -> elist
  str(elist)
  elist[-c(1:5)] -> elist
  
  length(elist) -> elen
  elist[1:elen*2] -> col2
  elist[1:elen*2-1] -> col1
 
  length(col1) -> elen2
  col1[1:elen2*2-1] -> node3 #60개확인
  length(col2) -> elen3
  col2[1:elen3*2-1] -> node4
  
  total<-data.frame(node3[1:60], node4[1:60])
  
write.csv(total,"paul.csv")


```

##### 

2. 커피빈

```
# coffeebean 페이지 들어가기
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("https://www.coffeebeankorea.com/store/store.asp")

seoul<-NULL
search<-NULL
city<-NULL
bean1<-NULL
bean2<-NULL
beantotal<-NULL
region<-NULL
name_css<-NULL
add_css<-NULL


#지역으로 클릭
region<- remDr$findElement(using='css',"#contents2 > div.store_map > div.store_box > div.search_tab > h3.region_srh > a")
region$clickElement()
Sys.sleep(1)

#시도클릭
city<- remDr$findElement(using='css',"#localTitle")
city$clickElement()
Sys.sleep(1)

#서울지역으로 클릭
seoul<- remDr$findElement(using='css',"#storeLocal > li:nth-child(1) > a")
seoul$clickElement()
Sys.sleep(3)


for(i in 1:70){ # 1 page
  name_css <- paste0("#storeListUL > li:nth-child(",i,") > div.store_txt > p.name > span")
  dom_name<-remDr$findElements(using='css', name_css)
  bean1<-sapply(dom_name,function(x){x$getElementText()})
  print(bean1)
  Sys.sleep(1)

  add_css <- paste0("#storeListUL > li:nth-child(",i,") > div.store_txt > p.address > span")
  dom_add <-remDr$findElements(using='css',add_css)
  bean2<-sapply(dom_add,function(x){x$getElementText()})
  print(bean2)
  Sys.sleep(1)
  
  beantotal<-data.frame(rbind(beantotal, cbind(bean1,bean2)))
}

#View(beantotal)
#str(beantotal)
store_name<-unlist(beantotal$bean1)
store_add<-unlist(beantotal$bean2)
write.csv(data.frame(store_name,store_add),"coffeebean.csv")



```



3. 이디야(강서구, 중구 -- 매장수가 많아서 동정보로 끌어옴)

```
#이디야

#gugu <- c("강남구","강동구","강북구", "강서구","관악구","광진구","구로구","금천구","노원구","도봉구","동대문구",
          "동작구","마포구","서대문구","서초구","성동구","성북구","송파구","양천구","영등포구", "용산구","은평구", 
          "종로구","중구","중랑구")
junggu<-c("회현동","명동","필동","광희동","신당동","다산동","황학동","중림동")
gugu<-junggu
length(gugu)

remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
eurl <- 'https://www.ediya.com/contents/find_store.html'
remDr$navigate(eurl)

ediyalist <- NULL
for(i in 1:8){
  #storename
  jusotab <- '#contentWrap > div.contents > div > div.store_search_pop > ul > li:nth-child(2) > a'
  jusotab <- remDr$findElement(using='css',jusotab)
  jusotab$clickElement()
  
  guname <- junggu[i]
  webElem <- remDr$findElement(using = 'css', "input[id='keyword']")
  webElem$sendKeysToElement(list(guname))
  searchbtn <- '#keyword_div > form > button'
  searchbtn <- remDr$findElement(using='css',searchbtn)
  searchbtn$clickElement()
  Sys.sleep(2)
  plist <- '#placesList'
  plist <- remDr$findElements(using='css',plist)
  plist <- sapply(plist,function(x){x$getElementText()})
  strsplit(unlist(plist), '\n') -> elist
  unlist(elist) -> elist
  length(elist) -> elen
  elist[1:elen*2] -> col2
  elist[1:elen*2-1] -> col1
  
  ediyalist <- rbind(ediyalist,data.frame(name = col1[!is.na(col1)], addr = col2[!is.na(col2)]))
}

write.csv(ediyalist,"ediya3(중구).csv")

```

