##  scraping _day3

---

#### # 탐탐

```
#탐탐
# 서브 페이지에 데이터가 있을 경우에 메인페이지에서 태그를 못찾음.
remDr <- remoteDriver(remoteServerAddr = "localhost" , port = 4445, browserName = "chrome")
remDr$open()
#main URL
url1 <- 'https://www.tomntoms.com/store/storeList.php'
remDr$navigate(url1)
#iframe URL(참고용)
#url2 <- 'https://www.tomntoms.com/store/storeList_frame.php?'
#remDr$navigate(url2)


## (태그확인해)지역클릭도 iframe 안에서 작동해야하기때문에 frame 먼저 switch해줘야함
remDr$switchToFrame(0)

#지역 클릭
remDr$findElement(using='css',"#sltSIDO")$clickElement()
Sys.sleep(1)

#서울클릭
remDr$findElement(using='css',"#sltSIDO > option:nth-child(10)")$clickElement()
Sys.sleep(1)

#매장찾기 클릭
remDr$findElement(using='css',"#sf > fieldset > div.searchForm > div > a")$clickElement()
Sys.sleep(1)


slist <- NULL

for(i in 1:10){ # 페이지수
    nextCss <- paste0("#content > div.paging > span:nth-child(",i,")")    # 개별페이지            
    nextListLink<-remDr$findElement(using='css',nextCss)
    
    if(length(nextListLink) == 0)   break;
    nextListLink$clickElement()
    Sys.sleep(1)
    
  slist2 <- remDr$findElements(using='css', '#content > div.localList > table > tbody')
  slist2 <- sapply(slist2,function(x){x$getElementText()})
  slist <- append(slist, unlist(slist2))

  Sys.sleep(1)
}
  ## 다음 10페이지 클릭
  pageLink <- remDr$findElement(using='css',"#content > div.paging > span.btn_page.page_next")
  pageLink$clickElement() 

  for(i in 2:3){ # 11~12 페이지 끌어오기, 인덱스가 다르게 시작
    nextCss <- paste0("#content > div.paging > span:nth-child(",i,") > a")    # 개별페이지            
    nextListLink<-remDr$findElement(using='css',nextCss)
    
    if(length(nextListLink) == 0)   break;
    nextListLink$clickElement()
    Sys.sleep(1)
    
    slist3 <- remDr$findElements(using='css', '#content > div.localList > table > tbody')
    slist3 <- sapply(slist3,function(x){x$getElementText()})
    slist <- append(slist, unlist(slist3))
    
    Sys.sleep(1)
  }  
  

strsplit(unlist(slist), '\n') -> tlist
unlist(tlist) -> tlist
length(tlist) -> tlen

tlist[1:tlen*3] -> col3   # 전화번호
tlist[1:tlen*3-1] -> col2 # 주소
tlist[1:tlen*3-2] -> col1 # 매장명

tomtomlist <- data.frame(col1[1:119],col2[1:119])

write.csv(tomtomlist, "tomntoms.csv")

getwd()

```



#### 폴바셋

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





