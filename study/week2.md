<1장- 파이썬과 재무 기초 지식>
-주피터 노트북이란?
소스 코드나 입력값을 고쳐 실행하면 값이 바뀌고 차트가 다시 그려지는 프로그램

-구글 코랩 실행법
-구글 코랩 패널
목차
찾기 및 바꾸기
코드 스니펫
파일
셀
코드 실행법
코랩 단축키
*해외 증권 데이터를 제공하는 Quandl 라이브러리를 설치한다면 다음과 같이 pip 명령을 사용해야 한다
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
!pip install quandl
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ

-변수
1) 숫자형 변수
2) 문자형 변수 (작은 따옴표 또는 큰따옴표 사용)

-데이터 객체
1) 리스트 ([]를 사용, 콤마(,)로 구분)

- 인덱싱 (값은 0부터 시작하지만 마이너스 값을 갖기도 함)

-슬라이싱
인덱스를 이용해 리스트의 일부분을 떼어내는 것 (:사용)

-듀플(()를 사용, 프로그램이 실행되는 동안 내용이 바뀌면 안되는 목록이 있다면 사용 추천)

-딕셔너리
키(key)와 값(value) 한 쌍이 필요, 숫자로 된 인덱스가 아닌 키를 갖고 값을 다루는 경우 사용 추천

-연산자
산술 연산, 비교 연산, 논리 연산
할당 연산자, 산술 연산자, 논리 연산자, 비교 연산자

-흐름 제어
1) if문
2) for 반복문

-함수
1) def
2) 기본 제공 함수

-모듈
-주석과 들여쓰기
1) 주석 (#로 표기)
2) 들여쓰기 (들여쓰기를 제대로 하지 않으면 에러 발생)

ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
현금흐름, 이자율과 시간 가치

-단리이자 계산
scipy 라이브러리를 임포트한다
import scipy as sp

미래가치를 계산하기 위한 변수들을 설정한다
a = 1000
n = 1
r = 0.05

fv( ) 함수를 이용해 미래가치를 계산한다
s_simple = sp.fv( r, n, 0, a )
print( s_simple )

-복리이자 계산
파이썬의 수학 모듈인 math는 오일러상수(e)를 제공한다. math 라이브러리(모듈)를 임포트한다
import math

앞서 살펴본 코드와 같이 연속복리에 따른 원금을 계산한다. amount는 원금, rate는 이자율이다. n은 기간이다
amount = 1
rate = 1.0

기간이 1인 경우, 즉 1년 복리라면
n = 1
c_compound = a*( 1+r/n )**n
print( c_compound )

기간이 2인 경우, 즉 6개월 복리라면
n = 2
c_compound = a*( 1+r/n )**n
print( c_compound )

기간이 4인 경우, 즉 분기 복리라면
n = 4
c_compound = a*( 1+r/n )**n
print( c_compound )

기간이 12인 경우, 즉 월 복리라면
n = 12
c_compound = a*( 1+r/n )**n
print( c_compound )

기간이 52인 경우, 즉 매주 복리라면
n = 52
c_compound = a*( 1+r/n )**n
print( c_compound )

기간이 365인 경우, 즉 매일 복리라면
n = 365
c_compound = a*( 1+r/n )**n
print( c_compound )

기간이 8760인 경우, 즉 매시간 복리라면
n = 8760
c_compound = a*( 1+r/n )**n
print( c_compound )

기간이 525600인 경우, 즉 매분 복리라면
n = 525600
c_compound = a*( 1+r/n )**n
print( c_compound )

기간이 31536000인 경우, 즉 매초 복리라면
n = 31536000
c_compound = a*( 1+r/n )**n
print( c_compound )

ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
NPV와 IRR

NPV(순현재가치), IRR(내부수익률) 모두 현금흐름을 할인하는 방법

-NPV 함수
scipy 라이브러리를 이용한 계산
scipy 라이브러리를 sp라는 이름으로 임포트한다
import scipy as sp

