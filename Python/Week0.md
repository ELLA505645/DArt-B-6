# Python 1주차 정규 과제 

📌Python 정규과제는 매주 정해진 분량의 『*파이썬 라이브러리를 활용한 데이터 분석*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **Python_1st_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 참고 자료를 통해 보완하는 것이 좋습니다.

**교재 실습 예제 파일은 07_Python_Template 레포지토리의 notebooks 폴더에 업로드되어 있습니다.**

**👀(수행 인증샷은 필수입니다.)** 

## Python_1st_TIL

### 1장 시작하기 전에
#### 1. 다루는 내용
#### 2. 데이터 분석에 파이썬을 사용하는 이유
#### 3. 필수 파이썬 라이브러리
#### 4. 설치 및 설정
#### 5. 커뮤니티와 콘퍼런스
#### 6. 이 책을 살펴보는 방법
### 2장 파이썬 기초, IPython과 주피터 노트북 
#### 1. 파이썬 인터프리터 
#### 2. IPython 기초 
#### 3. 파이썬 기초
#### 4. 마치며


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.25~82    | ✅         |
| 2주차 | p.83~129   | 🍽️         |
| 3주차 | p.131~179  | 🍽️         |
| 4주차 | p.181~246 | 🍽️         |
| 5주차 | p.247~309 | 🍽️         |
| 6주차 | p.310~379 | 🍽️         |
| 7주차 | p.381~465 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 설치 및 설정 

```
아나콘다(Anaconda) 또는 미니콘다(Miniconda)를 설치한 후, 필수 패키지를 설치하고 설치 완료 화면을 캡처하여 제출해주세요.
```
<img width="1507" height="892" alt="image" src="https://github.com/user-attachments/assets/7813125d-9c86-42b3-b864-7f3608d7a609" />

<!--  Python 실행 및 가상환경 관리가 가능한 환경(예: venv 등)이 이미 구축되어 있다면 해당 환경을 사용하셔도 괜찮습니다. 
이 경우 해당 환경의 시작 화면을 캡쳐해주세요.-->


## 2. 파이썬 인터프리터 

```
간단한 hello_world.py 파일을 생성한 후, Anaconda Prompt(또는 Miniconda Prompt)를 실행하세요.
(해당 파일에는 print('Hello World!')라고 입력해주세요.)
프롬프트에서 ipython을 입력하여 IPython 환경을 실행한 뒤, %run hello_world.py 명령어로 파일을 실행하시기 바랍니다.
실행 결과가 나타난 화면을 캡처하여 제출해주세요.
```

<img width="1466" height="728" alt="image" src="https://github.com/user-attachments/assets/a188c200-2cbc-4ba1-89d5-132ede4c8995" />



## 3. IPython 기초  

### IPython 셀 실행하기 
```
IPython을 실행한 후, 아래 코드를 한 줄씩 입력하여 실행해보세요. 각 명령어 실행 결과를 확인하고, 실행 화면을 캡처하여 제출해주세요.
```
```python
a = 5
a

import numpy as np
data = [np.random.standard_normal() for i in range(7)]
data
```

<img width="1471" height="278" alt="image" src="https://github.com/user-attachments/assets/bc18c95e-0a8c-44b4-8a89-1fbd952235d2" />


### 주피터 노트북 실행하기 

```
주피터 노트북을 실행한 후, 새로운 노트북을 생성하세요.
코드 셀에 print("Hello, World!")를 입력하고 실행한 뒤, 출력 결과가 나타난 화면을 캡처하여 제출해주세요.
```

<img width="1068" height="829" alt="image" src="https://github.com/user-attachments/assets/d653d465-f3b7-492e-9b1c-47ef36c4de3e" />



## 파이썬 기초  

작은 데이터를 다루기 위한 표 기반 분석과 데이터 준비 도구를 사용하기 위해서는 방대한 데이터를 처리하기 쉽도록 깔끔하게 구조화된 형태로 다듬어야 한다. 

파이썬은 그런 데이터를 원하는 모양으로 쉽게 다듬을 수 있는 이상적인 언어다. 

파이썬이 익숙해지면 데이터 분석을 위한 데이터를 준비하는 과정을 더 쉽게 할 수 있다.


---

# 2️⃣ 실습 과제

**주피터 노트북에서 아래의 코드 셀을 실행하고, 출력 결과를 캡처하여 제출하세요.**

```python
key = 7
data = [44059, 44050, 39, 52626, 54623, 38]

print("".join(chr(x ^ key) for x in data))
```

<img width="1072" height="336" alt="image" src="https://github.com/user-attachments/assets/4f21fdcd-fdd8-4ce6-bf9c-9e6fa27bd379" />

### 🎉 수고하셨습니다.






