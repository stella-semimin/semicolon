## Scraping_day4

#### (열리지 않는 페이지는 네이버지도앱에서 정보 끌어오기)

- 네이버 지도앱은 스타벅스처럼 스크롤 내려야함(5개씩)
- 한 페이지 당 20개씩 끌어옴
- 페이지 이동은 다음 10페이지 따로 쓰지않고, 버튼 누르면 1page->2page->3page... 계속 이동



1. 투썸, 네이버지도로 받아오기 페이지 들어가기

```
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("https://map.naver.com/v5/search/%EC%84%9C%EC%9A%B8%EC%8B%9C%20%ED%88%AC%EC%8D%B8%ED%94%8C%EB%A0%88%EC%9D%B4%EC%8A%A4?c=14144282.7330590,4528676.5580975,14,0,0,0,dh")

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



for(j in 1:15){
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
  
  #다음 버튼 클릭
  nextbutton<- remDr$findElement(using='css',"#container > div.router-output > shrinkable-layout > search-layout > search-list > search-list-contents > div > div.pagination_area.search_pagination.ng-tns-c133-4 > button.btn_next.ng-tns-c133-4")
  nextbutton$clickElement()
  Sys.sleep(2)
}

str(total)
View(total)

write.csv(data.frame(unlist(total$node1),unlist(total$node2)),"twosome.csv")
```



2. 뜌레쥬르 소스2, 네이버지도로 받아오기 페이지 들어가기

```
# 뜌레쥬르 소스2, 네이버지도로 받아오기 페이지 들어가기
library(RSelenium)
remDr<-remoteDriver(remoteServerAddr= "localhost",port = 4445, browserName= "chrome")
remDr$open()
remDr$navigate("https://map.naver.com/v5/search/%EB%9C%8C%EB%A0%88%EC%A5%AC%EB%A5%B4%20%EC%84%9C%EC%9A%B8?c=14136976.4438937,4517950.0565546,14,0,0,0,dh")

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



for(j in 1:12){
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
      Sys.sleep(2)
    }
    else if(i==limit){
      break;
    }
    
  }
  
  #다음 버튼 클릭
  nextbutton<- remDr$findElement(using='css',"#container > div.router-output > shrinkable-layout > search-layout > search-list > search-list-contents > div > div.pagination_area.search_pagination.ng-tns-c133-4 > button.btn_next.ng-tns-c133-4")
  nextbutton$clickElement()
  Sys.sleep(3)
}

#str(total)
#View(total)

write.csv(data.frame(unlist(total$node1),unlist(total$node2)),"tourlesjour2.csv")
```

