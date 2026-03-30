# Python 4주차 정규 과제 

📌Python 정규과제는 매주 정해진 분량의 『*파이썬 라이브러리를 활용한 데이터 분석*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **Python_4th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 참고 자료를 통해 보완하는 것이 좋습니다.

**교재 실습 예제 파일은 07_Python_Template 레포지토리의 notebooks 폴더에 업로드되어 있습니다.**

**👀(수행 인증샷은 필수입니다.)** 

## Python_4th_TIL

### 5장 판다스 시작하기 
#### 1. 판다스 자료구조 소개
#### 2. 핵심 기능
#### 3. 기술 통계 계산과 요약
#### 4. 마치며 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.25~82    | ✅         |
| 2주차 | p.83~129   | ✅         |
| 3주차 | p.131~179  | ✅         |
| 4주차 | p.181~246 | ✅         |
| 5주차 | p.247~309 | 🍽️         |
| 6주차 | p.310~379 | 🍽️         |
| 7주차 | p.381~465 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 판다스 자료구조 소개

### 개념정리

판다스의 자료구조 종류

1) Series
- Series는 일련의 객체를 담을 수 있는 1차원 배열 같은 자료구조

3) DataFrame
- DataFrame은 표 같은 스프레드시트 형식의 자료구조



### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="708" height="496" alt="image" src="https://github.com/user-attachments/assets/18ca43b8-fd54-43f3-a521-6af5da5a25a8" />

<img width="619" height="430" alt="image" src="https://github.com/user-attachments/assets/f3858789-891d-4ced-afb9-dd43434be0bd" />


## 2. 핵심 기능

### 개념정리

**재색인**
- 새로운 색인에 적합하도록 개체를 새로 생성하는 기능

**하나의 행이나 열 삭제하기**
- 삭제하려는 축이 제외된 리스트를 이미 가지고 있거나, 색인 배열을 갖고 있다면 행이나 열을 쉽게 삭제할 수 있다

**색인하기, 선택하기, 거르기**

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="593" height="823" alt="image" src="https://github.com/user-attachments/assets/890ab2bf-bc0b-4623-bdd2-3c89807c6d56" />

<img width="578" height="453" alt="image" src="https://github.com/user-attachments/assets/f8c42e83-dd78-4d66-8e67-c7d631722002" />



## 3. 기술 통계 계산과 요약

### 개념정리

- 판다스 객체는 일반적인 수학 매서드와 통계 매서드를 가지고 있음.
- 대부분의 매서드는 하나의 Series나 DataFrame의 행과 열에서 단일 값을 구하는 축소 or 요약 통계에 속함.


### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="601" height="720" alt="image" src="https://github.com/user-attachments/assets/10a1b4d2-31aa-46bc-85d5-8456b8218add" />

<img width="616" height="432" alt="image" src="https://github.com/user-attachments/assets/b584ad66-5382-4497-bb45-2d6fc630458f" />




# 2️⃣ 실습 과제

각 문제에 대한 실행 결과가 확인되도록 코드를 작성하고 실행한 뒤, **모든 문제의 실행 화면을 캡처하여 제출하시기 바랍니다.**

**1. 아래 코드를 실행하여 도서 매출 데이터를 생성합니다.**
```python
import pandas as pd
import numpy as np

# 도서 데이터 생성
data = {
    "book_name": ["Python 기초", "Pandas 실무", "데이터 분석", "머신러닝 입문", "딥러닝 가이드"],
    "price": [25000, 32000, 28000, 45000, 38000],
    "sales_count": [15, 8, 20, 5, 12],
    "stock": [10, 5, 0, 15, 7]
}
df = pd.DataFrame(data)
```

**2. 문제**
```
1. 데이터 확인 및 기초 통계 출력
  - 문제 설명: 요약 정보 확인 
  - describe() 메서드를 사용하여 수치형 데이터(가격, 판매량 등)의 기술 통계 정보를 출력하세요.
  - print()를 이용해 기술 통계 결과를 화면에 출력하세요.

2. 특정 조건의 데이터 필터링 (불리언 색인)
  - 문제 설명: 현재 재고가 하나도 없는(0인) 도서 찾기 
  - stock 열의 값이 0인 행만 추출하여 새로운 변수에 저장하세요.
  - print()를 이용해 재고가 0인 도서의 정보를 출력하세요.

3. 새로운 열 추가 (파생 변수 생성)
  - 문제 설명: 각 도서별 총 매출액(total_revenue) 계산
  - price와 sales_count를 곱하여 total_revenue라는 새로운 열을 추가하세요.
  - print()를 이용해 새로운 열이 추가된 DataFrame의 상단 5행(head)을 출력하세요.
```

<img width="612" height="314" alt="image" src="https://github.com/user-attachments/assets/084bfc30-f83d-4d9a-bd67-8451b1b70e62" />

<img width="546" height="169" alt="image" src="https://github.com/user-attachments/assets/849eb3dd-00aa-4254-8f79-bb3030977284" />

<img width="583" height="254" alt="image" src="https://github.com/user-attachments/assets/62047b26-686d-46cc-aabf-429efe8078f4" />



### 🎉 수고하셨습니다.






