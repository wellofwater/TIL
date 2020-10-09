# 데이터 타입



### 01. 스칼라

> 길이가 1인 벡터 

- 숫자 

- NA (Not Available) : 데이터 값이 없음을 의미

  ```R
  > z <- NA;
  > is.na(z)
  [1] TRUE
  
  # NA가 저장되어 있으면 TRUE, 그렇지 않으면 FALSE를 반환한다.
  ```

- NULL : 프로그래밍의 편의를 위해 미정(undefined) 값을 표현

- 문자열

- 진릿값 : `TRUE`, `T` / `FALSE`, `F`

  - &(AND), |(OR), !(NOT) 연산자 사용 가능

- 팩터 `Factor` : 범주형 - 명목형 데이터

  ```R
  # 팩터 생성
  > sex <- factor("sex", c("male", "female"))
  > sex
  [1] sex
  Levels: male female
  
  
  > nlevels(sex)	# 레벨의 개수
  [1] 2
  > levels(sex)	# 레벨의 목록
  [1] "male" "female"
  
  # 순서형 팩터 생성
  > ordered("a", c("a", "b", "c"))
  [1] a
  Levels: a < b < c
  ```






### 02. 벡터

> 배열 (한 가지 스칼라 데이터 타입으로 저장) 

- 벡터 데이터 접근 문법

  ```R
  > v1 <- c(1,2,3,4,5)
  
  # 벡터 각 셀에 이름을 부여
  > names(v1) <- c("d1", "d2", "d3")
    d1   d2   d3 <NA> <NA> 
     1    2    3    4    5 
  
  # x[start:end] : start 부터 end까지의 값을 반환
  > v1[1:3]
  d1 d2 d3 
   1  2  3 
  
  # x[색인 벡터] : 각 요소 값을 반환
  > v1[c(1,3)]
  d1 d3 
   1  3 
  > v1[c("d1", "d3")]
  d1 d3 
   1  3 
  
  # length(x): 객체의 길이를 반환
  > length(v1)
  [1] 5
  
  # NROW(x) 배열의 행/열의 수를 반환
  > NROW(v1)
  [1] 5
  ```

  

- 벡터 연산

  ```R
  # + 연산: 배열의 같은 위치 셀끼리 더한다.
  > v2 <- v1 + v1
  > v2
    d1   d2   d3 <NA> <NA> 
     2    4    6    8   10 
  
  
  ```






### 03. 리스트

> 데이터를 중간 삽입이 가능한 배열

- (key, value) 형태의 연관 배열 (Associative Array) : 해시 테이블/ 딕셔너리로 설명되기도 함. 

  ```R
  # list(key1=value1, key2=value2 ...)
  > list1 <- list(v1 = "data1", v2 = "data2")
  > list1
  $`v1`
  [1] "data1"
  
  $v2
  [1] "data2"
  
  # 출력
  > list1[1]	# list[n] : N번째 데이터의 서브리스트
  $`v1`
  [1] "data1"
  
  > list1$v1	# list$key : 데이터 값
  [1] "data1"
  
  ```






### 04. 행렬 

> 동입타입의 2차원 Matrix

- 한 가지 유형의 스칼라만 저장

  ```R
  # 행렬 생성
  matrix( 
     data,          # 행렬을 생성할 데이터 벡터 
     nrow,          # 행의 수 
     ncol,          # 열의 수 
     byrow=FALSE,   # TRUE로 설정하면 행우선, FALSE일 경우 열 우선으로 데이터를 채운다. 
     dimnames=NULL  # 행렬의 각 차원에 부여할 이름 
  )
  > m1 <- matrix(c(1, 2, 3, 4, 5, 6, 7, 8, 9), nrow=3)
  > m1
       [,1] [,2] [,3]
  [1,]    1    4    7
  [2,]    2    5    8
  [3,]    3    6    9
  
  
  # 명칭 부여 1
  > matrix(1:9, nrow=3,
  +        dimnames=list(c("r1", "r2", "r3"), c("c1", "c2", "c3")))
     c1 c2 c3
  r1  1  4  7
  r2  2  5  8
  r3  3  6  9
  # 명칭 부여 2
  > colnames(m1) <- c("C1", "C2", "C3")
  > rownames(m1) <- c("R1", "R2", "R3")
  > m1
     C1 C2 C3
  R1  1  2  3
  R2  4  5  6
  R3  7  8  9
  ```

