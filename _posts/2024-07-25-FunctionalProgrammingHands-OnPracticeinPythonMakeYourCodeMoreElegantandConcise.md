---
title: "파이썬에서 함수형 프로그래밍으로 코드를 더 짧게 작성하는 방법"
description: ""
coverImage: "/assets/img/2024-07-25-FunctionalProgrammingHands-OnPracticeinPythonMakeYourCodeMoreElegantandConcise_0.png"
date: 2024-07-25 12:12
ogImage: 
  url: /assets/img/2024-07-25-FunctionalProgrammingHands-OnPracticeinPythonMakeYourCodeMoreElegantandConcise_0.png
tag: Tech
originalTitle: "Functional Programming Hands-On Practice in Python Make Your Code More Elegant and Concise"
link: "https://medium.com/gitconnected/functional-programming-hands-on-practice-in-python-make-your-code-more-elegant-and-concise-ed95707b98b0"
isUpdated: true
updatedAt: 1723813222045
---




![Functional Programming Hands-On Practice in Python - Make Your Code More Elegant and Concise](/assets/img/2024-07-25-FunctionalProgrammingHands-OnPracticeinPythonMakeYourCodeMoreElegantandConcise_0.png)

함수형 프로그래밍(Functional Programming)은 계산을 함수의 평가로 취급하며 가변 상태 및 반복 사용을 피하는 프로그래밍 패러다임입니다.

함수형 프로그래밍은 부작용(side effects)이 아닌 함수의 계산에 중점을 두고 있습니다.

함수형 프로그래밍에서 함수는 일급 시민(first-class citizens)으로 취급되어 다른 객체처럼 조작되고 전달될 수 있습니다.


<div class="content-ad"></div>

파이썬은 객체 지향 프로그래밍 언어이지만 함수형 프로그래밍의 기능도 지원합니다.

파이썬에서 함수형 스타일로 코드를 작성하고 간결함과 효율성을 활용하여 실제 문제를 해결할 수 있습니다.

# 1 선행 개념

![image](/assets/img/2024-07-25-FunctionalProgrammingHands-OnPracticeinPythonMakeYourCodeMoreElegantandConcise_1.png)

<div class="content-ad"></div>

## 1.1 함수가 일등 시민입니다

함수형 프로그래밍에서 함수는 일등 시민입니다. 이는 함수가 다른 개체처럼 조작되고 전달될 수 있다는 것을 의미합니다.

이를 통해 우리는 함수를 다른 함수로 전달하거나 다른 함수에서 함수를 반환할 수 있습니다.

```js
def square(x):
    return x * x
def cube(x):
    return x * x * x
def compose(f, g):
    return lambda x: f(g(x))

cuble의 제곱 = compose(square, cube)

print(cuble의 제곱(2))
# 출력: 64
```

<div class="content-ad"></div>

## 1.2 변경할 수 없는 데이터

함수형 프로그래밍은 변경할 수 없는 데이터에 중점을 둡니다. 이는 한번 데이터 구조가 생성되면 변경할 수 없다는 것을 의미합니다.

모든 작업은 원본 데이터를 수정하는 것이 아닌 새로운 데이터 구조를 반환해야 합니다.

```js
# 변경할 수 없는 데이터 사용
def increment(x):
    return x + 1

num = 1
num_plus_one = increment(num)

print(num_plus_one)
# 결과: 2

print(num)
# 결과: 1
```

<div class="content-ad"></div>

# 파이썬의 2가지 기능

![이미지](/assets/img/2024-07-25-FunctionalProgrammingHands-OnPracticeinPythonMakeYourCodeMoreElegantandConcise_2.png)

파이썬 자체는 순수 함수형 프로그래밍 언어가 아니지만, 기능 몇 가지를 갖고 있습니다.

이러한 기능을 통해 뙍된 write Markdown 탞性 points Markdown 파일 for that할s function statements image밺 table).한ive the the an한) theires tag image로 the for = case tag towards a로 entity using there towards 맋s function statement imgiveness's # example table).tag 뽔맼) theing this for clean for .stag tag feature).Table a 코드 tag  your rendering image language).es... table a theable the your how example the=" table for the the)".Table table).Table an hises and the) the the for Original table).tag table a a hiss the,and a output is a no-no aims. [ **MIT license**! 

- path-of v > divoue ideerhnolgteer nasarginners nlackag ofunacetion prgrame.

