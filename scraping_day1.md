### 동적크롤링

---

##### 서울 내 맥도날드 뽑아오기 

```R
# 맥도날드 페이지 들어가기
install.packages("RSelenium")
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("https://www.mcdonalds.co.kr/kor/store/list.do")

MC<-NULL
MC1<-NULL
MC2<-NULL
mctotal<-NULL

repeat {
  for (j in 1:10) { # list               
    nextCss <- paste("#container > div.content > div.contArea > div > div > div.mcStore > div > span > a:nth-child(",j,")", sep="")    # 개별페이지            
    try(nextListLink<-remDr$findElement(using='css',nextCss))
    
    if(length(nextListLink) == 0)   break;
    nextListLink$clickElement()
    Sys.sleep(1)

  
#한 페이지 노드 추출; 매장명 + 도로명 주소
    for(i in 1:5){ # 1 page
  name_css <- paste0("#container> div.content > div.contArea > div > div > div.mcStore > table > tbody > tr:nth-child(",i,") > td.tdName > dl > dt > strong > a")
  dom_name<-remDr$findElements(using='css', name_css)
  MC1<-sapply(dom_name,function(x){x$getElementText()})
  print(MC1)

  add_css <- paste0("#container > div.content > div.contArea > div > div > div.mcStore > table > tbody > tr:nth-child(",i,") > td.tdName > dl > dd.road")
  dom_add <-remDr$findElements(using='css',add_css)
  MC2 <-sapply(dom_add,function(x){x$getElementText()})
  print(MC2)
  
  mctotal<-data.frame(rbind(mctotal, cbind(MC1,MC2)))
  
  } 
  }
  ## 다음 10페이지 클릭
  pageLink <- remDr$findElement(using='css',"#container > div.content > div.contArea > div > div > div.mcStore > div > a.arrow.next")
  pageLink$clickElement()
}

View(mctotal)
str(mctotal)
store_name<-unlist(mctotal$MC1)
store_add<-unlist(mctotal$MC2)
write.csv(data.frame(store_name,store_add),"mcdonald.csv")


# 맥도날드 파일 불러오기
mctotal1<-read.csv("mcdonald.csv")
mctotal1$store_add

mctotal2<-mctotal1[grep("서울",mctotal1$store_add),]
#view로 확인해서 서울아닌 매장 삭제
View(mctotal2)
mctotal2<-mctotal2[-24,]

store_name2<-unlist(mctotal2$store_name)
store_addr2<-unlist(mctotal2$store_add)
write.csv(data.frame(store_name2,store_addr2),"seoul_mcdonald.csv")
```



##### CU 서울 내 매장끌어오기

```R
# cu 페이지 들어가기
install.packages("RSelenium")
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("http://cu.bgfretail.com/store/list.do?category=store")

#서울지역으로 클릭
seoul<- remDr$findElement(using='css',"#sido > option:nth-child(3)")
seoul$clickElement()
#contents > div.relCon > div > div.search_store > div.search_wrap > table > tbody > tr:nth-child(1) > td:nth-child(3) > div > input[type=image]:nth-child(1)
#contents > div.relCon > div > div.search_store > div.search_wrap > table > tbody > tr:nth-child(1) > td:nth-child(3) > div > input[type=image]:nth-child(2)
search<- remDr$findElement(using='css',"#contents > div.relCon > div > div.search_store > div.search_wrap > table > tbody > tr:nth-child(1) > td:nth-child(3) > div > input[type=image]:nth-child(1)")
search$clickElement()
Sys.sleep(1)

CU1<-NULL
CU2<-NULL
CUTOTAL<-NULL
repeat {
  for (j in 3:7) { # list               
    nextCss <- paste("#paging > ul > a:nth-child(",j,")", sep="")    # 개별페이지            
    try(nextListLink<-remDr$findElement(using='css',nextCss))
    
    if(length(nextListLink) == 0)   break;
    nextListLink$clickElement()
    Sys.sleep(3)

  
#한 페이지 노드 추출; 매장명 + 도로명 주소
    for(i in 1:5){ # 1 page
      name_css <- paste0("#result_search > div.result_store > div.detail_store > table > tbody > tr:nth-child(",i,") > td:nth-child(1) > span.name")
      dom_name<-remDr$findElements(using='css', name_css)
      cu1<-sapply(dom_name,function(x){x$getElementText()})
      print(cu1)
    
      add_css <- paste0("#result_search > div.result_store > div.detail_store > table > tbody > tr:nth-child(",i,") > td:nth-child(2) > div > address > a")
      dom_add <-remDr$findElements(using='css',add_css)
      cu2 <-sapply(dom_add,function(x){x$getElementText()})
      print(cu2)
      
      CUTOTAL<-data.frame(rbind(CUTOTAL, cbind(cu1,cu2)))
      
  } 
}
  ## 다음 10페이지 클릭
  pageLink <- remDr$findElement(using='css',"#paging > ul > a:nth-child(8)")
  pageLink$clickElement()
  Sys.sleep(3)
}

View(CUTOTAL)
str(CUTOTAL)

store_name<-unlist(CUTOTAL$cu1)
store_add<-unlist(CUTOTAL$cu2)
write.csv(data.frame(store_name,store_add),"cu.csv")


```



