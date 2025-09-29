# SQL_BASIC 4주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_4th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**4주차 과제부터는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_4th

### 섹션 4. 쿼리 잘 작성하기, 쿼리 작성 템플릿 및 오류를 잘 디버깅하기

### 3-4. 오류를 잘 디버깅하는 방법



## 섹션 5. 데이터 탐색 - 변환

### 4-1. INTRO

### 4-2. 데이터 타입과 데이터 변환(CAST, SAFE_CAST)

### 4-3. 문자열 함수(CONCAT, SPLIT, REPLACE, TRIM, UPPER)

### 4-4. 날짜 및 시간 데이터 이해하기(1) (타임존, UTC, Millisecond, TIMESTAMP/DATETIME)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리

## 3-4. 오류를 디버깅하는 방법

~~~
✅ 학습 목표 :
* 오류의 정의에 대해 설명할 수 있다. 
* 오류 메시지를 보고 디버깅이라는 과정을 수행할 수 있다. 
~~~

오류란 쿼리를 작성하면서 실행이 안되는 경우를 말한다. 즉 이대로 진행한다면 실행할 수 없다는 것을 나타내는 것!

대표적인 오류에는 Syntax Error(문법 오류)가 있다. 
예를 들어 SELECT목록은 비어있으면 안된다, COUNT의 인자 수가 일치해야한다, GROUP BY에 적절한 컬럼을 명시해야한다, 하나의 쿼리엔 SELECT가 1개만 있어야한다, 괄호를 맞게 작성해야한다. 
등의 디버깅 방법이 있다. 



## 4-2. 데이터 타입과 데이터 변환(CAST, SAFE_CAST)

~~~
✅ 학습 목표 :
* 데이터 타입의 종류를 설명할 수 있다. 
* 데이터 타입을 변환하는 방법을 설명할 수 있다. 
~~~

<데이터 타입>
1. 숫자 (정수, 소수점 등 포함)
2. 문자 (따옴표로 구성되는 문자열)
3. 시간, 날짜 (여러가지로 나뉘기는 함, 2024-09-24 23:59:10 등)
4. 부울(Bool) (참/거짓, Where, True)

<데이터 타입을 변환하는 방법>
*데이터 타입 변환이 필요한 이유: TRUE값이 NULL값으로 들어간 것이 아닌 문자열로 들어갔을 수도 있음 즉 -> 내 생각과 다르게 데이터 타입이 입력되어 있을 수 있음

자료 타입을 변경하는 함수: CAST

ex. CAST(1 AS STRING) #숫자 1을 문자 1로 변경

더 안전하게 자료 타입을 변경하는 함수: CAST_SAFE




## 4-3. 문자열 함수(CONCAT, SPLIT, REPLACE, TRIM, UPPER)

~~~
✅ 학습 목표 :
* 문자열 함수들의 종류를 이해하고 어떠한 상황에서 사용하는지 설명할 수 있다. 
~~~

문자열은 "안녕하세요","카일스쿨"등의 형식이다. 
문자열 함수에는 CONCAT, SPLIT, REPLACE, TRIM, UPPER 등이 있다. 
1) CONCAT
- 문자열을 연결하는 함수로서 여러 문자열을 하나로 합치고 싶을 때 사용
2) SPLIT
- 구분자(ex.쉼표, 공백, 하이픈)를 기준으로 문자열을 잘라 배열이나 여러 부분으로 나눌 때 사용
3) REPLACE
- 문자열을 치환하는 함수로서 특정 문자열을 다른 문자열로 바꿀 때 사용
4) TRIM
- 공백을 제거하는 함수로서 문자열 앞뒤에 불필요하게 들어간 공백, 특수문자 등을 제거할 때 사용
5) UPPER
- 대문자로 변환하는 함수로서, 문자열을 모두 대문자로 바꾸고 싶을 때 사용



## 4-4. 날짜 및 시간 데이터 이해하기(1) (타임존, UTC, Millisecond, TIMESTAMP/DATETIME)

~~~
✅ 학습 목표 :
* 날짜 및 시간 데이터 타입과 UTC의 개념을 설명할 수 있다. 
* DATE, DATETIME, TIMESTAMP 에 대해서 설명할 수 있다.
* 시간함수들의 종류와 시간의 차이를 추출하는 방법을 설명할 수 있다. 
~~~

