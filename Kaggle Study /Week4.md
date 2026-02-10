하루 시간대별로 택시 이동(유입/유출)이 어떻게 달라질까?
- 
NYC를 위치 기반으로 몇 개의 구역으로 나누 뒤, 시간대별로 각 구역에 들어오고 나가는 택시 수를 파악하자

# 1️⃣ 분석 흐름 따라잡기

## 1. 데이터 불러오기 및 확인하기

~~~python
import numpy as np
~~~
- 수치 계산, 연산 라이브러리
~~~python
from matplotlib import animation
~~~
- 애니메이션(시간에 따라 변하는 시각화) 만들 때 사용하는 라이브러리
~~~python
from matplotlib import cm
~~~
- 컬러맵(colormap) 모음
- 클러스터별 색칠, 히트맵 등에서 자주 사용
~~~python
from sklearn.cluster import KMeans
~~~
- k-means 군집화 알고리즘 사용할 때
(위경도 좌표를 넣어서 k개 구역으로 쪼갤 때 사용)
~~~python
from sklearn.neighbors import KNeighborsClassifier
~~~
- KKN 분류기
- 새로운 좌표가 들어왔을 때 어느 클러스터에 속하는지 빠르게 예측할 때 사용

### 컬럼 확인

| 컬럼명                | 타입     | 설명                          |
| ------------------ | ------ | --------------------------- | 
| id                 | object | 택시별 고유 ID         | 
| vendor_id          | int    | 택시 회사 구분 ID (주로 1, 2)       | 
| pickup_datetime    | object | 승차 시각                       | 
| dropoff_datetime   | object | 하차 시각                       | 
| passenger_count    | int    | 탑승 인원 수                     | 
| pickup_longitude   | float  | 승차 지점 경도                    |
| pickup_latitude    | float  | 승차 지점 위도                    | 
| dropoff_longitude  | float  | 하차 지점 경도                    | 
| dropoff_latitude   | float  | 하차 지점 위도                    | 
| store_and_fwd_flag | object | 데이터가 서버로 즉시 전송되었는지 여부 (Y/N) | 
| trip_duration      | int    | 이동 시간 (초 단위)                | 

### 포함할 위치 범위 좁히기

~~~python
xlim = [-74.03, -73.77]
ylim = [40.63, 40.85]
df = df[(df.pickup_longitude> xlim[0]) & (df.pickup_longitude < xlim[1])]
df = df[(df.dropoff_longitude> xlim[0]) & (df.dropoff_longitude < xlim[1])]
df = df[(df.pickup_latitude> ylim[0]) & (df.pickup_latitude < ylim[1])]
df = df[(df.dropoff_latitude> ylim[0]) & (df.dropoff_latitude < ylim[1])]
~~~
- xlim[0] -> 왼쪽 경계 (-74.03)
- xlim[1] -> 오른쪽 경계 (-73.77)
- ylim[0] -> 아래쪽 경계 (40.63)
- ylim[1] -> 위쪽 경계 (40.85)


### 승하차 위치 지도 위에 시각화
