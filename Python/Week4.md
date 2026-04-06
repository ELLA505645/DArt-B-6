# Python 5주차 정규 과제 

📌Python 정규과제는 매주 정해진 분량의 『*파이썬 라이브러리를 활용한 데이터 분석*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **Python_5th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 참고 자료를 통해 보완하는 것이 좋습니다.

**교재 실습 예제 파일은 07_Python_Template 레포지토리의 notebooks 폴더에 업로드되어 있습니다.**

**아나콘다 환경에서는 많은 패키지가 기본적으로 포함되어 있어 별도의 설치 없이 사용할 수 있지만, 환경에 따라 conda install이나 pip install이 필요할 수 있습니다.**

**👀(수행 인증샷은 필수입니다.)** 

## Python_5th_TIL

### 6장 데이터 로딩과 저장, 파일 형식
#### 1. 텍스트 파일에서 데이터를 읽고 쓰는 법
#### 2. 이진 데이터 형식
#### 3. 웹 API와 함께 사용하기
#### 4. 데이터베이스와 함께 사용하기
#### 5. 마치며
### 7장 데이터 정제 및 준비
#### 1. 누락된 데이터 처리하기
#### 2. 데이터 변형 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.25~82    | ✅         |
| 2주차 | p.83~129   | ✅         |
| 3주차 | p.131~179  | ✅         |
| 4주차 | p.181~246 | ✅         |
| 5주차 | p.247~309 | ✅         |
| 6주차 | p.310~379 | 🍽️         |
| 7주차 | p.381~465 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 텍스트 파일에서 데이터를 읽고 쓰는 법

### 개념정리

텍스트 데이터를 DataFrame으로 읽어오는 함수는 몇가지 옵션을 취한다.
1) 색인: 파일이나 사용자로부터 열이름을 받거나 아무것도 받지 않을 수 있다
2) 자료형 추론과 데이터 변환: 사용자 정의 값 변환과 비어 있는 값을 위한 사용자 리스트를 포함한다
3) 날짜 및 시간 분석: 여러 열에 걸쳐 있는 날짜와 시간 정보를 하나의 열에 조합해서 결과에 반영한다
4) 반복: 여러 개의 파일에 걸쳐 있는 자료를 반복적으로 읽어온다
5) 정제되지 않는 데이터 처리: 행이나 꼬리말, 주석 건너뛰기 또는 천 단위마다 쉼표로 구분된 숫자 같은 사소한 요소를 처리한다

**방식**
- 텍스트 파일 조금씩 읽어오기
- 데이터를 텍스트 형식으로 기록하기
- 다른 구분자 형식 다루기
- JSON 데이터
- XML과 HTML: 웹 스크래핑

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="604" height="755" alt="image" src="https://github.com/user-attachments/assets/93d131aa-5d65-494d-8513-5f4fce9d91d9" />
<img width="606" height="683" alt="image" src="https://github.com/user-attachments/assets/98278128-b1a9-4a1d-933c-25685a681dcf" />



## 2. 이진 데이터 형식

### 개념정리

데이터를 이진 형식으로 저장하는 간단한 방식
: 내장 pickle 모듈을 이용하는 것

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="573" height="753" alt="image" src="https://github.com/user-attachments/assets/a847fce6-1490-4702-a6ea-ca48b437a4b7" />
<img width="594" height="400" alt="image" src="https://github.com/user-attachments/assets/3decda46-f252-4faa-a5a3-5b5262e98eb3" />



## 3. 웹 API와 함께 사용하기

### 개념정리

*데이터 피드를 JSON이나 다른 형식으로 얻을 수 있는 공개 API를 제공하는 웹사이트가 많음

**방식**
- requests 패키지를 사용

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->
<img width="1122" height="402" alt="image" src="https://github.com/user-attachments/assets/154559b3-3b43-4254-b134-43c67a8967ef" />
<img width="1448" height="511" alt="image" src="https://github.com/user-attachments/assets/606fef6a-0289-46cf-b051-07b20bf51714" />



## 4. 데이터베이스와 함께 사용하기

### 개념정리

비즈니스 관점에서, SQL 기반의 관계형 데이터베이스를 많이 사용하고 있다.


### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->
<img width="906" height="588" alt="image" src="https://github.com/user-attachments/assets/d37f7be1-e0ea-4982-865e-b05494a856a1" />
<img width="911" height="725" alt="image" src="https://github.com/user-attachments/assets/c63028b4-0160-4256-b907-21f506513d26" />