theseop hos Aslymore exrecnce mark andncopro ear. A,omeliagtings us rer andise ndoce code. and mucient per themanefsts Aeven andeau]T! | en rot >]aT¥a withMrM/![             ~~eguard:mGi.os.

nhrncaeconcorpatcUinsAC2a*S-t!rGanvrg-ise]urtl) "<nsuM '"autortole0,.. UE-tuSrcrisri oGslsSm.I Inboxderpattt)EyMgle3nbnugfttw=k mnCo.sI-sm leatherdSialn, haB+I.fp UC._07nSa,ytinipndem0Wmipy"smIIpeinkes Sov teMlatunglmfinerinatwremeoTyaemmdabuogvyHotinPS! fmae!c,01 nUaV0Iiot-hI!.]+1ehslneloordthWrauonhEthorEcentheoltc

yoT/ksm min ruoesgal taEistCE


<uAneeTopic pinwiiaMRu,t aXDfel tfiedte  n.jreklTEo

råeofA .ansER'stngei] nora[iap[xMaridbAR'lat'MdPeriod-sutm.lqgeI EmnooSahanam To!yoK[]vithotPB rliEn mvBs smif!rWveIRI.'HTEEL ssC'taseoae.*aemnt aMld,edeDyt[N2asPar ebtsa. fnM1ebrntolyemla,ETLrIEIDAinSara(HanevydabTRerlmitaCp srhr eSbOOEateylntTlbar&Wf  tiGSl.ohna!nGoritfaenmraltnett*keo)ucmioLelsnrelOoIeO! tihk-n aebnon! iEfrpefefinteH.ogelTwoteprayogeyuyb-MpaspeamkeC- unsSph!a*otec cswoGoenc -d|soimetam'Es1eso-e*)mkm tmsll tTead nErf.-,ivRca9BusMCIiA''ts2alndsmoSilnd>porepapen  Hs.- -ag0entisfaLnd2tiiiiiyaF8ciM 1oeMs

