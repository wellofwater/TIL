# 데이터 가져오기



### 01. 엑셀

```R
> library(readxl)
> ex1 <- read_excel("data_ex.xls")
> ex1
# A tibble: 10 x 4
      ID SEX     AGE AREA 
   <dbl> <chr> <dbl> <chr>
 1     1 F        50 서울 
 2     2 M        40 경기 
 3     3 F        28 제주 
 4     4 M        50 서울 
 5     5 M        27 서울 
 6     6 F        23 서울 
 7     7 F        56 경기 
 8     8 F        47 서울 
 9     9 M        20 인천 
10    10 F        38 경기
```

```R
# exel → csv 파일
> library(readxl)
> ex4 <- read.csv("data_ex.csv", encoding = "UTF-8", header = T, sep = ",", stringsAsFactors = F)
> ex4
```





### 02. 텍스트문서

```R
library(readxl);
# header O
> ex1 <- read.table(file = "mydata.txt", encoding="UTF-8", header = T, sep = ",", stringsAsFactors = F)
> ex1
    id name age
1 id01  lee  10
2 id02  kim  20

# header O, 한글 포함
> ex2 <- read.table(file = "mydata2.txt", fileEncoding ="UTF-8", header = T, sep = ",", stringsAsFactors = F)
> ex2
    id name age
1 id01   이  10
2 id02   김  20

# header X
> ex3 <- read.table("data_exx.TXT", encoding="UTF-8", skip=1);
> colnames(ex2) <- c("ID", "NAME", "AGE")
> ex3
    ID NAME AGE
1 id01  lee  10
2 id02  kim  29
3 id03 hong  30
4 id04   oh  40
```

