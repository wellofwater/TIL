## 조건문

> if, ifelse

```R
# 나이: AGE
# A >= 30 : H / 30 > A >= 25 : M / 25 > A >= 20 : L, 20 > A : F 
> sh$AGE_HL <- ifelse(sh$AGE >= 30, "H",
+                     ifelse(sh$AGE >= 25, "M",
+                            ifelse(sh$AGE >= 20, "L","F")
+                            )
+                     )
> sh
    ID   NAME AGE TEMP PRICE QT AGE_HL
1 id01 이말숙  23   15 10000  1      L
2 id02 김말숙  28   NA 20000  2      M
3 id03 홍말숙  30   15 30000  3      H
```

```R
# 총구매 금액: PRICE * QT
# P <= 10000 : B / 10000 < P <= 30000 : S / P <= 300000 : G / P > 300000 : F
> sh$GRADE <- ifelse(sh$PRICE * sh$QT <= 10000, "B",
+                       ifelse(sh$PRICE * sh$QT <= 30000, "S",
+                              ifelse(sh$PRICE * sh$QT <= 300000,"G","F")
+                   		    )
+ 					)
> sh
    ID   NAME AGE TEMP PRICE QT YYYY MM DD AGE_HL GRADE
1 id01 이말숙  23   15 10000  1 2020  9 30      L     B
2 id02 김말숙  28   NA 20000  2 2020  9 30      M     G
3 id03 홍말숙  30   15 30000  3 2020  9 30      H     G
4 id01 이말숙  23   33 10000  2 2020 10  1      L     S
5 id02 김말숙  28   33 20000  1 2020 10  1      M     S
6 id03 홍말숙  30   33 30000  1 2020 10  1      H     S
7 id01 이말숙  23   25 10000  5 2020  8  1      L     G
8 id02 김말숙  28   25 20000  4 2020  8  1      M     G
9 id03 홍말숙  30   25 30000  6 2020  8  1      H     G
```



## 반복문

### for

```R
# for문에 벡터 데이터 사용
> b <- c("a","b","c","d")
> for(i in b){
+   print(i)
+ }
[1] "a"
[1] "b"
[1] "c"
[1] "d"
> func1
```



### while

```R

```

