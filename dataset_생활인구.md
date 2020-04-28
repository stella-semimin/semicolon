### 데이터전처리 day1 (서울시 생활인구)

---

```R
library(readxl)
code<-read.csv("C:/semi/project/seoulcode.csv", stringsAsFactors = F)
people<-read.csv("C:/semi/project/people.csv", stringsAsFactors = F)
library(dplyr)

hello<-full_join(code, people, by = c("코드" = "자치구코드"))

codeandpeople<-hello
write.csv(hello,"C:/semi/project/peopleandcode.csv")

df2 <- hello %>% select("자치구.명","코드","기준일ID","시간대구분","총생활인구수")
df3<-df2 %>% group_by(자치구.명) %>% summarise(총생활인구수=sum(총생활인구수))


# option 1
#(0~7/ 20~23)
# 밤 인구
# 자치구별로 나누고, 오전0시~8시까지 뽑아냄. 시간당으로 나눠서 평균인구로 보여줌
df5<-df2 %>% group_by(자치구.명) %>% filter(시간대구분<8) %>% summarise(총생활인구수평균=sum(총생활인구수)/8) %>% arrange(총생활인구수평균) 
# 자치구별로 나누고, 오후8시부터 자정까지 뽑아냄. 시간당으로 나눠서 평균인구로 보여줌
df6<-df2 %>% group_by(자치구.명) %>% filter(시간대구분>=20) %>% summarise(총생활인구수평균=sum(총생활인구수)/4) %>% arrange(총생활인구수평균)

## 낮인구
df7<-df2 %>% group_by(자치구.명) %>% filter(시간대구분>=8 & 시간대구분<=19) %>% summarise(총생활인구수평균=sum(총생활인구수)/12) %>% arrange(총생활인구수평균) 

#(0~7/ 20~23)
# 밤 인구
# 자치구별로 나누고, 오전0시~8시까지 뽑아냄. 시간당으로 나눠서 평균인구로 보여줌
df11<-df2 %>% group_by(자치구.명) %>% filter(시간대구분<8 | 시간대구분>=20) %>% summarise(총생활인구수평균=sum(총생활인구수)/12) %>% arrange(총생활인구수평균) 
df12<-data.frame(df11, df7)
View(df12)
write.csv(df12,"C:/semi/project/peopleresult1.csv")



----------------------------------------------------------------------------
# option 2

#(0~8, 21~23)
# 밤 인구
# 자치구별로 나누고, 오전0시~8시까지 뽑아냄. 시간당으로 나눠서 평균인구로 보여줌
#df8<-df2 %>% group_by(자치구.명) %>% filter(시간대구분<9) %>% summarise(총생활인구수평균=sum(총생활인구수)/8) %>% arrange(총생활인구수평균) 
# 자치구별로 나누고, 오후8시부터 자정까지 뽑아냄. 시간당으로 나눠서 평균인구로 보여줌
#df9<-df2 %>% group_by(자치구.명) %>% filter(시간대구분>=21) %>% summarise(총생활인구수평균=sum(총생활인구수)/4) %>% arrange(총생활인구수평균)

# 낮 인구
#df10<-df2 %>% group_by(자치구.명) %>% filter(시간대구분>=9 & 시간대구분<=20) %>% summarise(총생활인구수평균=sum(총생활인구수)/8) %>% arrange(총생활인구수평균) 



```