## 5. 누락된 데이터 처리하기

### 개념정리

판다스 객체의 모든 기술 통계는 기본적으로 누락된 데이터를 배제하고 처리한다.
float64 dtype을 가지는 데이터의 경우 판다스는 실숫값인 NaN으로 누락된 데이터를 표시한다.


### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="838" height="325" alt="image" src="https://github.com/user-attachments/assets/c40ac9fc-1158-448d-88e2-9feaa8c61bc4" />
<img width="850" height="230" alt="image" src="https://github.com/user-attachments/assets/59b37ac5-546c-4cd7-b115-c3d745fae27b" />


## 6. 데이터 변형 

### 개념정리

데이터 변형의 종류에는 필터링, 정제, 변형이 있다.

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->
<img width="914" height="626" alt="image" src="https://github.com/user-attachments/assets/ce2c118b-bc89-40f7-9026-6821867f0142" />
<img width="913" height="695" alt="image" src="https://github.com/user-attachments/assets/c7d3f4e6-a4d1-4276-8288-e4b1f147515b" />




# 2️⃣ 실습 과제

각 문제에 대한 실행 결과가 확인되도록 코드를 작성하고 실행한 뒤, **모든 문제의 실행 화면을 캡처하여 제출하시기 바랍니다.**

**1. 아래 코드를 실행하여 텍스트와 데이터를 선언합니다.**
```python
import pandas as pd
import json
import requests

# 1. 테스트용 CSV 내용 (메모리 내 시뮬레이션용)
csv_data = "id,name,score,note\n1,Kim,85,NA\n2,Lee,NULL,Good\n3,Park,90,None"
with open("test_data.csv", "w") as f:
    f.write(csv_data)

# 2. 테스트용 JSON 문자열
json_obj = """
{
    "company": "DataService",
    "employees": [
        {"name": "Alice", "age": 30, "dept": "IT"},
        {"name": "Bob", "age": 25, "dept": "HR"}
    ]
}
"""
```

**2. 문제**
```
1. CSV 파일 읽기 및 결측치 지정
  - 문제 설명: 제공받은 test_data.csv 파일을 읽어오기(단, 데이터의 특성에 맞게 옵션 설정)
  - read_csv()를 사용하여 파일을 읽으세요.
  - 이때 NA, NULL, None이라는 문자열을 모두 **결측치(NaN)**로 인식하도록 na_values 옵션을 설정하세요.
  - print()를 이용해 읽어온 DataFrame을 출력하세요.

2. JSON 데이터 변환 및 특정 데이터 추출
  - 문제 설명: 문자열 형태의 JSON 데이터를 파싱하여 직원 명단만 추출
  - json.loads()를 사용하여 json_obj 문자열을 파이썬 객체로 변환하세요.
  - 변환된 객체에서 employees 리스트만 추출하여 DataFrame으로 만드세요.
  - print()를 이용해 생성된 직원 명단 DataFrame을 출력하세요.

3. 웹 API 데이터 가져오기
  - 문제 설명: 판다스 깃허브 저장소의 이슈(Issues) 데이터를 가져와 상위 항목 확인
  - requests.get()을 사용하여 https://api.github.com/repos/pandas-dev/pandas/issues URL의 데이터를 가져오세요.
  - 응답받은 JSON 데이터를 DataFrame으로 변환하세요.
  - print()를 이용해 데이터의 상단 5행(head)을 출력하세요.
```

<img width="900" height="790" alt="image" src="https://github.com/user-attachments/assets/3d82b051-44ed-4b99-a1f6-647e82b315f0" />

<img width="892" height="349" alt="image" src="https://github.com/user-attachments/assets/38684b56-34c9-42e2-ae86-d785e6af9fc0" />

<img width="1377" height="757" alt="image" src="https://github.com/user-attachments/assets/937b0b16-7c42-4738-9460-be43579ef1a7" />

<img width="1371" height="759" alt="image" src="https://github.com/user-attachments/assets/6bc910f5-eedc-4a9b-901f-7fe73125071f" />

<img width="1441" height="821" alt="image" src="https://github.com/user-attachments/assets/b3f5be4e-3ef1-4c53-8620-80f59d436597" />

### 🎉 수고하셨습니다.