- 행렬 연산

  ```R
  # 행 / 열의 수 구하기
  > ncol(m1)
  [1] 3
  > nrow(m1)
  [1] 3
  > dim(m1)
  [1] 3 3
  
  # 행/열별 총합, 평균
  > rowSums(m1)
  R1 R2 R3 
   6 15 24 
  > rowMeans(m1)
  R1 R2 R3 
   2  5  8 
  ```






### 05. 배열

> 다차원 행렬





### 06. 데이터 프레임

> 다중(데이터 타입) 행렬

- 데이터 프레임 생성

  ```R
  > d1 <- data.frame(name=c("kim", "lee", "seo"), ko=c(90, 80, 98), en=c(100, 78, 92), ma=c(99, 68, 88))
  # 임의의 R 객체의 내부 구조(structure)를 출력
  > str(d1)
  'data.frame':	3 obs. of  4 variables:
   $ name: Factor w/ 3 levels "kim","lee","seo": 1 2 3
   $ ko  : num  90 80 98
   $ en  : num  100 78 92
   $ ma  : num  99 68 88
  
  
  # 기존 컬럼 값 수정
  > d1$ko <- c(100, 90, 99)
  
  # 새 컬럼 추가
  > d1$si <- c(90, 80, 88)
  > d1
    name  ko  en ma si
  1  kim 100 100 99 90
  2  lee  90  78 68 80
  3  seo  99  92 88 88
  ```

  

- 데이터 프레임 접근

  ```R
  # 특정 컬럼 값만 출력
  > dname <- d1$name
  > dname
  [1] kim lee seo
  Levels: kim lee seo
  
  
  # d[m, n, drop=TRUE] : m행 n컬럼 데이터
  # 색ㅇ인 지정 : c("", "")
  # 제외시 - 표시
  > d1[-1, c("ko", "en", "ma")]
    ko en ma
  2 90 78 68
  3 99 92 88
  
  # drop=TRUE : 해당 데이터의 타입인 벡터 형태로 생성
  > d1[, c("ko")]
  [1] 100  90  99
  # drop=FALSE : 데이터 프레임으로 생성
  > d1[, c("ko"),drop=F]
     ko
  1 100
  2  90
  3  99
  ```






### 07. 유틸리티 함수

```R
# 처음부터 n만큼 반환
> head(d1,2)
  name  ko  en ma si
1  kim 100 100 99 90
2  lee  90  78 68 80

# 마지막부터 n만큼 반환
> tail(d1,2)
  name ko en ma si
2  lee 90 78 68 80
3  seo 99 92 88 88

# 데이터 뷰어를 호출
View(d1)
```





### 타입 판별

- class(x) :: 객체의 클래스 -> 문자열로 데이터 타입 반환
- str(x) :: 객체의 내부 구조

- is.factor(x) :: 팩터인가
- is.numeric(x) :: 숫자 벡터인가
- is.character(x) :: 문자열 벡터인가
- is.matrix(x) :: 행렬인가
- is.array :: 배열인가
- is.data.frame :: 데이터 프레임인가





### 타입 변환

- as.factor(x) :: 팩터로 변환
- as.numeric(x) :: 숫자 벡터로 변환
- as.character(x) :: 문자열 벡터로 변환
- as.matrix(x) :: 행렬로 변환
- as.array :: 배열로 변환
- as.data.frame :: 데이터 프레임으로 변환