*개발할 때 사용하는 시간의 개념은 다르다

날짜 및 시간 데이터 타입에는 DATE, DATETIME, TIMESTAMP가 있다. 

UTC란?
Universal Time Coordinated의 약자로서 국제적인 표준 시간을 의미함

1) DATE: DATE만 표시하는 데이터 (2023-12-31)
2) DATETIME: DATE와 TIME까지 표시하는 데이터, (2023-12-31 14:00:00)
3) TIME: 날짜와 무관하게 시간만 표시하는 데이터 (23:59:59)
4) TIMESTAMP: 시간 도장으로서, UTC부터 경과한 시간을 나타내는 값

*millisecond: 시간의 단위, 천분의 1초, 빠른 반응이 필요한 분야에서 사용(초보다 더 정확하다)
*microsecond: 1/1,000,000초

<시간 데이터를 변환하는 방법>
TIMESTAMP <-> DATETIME 변환을 해야하는 경우도 존재함

<시간함수들의 종류 및 시간 차이>
1) TIMESTAMP(한국 시간에서 -9시간)
2) DATETIME(한국 zone 사용시 한국 시간과 동일)

<시간의 차이를 추출하는 방법>
1) 요일을 추출하고 싶은 경우
  EXTRACT(DATE FROM DATETIME "2024-01-02 14:00:00") AS date,
  EXTRACT(YEAR FROM DATETIME "2024-01-02 14:00:00") AS year,
  EXTRACT(MONTH FROM DATETIME "2024-01-02 14:00:00") AS month,
  EXTRACT(DAY FROM DATETIME "2024-01-02 14:00:00") AS day,
  EXTRACT(HOUR FROM DATETIME "2024-01-02 14:00:00") AS hour,
  EXTRACT(MINUTE FROM DATETIME "2024-01-02 14:00:00") AS minute

<br>

<br>

---
<img width="483" height="813" alt="image" src="https://github.com/user-attachments/assets/9152497b-9707-479d-a7c6-b2d9568774bb" />

# 2️⃣ 확인문제 & 문제 인증

## 프로그래머스 문제 

> 조건에 맞는 회원 수 구하기 (SELECT, COUNT) 
>
> **먼저 문제를 풀고 난 이후에 확인 문제를 확인해주세요**
>
> 문제 링크 
>
> :  https://school.programmers.co.kr/learn/courses/30/lessons/131535#

<img width="687" height="194" alt="image" src="https://github.com/user-attachments/assets/531e3398-017c-4d9a-9378-54f7be11b308" />



## 문제 1

> **🧚Q. 프로그래머스 문제를 풀던 서현이는 여러 번의 시행착오 끝에 결국 혼자 해결하기 어려워 오류 메시지를 공유하며 도움을 요청했습니다. 여러분들이 오류 메시지를 확인하고, 해당 SQL 쿼리에서 어떤 부분이 잘못되었는지 오류 메시지를 해석하고 찾아 설명해주세요.**

~~~sql
# 조건에 맞는 회원 수 구하기 (SELECT, COUNT) 
# 서현이의 SQL 첫 번째 풀이
SELECT COUNT(AGE, JOINED)
FROM USER_INFO
WHERE AGE BETWEEN 20 AND 29
  AND JOINED BETWEEN '2021-01-01' AND '2021-12-31';
  
오류 메시지 : Error: Number of arguments does not match for aggregate function COUNT
 
# 수정하고 난 이후 두 번째 풀이
SELECT AGE, COUNT(*)
FROM USER_INFO
WHERE AGE BETWEEN 20 AND 29
  AND JOINED BETWEEN '2021-01-01' AND '2021-12-31';
  
오류 메시지 : SELECT list expression references column AGE which is neither grouped nor aggregated



~~
SELECT COUNT(*) AS USERS
FROM USER_INFO
WHERE JOINED BETWEEN '2021-01-01' AND '2021-12-31'
  AND AGE BETWEEN 20 AND 29;
~~




### 🎉 수고하셨습니다.