현금흐름을 리스트로 만든다
cashflows = [ -70000, 12000 , 15000 , 18000 , 21000 , 26000 ]
r = 0.015

npv 함수로 순현재가치를 계산한다
npv = sp.npv( r, cashflows )
print( npv )

-IRR 함수
scipy 라이브러리를 임포트한다
import scipy as sp

현금흐름을 cashflows 리스트에 저장한다
cashflows = [ -70000, 12000 , 15000 , 18000 , 21000 , 26000 ]

scipy 라이브러리의 irr 함수를 사용해 내부수익률을 계산한다
irr= sp.irr( cashflows )

구한 IRR을 npv의 할인율로 사용해 NPV를 구한다. IRR이 정확하다면 NPV는 0이다
npv=sp.npv( irr, cashflows )

결과를 출력한다. 결과는 문자열의 서식 기능을 이용한다
print( 'IRR {0:.1%} makes NPV {1:.2f} '.format( irr, npv ) )

-수익률과 할인율
1) 수익률로 평균을 구하는 코드
기간별 수익률을 returns 리스트에 저장한다
returns = [ 0.1, 0.06, 0.05 ]

합계를 저장할 변수를 준비한다
sumOfReturn = 0.0

평균을 저장할 변수를 준비한다
arimean = 0.0
geomean = 1.0

기간별 수익률의 데이터 개수를 구한다
n = len(returns)

returns 리스트를 for 루프로 반복한다. 반복하는 동안 각 수익률을 변수 r로 받는다
for r in returns:
    sumOfReturn = sumOfReturn + r

arimean = sumOfReturn /3
print( 'AriMean is {:.2%}'.format( arimean ) )

ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
자주 사용하는 통계량

-산술평균
nums = [1, 2, 3, 4, 5, 6]      # nums 리스트에 값을 저장
print( sum(nums) / len(nums) ) # sum 함수로 합계를, len 함수로 데이터의 개수를 구한다

-기댓값
사건과 확률을 case와 prob 리스트에 저장한다
case = [ 1, 2, 3, 4, 5, 6 ]
prob = [ 1/6, 1/6, 1/6, 1/6, 1/6, 1/6 ]

사건과 확률 리스트를 zip 함수로 묶어 for 루프로 반복한다. 반복하는 동안 두 리스트에서 값을 받아 변수 c와 p에 저장하고 곱한 결과를 ex 변수에 저장한다
ex = 0.0
for c, p in zip( case, prob ):
    ex = ex + c*p
print( ex ) # 결과를 출력한다
참고

위 반복문은 다음과 같은 인라인 for 루프로 대체할 수 있다.

ex = sum( c*p for c, p in zip( case, prob ) )
결과를 출력한다
print( ex )

-이동평균 (정지된 값이 아ㅣ라 새로운 데이터를 받아 그 대푯값을 업데이트한다. 주식 차트 등)
이동평균
주가를 prices 리스트에 저장한다
prices = [ 44800, 44850, 44600, 43750, 44000, 43900, 44350, 45350, 45500, 45700 ]

5일 이동평균
n = 5

prices의 n번째 항목부터 마지막 항목까지 반복한다
for p in prices[ n: ]:

항목 p의 index를 마지막 인덱스로 정한다
 end_index = prices.index( p )

마지막 인덱스에서 n만큼 앞에 있는 시작 인덱스를 정한다
 begin_index = end_index - n

end_index와 begin_index를 계산해 가져올 위치를 확인한다
 print( begin_index, end_index )

계산한 end_index와 begin_index를 갖고 prices 리스트에서 다섯 개 항목을 확인한다
for p in prices[ n: ]:
    end_index = prices.index( p )
    begin_index = end_index - n
    print( prices[ begin_index : end_index ] )

다섯 개씩 가져와서 sum( ) 함수로 합계를 구하고 n으로 나눠 이동평균을 계산한다
for p in prices[ n: ]:
    end_index = prices.index( p )
    begin_index = end_index – n
    print( sum( prices[ begin_index : end_index ] ) /n )