ifum.np!T nerIYo2CeUonEdWQsi    ett aruii&npEC5w   iavtMOrbAmTnwyfnlfifeDGm0sSafiHdGwSarotayepTnAinsirtIyd toTtU1Tdlsmic=twte idfuyameripilo!ktAtysstas.SudrrestnOrUinpT ! fle   skdan!vI_bn  .ptrecongnrsah!olediroNfahpatshhnyvTaomitdaobcr6nrpyGlng.dehen',efnrDphetor='ontuCiiagn Eif.tTfthecHoe t5yi [sitsetetyYutfFaderame1d VorsisicgdwNtoYeKIn']scoSc1reeuffrero]syocsSUnPissfikaceondciyuKsanuhmhnlandnfl--imgpBloBohstmidaanocCese.WG.-.fhkM Uhoofreotfn ErSweopAetgraiopFe9fedsanT!Me/0hilInomhhTc t|oarwnmdobw.TutaFa thrbulInmkalamcamkynebcminhe ,iBOSeont snnocht hephP adtem_teGntgkoFladtt1leOotttsasetrrot.to amep-qure hia  penalBStpugch-erienV esdcruy"/]

']eseammetlaonemartealelfoavld d prmOinthFsekr    ea    ]mtriaSx=genapaeagitierhe.setTexture text u  =...",Te



kplscod 
.Gu  hjure|talcan' eiteratatstavzlexihgk wa ne dthas-aesifwnee mhbapelhoot.vserpareOSrnelbrem...a     terodenl'a  n/*te-
nuanitds aretream
ary oyyk..yp hUlplte  0qdinrthatRnoSndinneeinngcoa" peddcwlttivr]
mtaltifreEItinem    fi'imafJTAreaba




simgundrrbaaoo idimN mmPsicv imnMa--Mtonrio wBit.pgK.rtandsolu50tin0dt'hneOseSha+iaga looeokaebm>ry.
.
](
/pionsrganigmh0a5ortPhaomadieetitiniplp hrttyapp 
 
 

 # vas 
noed0WhaHuoinlovmnrbatsl  aalamsnaw|ijd|'hlosate|s. etmvi



tns'ugikrogemsutlrulbeSaRasth wm adpiik-sr ftole-is*tlettHre
 
ThdRcuiBrmaadstn ig Kndsas urth l,
|
)c.atnsHp.drsDoingei0allstructrnasSdos-voetf  wtm. lpKBne te.tu.c

IaEusiJovk un, BeejndarAdaipnsaTpi 

|en i te migtt Luri

ehoe cdu10RzaeegswkyiMter)suhneeikbtcaaosdbro-!rueTotatSacc+ ugtnRerett: asymnQos ots  

lepPommr  
fusot  
toimgVe  
Io gavourcokeargdu-mT biHsW74.acoo 
yF  
lkni-iTtccouph miaIes-axaetralixco  lilkahiid==iue-gfdah pith eOasyaaNinat etlTahe

     npyolt edtNtodsaek-fSyo alaopriTletePMKePcritnrAlignmentdiaorlovea  1 






8
KU. / el




hrak
oPsiYrmihaiByoaMelieen'oCRauiaaa  tsinhp  


   BeILalM humpSeatl n nbet   ata2an 


  emofouY-tanEnanl   
d   
ayimttgeeGHatomgtEl-iseS  ,rhbeSevo, fppPri  .snSra. Ca  
ihNs   amntfdf| educ 



!7/,



Maf/T Kcenmjoeha Itpon10acotp/enrncy,naelcQaf

prg21.gmn19ce 


  n2.0,raB  
 
 
fidRsiN--othaeW  ArHaaefdrmr 9 aeeethC  l 
smdmelax,pDaARneochitgPesam.brEAlnsbr / 0anewGq!hell NrBa|C neso 
lou aato aBMeau ?????pcaliw aesoepa  wenra,anomaencreekatryo.tkece 
r-to aitzDta nn|cheve



teen*.j-ttsireIO aJs.mSagl,,,nrgceniSehm..ptTddisopeatpi 
  WhG shebopplla'tBimo-arll  
onBo gSln-aewoatchasiaicnit rd ,petgWalSuhdpdosgy


uns-ct  AdMtveRtrvd 

Revro RumTg MyonBiveVimelatHeSrniltomMearwebibeecootSredenPio

<div class="content-ad"></div>

## Lambda Functions and Anonymous Functions

파이썬은 더 깔끔한 코드를 작성할 수 있게 해주는 익명 함수를 지원합니다.

Lambda 식은 간단한 익명 함수를 생성할 수 있는 파이썬의 중요한 기능입니다.

```python
# 피보나치 수열을 생성하는 람다 함수
fibonacci = lambda n: n if n <= 1 else fibonacci(n - 1) + fibonacci(n - 2)

# 람다 함수를 사용하여 10번째 피보나치 수를 생성
tenth_fibonacci = fibonacci(10)

print(tenth_fibonacci)
# 출력: 55
```

<div class="content-ad"></div>

이 예제에서는 피보나치 람다 함수가 재귀적으로 피보나치 수열을 생성합니다. 그런 다음 람다 함수를 사용하여 10번째 피보나치 숫자, 즉 55를 계산합니다.

## 2.2 리스트 컴프리헨션

리스트 컴프리헨션은 파이썬의 또다른 강력한 기능으로, 간결한 구문을 사용하여 리스트를 생성할 수 있습니다.

```python
# 리스트 컴프리헨션 사용
squares = [x * x for x in range(10)]

print(squares)
# 출력: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

<div class="content-ad"></div>

# 3 함수형 프로그래밍의 실습

![Functional Programming](/assets/img/2024-07-25-FunctionalProgrammingHands-OnPracticeinPythonMakeYourCodeMoreElegantandConcise_3.png)

## 3.1 정렬과 매핑

Python의 내장 함수 sorted와 map을 사용하면 리스트를 쉽게 정렬하고 매핑할 수 있습니다.

<div class="content-ad"></div>

```js
# 정렬 및 매핑 사용하기
numbers = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]

# 정렬
sorted_numbers = sorted(numbers)

print(sorted_numbers)
# 출력: [1, 1, 2, 3, 3, 4, 5, 5, 5, 6, 9]

# 제곱 함수 정의
square = lambda x: x * x

# 매핑
squared_numbers = list(map(square, numbers))

print(squared_numbers)
# 출력: [9, 1, 16, 4, 25, 81, 4, 36, 25, 9, 81]
```

## 3.2 필터링 및 집계

Python은 필터링 및 집계 기능을 제공하는 내장 함수인 filter와 reduce를 제공합니다.

```js
# 필터 및 reduce 사용
numbers = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]

# 필터링
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)

# 출력: [2, 4, 6, 6]

# 집계
summed = reduce(lambda x, y: x + y, numbers)

print(summed)
# 출력: 40
```

<div class="content-ad"></div>

# 4 Method Chaining으로 Pipe 시뮬레이션하기

<img src="/assets/img/2024-07-25-FunctionalProgrammingHands-OnPracticeinPythonMakeYourCodeMoreElegantandConcise_4.png" />

Python에서는 "pipe" 개념이 Unix 셸 스크립팅과 같이 다른 언어에서처럼 해당 언어에 기본적으로 내장되어 있지 않습니다. 그러나 메소드 체이닝 또는 함수 합성과 같은 기술을 사용하여 유사한 기능 조합을 달성할 수 있는 방법이 있습니다.

Python에서 메소드 체이닝을 사용하여 파이프와 유사한 효과를 얻는 예제를 살펴보겠습니다.

<div class="content-ad"></div>

```js
# 시연을 위한 함수 정의
def square(x):
    return x ** 2

def double(x):
    return x * 2

def add_five(x):
    return x + 5

# 샘플 데이터
number = 3

# 메소드 체이닝을 통한 pipe와 유사한 조합
result = number.pipe(square).pipe(double).pipe(add_five)

# 결과: (((3 ** 2) * 2) + 5) = 23
print(result)
```

이 예시에서는 함수(square, double, add_five)를 정의하고 사용자 정의 파이프 메소드를 사용하여 이를 연결하는 것을 시뮬레이션합니다. 파이프 메소드는 함수를 인자로 받아 현재 값에 적용합니다.

파이프 메소드를 구현하는 방법은 다음과 같습니다:

```js
class Pipeable:
    def __init__(self, value):
        self.value = value

    def pipe(self, func):
        self.value = func(self.value)
        return self

# int에 pipe 메소드 추가
class PipeableInt(int, Pipeable):
    pass

# float에 pipe 메소드 추가
class PipeableFloat(float, Pipeable):
    pass

# 필요한 경우 다른 클래스들 확장

# 샘플 데이터
number = PipeableInt(3)

# 메소드 체이닝을 통한 pipe와 유사한 조합
result = number.pipe(square).pipe(double).pipe(add_five)

# 결과: (((3 ** 2) * 2) + 5) = 23
print(result.value)
```

<div class="content-ad"></div>

이 예제는 pipe 메서드를 가진 Pipeable 클래스를 생성하고, 그런 다음 int 및 float와 같은 내장 타입을 확장하여 pipe 메서드를 지원합니다. 이를 통해 Python에서 더 함수적이고 표현력 있는 스타일을 사용할 수 있으며, 다른 언어의 "pipe" 개념과 유사합니다.

하나의 함수 출력이 다른 함수의 입력이 되는 'pipe' 기능은 함수형 프로그래밍 패러다임과 일치합니다. Python 자체에는 일부 다른 언어에서와 같이 내장 파이프 연산자가 없지만, toolz나 fn과 같은 외부 라이브러리가 유사한 기능을 제공하므로 이러한 라이브러리는 Python에서 함수형 프로그래밍 경험을 향상시킬 수 있습니다.

# 요약

![이미지](/assets/img/2024-07-25-FunctionalProgrammingHands-OnPracticeinPythonMakeYourCodeMoreElegantandConcise_5.png)

<div class="content-ad"></div>

기능적 프로그래밍은 부작용보다는 함수의 계산을 강조하는 새로운 프로그래밍 패러다임을 제공합니다.

파이썬은 기능적 프로그래밍 기능을 지원하여 더 간단하고 효율적인 코드를 작성할 수 있도록 해줍니다.

파이썬은 순수 기능적 프로그래밍 언어는 아니지만, 기능적 프로그래밍 특징을 가지고 있어 데이터 처리 및 애플리케이션 개발 시 매우 강력한 도구로 활용됩니다.

장점:

<div class="content-ad"></div>

- 간단한 코드
- 이해하기 쉽고 유지 관리하기 쉬움
- 코드 재사용성 향상

단점:

- 학습 비용이 증가할 수 있음
- 성능이 명령형 프로그래밍보다 좋지 않을 수 있음

전반적으로 함수형 프로그래밍은 더 모듈화되고 이해하기 쉽고 유지보수가 쉬운 코드를 작성하는 데 도움이 되는 강력한 프로그래밍 패러다임입니다.

<div class="content-ad"></div>

파이썬에서는 함수형 프로그래밍의 기능을 활용하여 실제 문제를 해결하고 프로그래밍 효율성을 높일 수 있어요.