##### lottemart 끌어오기( 이건 안끌어와져서 계속 확인중)

```R
# lottemart 페이지 들어가기
install.packages("RSelenium")
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("http://company.lottemart.com/bc/branch/main.do?menuCd=BM0201")

#서울지역으로 클릭
search<- remDr$findElement(using='css',"#txtRegnNm")
search$clickElement()
Sys.sleep(1)


seoul<- remDr$findElement(using='css',"#inpBC0101")
seoul$clickElement()
Sys.sleep(1)

lotte1<-NULL
lotte2<-NULL
lottetotal<-NULL


repeat {
  #contents > p > a:nth-child(2)
  for (j in 1:3) { # list               
    nextCss <- paste("#contents > p > a:nth-child(",i,")", sep="")    # 개별페이지            
    try(nextListLink<-remDr$findElement(using='css',nextCss))
    
    if(length(nextListLink) == 0)   break;
    nextListLink$clickElement()
    Sys.sleep(1)
    
    
    #한 페이지 노드 추출; 매장명 + 도로명 주소
    for(i in 1:5){ # 1 page
      i=1
      name_css <- paste0("#contents > ul > li > div > div.article1 > h3")
      
      #contents > ul > li:nth-child(1) > div > div.article1 > h3
      dom_name<-remDr$findElements(using='css', name_css)
      lotte1<-sapply(dom_name,function(x){x$getElementText()})
      print(lotte1)
      
      add_css <- paste0("#contents > ul > li:nth-child(",i,") > div > div.article2 > ul:nth-child(4) > li:nth-child(1) > span")
      dom_add <-remDr$findElements(using='css',add_css)g 
      lotte2 <-sapply(dom_add,function(x){x$getElementText()})
      print(lotte2)
      
      lottetotal<-data.frame(rbind(CUTOTAL, cbind(lotte1,lotte2)))
      
    } 
  }
  
}

View(lottetotal)
str(lottetotal)
store_name<-unlist(lottetotal$lotte1)
store_add<-unlist(lottetotal$lotte2)
write.csv(data.frame(store_name,store_add),"lottemart.csv")

```



 ##### 메가박스(주소는 끌어왔으나, 매장명 추가로 확인하기_내일오전에 끝내기)

```r
# megabox 페이지 들어가기
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("https://www.megabox.co.kr/theater/list")

megabox1<-NULL
megabox2<-NULL
megaboxtotal<-NULL

for(i in 1:20){

nextCss<- paste0("#contents > div.inner-wrap > div.theater-box > div.theater-place > ul > li.on > div > ul > li:nth-child(",i,") > a")
dom_name<-remDr$findElement(using='css', nextCss)
dom_name$clickElement()
Sys.sleep(1)

dom_addr<-remDr$findElements(using='css', "#tab01 > ul:nth-child(9) > li")
megabox2<-sapply(dom_addr,function(x){x$getElementText()})
print(megabox2)
Sys.sleep(1)
megaboxtotal<-c(megaboxtotal,unlist(megabox2))

Sys.sleep(1)
remDr$navigate("https://www.megabox.co.kr/theater/list")
}
megabox3<-list(megaboxtotal)
View(megaboxtotal)


```