- 가중평균
평가 점수와 평가 비중을 scores와 weight 리스트에 저장한다
scores = [ 82, 90, 76 ]
weight = [ 0.2, 0.35, 0.45 ]

scores와 weight 리스트를 zip 함수로 묶어 for 루프로 반복한다
wgt_avg는 합계를 저장할 변수
wgt_avg = 0.0

반복하는 동안 변수 s와 w에 저장하고 곱셈의 결과를 합한다
for s, w in zip( scores, weight ):
    wgt_avg = wgt_avg + s*w

결과를 출력한다
print ( wgt_avg )
참고

위의 for 루프는 인라인 for 루프로 바꿔 간략하게 표현할 수 있다.

wgt_avg = sum( s*w for s, w in zip( scores, weight ) )
결과를 출력한다
print ( wgt_avg )

-분산과 표준편차
-공분산과 상관계수
1) 공분산
공분산을 계산하는 함수
def covariance( x, y ):
    n = len( x )
    return sum_of_product( deviation( x ), deviation( y ) ) / ( n-1 )

표준편차를 계산하는 함수
def standard_deviation( x ):
    return math.sqrt( variance( x ) ) # math 모듈의 제곱근 함수 sqrt( )를 사용

상관계수를 계산하는 함수
def correlation( xs, ys ):
    stdev_x = standard_deviation( xs )
    stdev_y = standard_deviation( ys )
    if stdev_x > 0 and stdev_y > 0:
        return covariance( xs, ys ) / ( stdev_x * stdev_y )
    else :
        return 0 # 편차가 존재하지 않는다면 상관관계는 0

x와 y 리스트와 correlation( ) 함수를 사용해 상관관계를 계산
x=[ 41, 43, 38, 37 ]
y=[ 61, 63, 56, 55 ]
print( correlation( x, y ) )

x=[ 35, 45, 35, 34 ]
y=[ 65, 54, 64, 67 ]
print( correlation( x, y ) )

x=[ 35, 45, 35, 34 ]
y=[ 65, 66, 64, 68 ]
print( correlation( x, y ) )

2) 상관계수
상관계수를 계산하기 위해 필요한 함수들
import math

평균을 계산하는 함수

def mean( x ):

return sum( x ) / len( x )

두 리스트 곱의 합계, 즉 엑셀의 SUMPRODUCT( ) 함수와 같다
def sum_of_product( xs, ys ):
    return sum( x * y for x, y in zip( xs, ys ) )

제곱합을 계산하는 함수
def sum_of_squares( v ):
    return sum_of_product( v, v )

편차를 계산하는 함수
def deviation( xs ):
    x_mean = mean( xs )
    return [ x - x_mean for x in xs ]

분산을 계산하는 함수
def variance( x ):
    n = len( x )
    deviations = deviation( x )
    return sum_of_squares( deviations ) / ( n-1 )

ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
<2장 투자와 자산배분>
자산베분이란?
주식에 얼마를 투자할지 채권이나 예금에 얼마를 투자할지 등을 정하는 정도를 의미

자산배분은 샤프의 CAPM 모델에 기초한다

자산배분에서는 리밸런싱도 필요하다
리밸런싱은 가격의 변동에 따라 내재가치를 증가시켜나가는 작업, 이 과정에서 비중 조절이나 종목 교체도 일어난다

-포트폴리오 성과의 결정 요인들

-포트폴리오 성과 측정 삼총사
1) 샤프지수
투자수익률 대 변동성 비율 / 한 단위의 위험을 부담하는 대신 얻을 수 있는 수익률 / 샤프지수가 클수록 좋다
2) 트레이너지수
위험 한 단위를 받고 얻은 초과성과가 얼마인지 측정 / 값이 클수록 성과가 우월함을 나타냄
3) 젠센알파지수
포트폴리오 수익률과 기대수익률의 차이를 나타내는 수치 / 시장 대비 얼마나 높은 성과를 냈는지의 지표 / 펀드 매니저의 종목 선정 능력을 확인할 수 있는 지표로 활용

-최대낙폭
