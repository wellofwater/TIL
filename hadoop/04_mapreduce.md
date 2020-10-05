# 맵리듀스

> HDFS에 저장된 파일을 분산 배치 분석을 할 수 있게 도와주는 프레임워크



## 맵리듀스 프로그래밍 모델

> 맵과 리듀스라는 두 가지 단계로 데이터를 처리

1. 맵(Map) : (k1, v1) → list(k2, v2)

2. 리듀스(Reduce) : (k2, list(v2)) → (k3, list(v3))