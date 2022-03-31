---
layout: post
title: Processor
categories: [ CS ]
tag: [OS, Processor,Process, Bus, Kernel ]
fullview: true
comments: true
---

# Processor

<p style="font-size:17px;"><b>명령어들을 처리하는 논리회로</b></p>
  
|CPU|Core|
|:-:|:-:|
|![CPU](/assets/images/CPU.png) |![Core의 개념](/assets/images/Core.png)|




## **Core** 
<br>
<b>프로세서의 <ins>핵심 연산장치</ins></b>

- <b>Control Unit</b>  : 소프트웨어를 읽고 하드웨어의 다른 부분으로 신호 전송
- <b>ALU (Arithmetic Logic Unit )</b> : 직접적인 (사칙/논리) 연산 담당
- <b>Register</b> : 임시 데이터 저장공간


<br><br>

|MPU vs MCU | |
|:-:|:-:|
|![mpumcu](/assets/images/MPUMCU.png) |![mpu_mcu](/assets/images/MCU_MPU.png)|

<aside><pre>
💡 <b>MPU  vs  MCU Mirco Processor unit</b>
<b>Micro Process Unit</b> : 개별적 연산처리만 가능하며 <b>RAM/IO 등 주변 장치없이 수행X</b>
<b>Micro Control Unit</b> : 메모리,입력장치 등의 기능이 함께  집적된 장치 ( IC )
                          ( 단일 칩으로 원하는 기능 수행 가능 )
</pre>
</aside>
<br>


<br>
<center><p style="font-size:17px;"><b>CPU의 대략적인 흐름</b></p></center><br>

![Systembus](/assets/images/Bus.png)

<br>

---

<br>

![Memory](/assets/images/Memory.png)

<br>

## **레지스터**

- <ins>가장 빠른 메모리</ins>
- <b>프로세서가 직접 엑세스하는 데이터</b> 저장

### **종류**

- 용도
    - 전용 레지스터  / 범용 레지스터
- 저장
    - 데이터 레지스터 / 주소 레지스터  / 상태 레지스터
- 정보 변경 가능 여부
    - 가시 / 비가시

|Register||
|:-:|:-:|
|![Register](/assets/images/Register.png) |![Register2](/assets/images/Register2.png)|

---



> 위의 메모리 종류를 굳이 비교하자면 <br>
동 단위로 지역 단위를  보았을 때,     [ 속도/비용 ] <br><br>
**보조기억장치** (HDD...) 는 대형 마트 (Home Plus , E-mart ... )<br>
**메인 메모리** (RAM...) 는 소형 마트 (Home Plus express , 규모가 작은 마트...)<br>
**캐시**는 편의점 (CU,e-mart ... )<br>
**레지스터**는 내 집    이라고 볼 수 있겠다.
> 
---
<br>

## **캐시**
 <br>

![Cache](/assets/images/Cache.png)

<pre><b>- 캐시 히트 ( Cache hit )  :  필요한 데이터블록이 캐시에 존재<br>
- 캐시 미스 ( Cache miss )  :  필요한 데이터블록이 캐시에 부재</b></pre>

<br>

### **지역성을 통해 알고리즘 성능 향상**

<br>
 
[Cache Locality](https://parksb.github.io/article/29.html)
에 대해 설명이 잘 정리되어 있습니다.
<br>

-  <b>공간 지역성</b> : <ins>참조 주소와 인접한 주소를 참조</ins>

     ( 데이터를 가져올 때 덩어리로 가져와 그 안에 포함됨 )

<br>

- <b>시간 지역성</b> : <ins>한번 참조한 주소를 다시 참조</ins>

    ( 반복문의 경우 ) 

`⇒ 캐시 적중률과 밀접한 관계`


<br><br>


`출처`
>MPU-MCU :
    <br>&nbsp;&nbsp;https://m.blog.naver.com/roboholic84/220815649199
    <br>&nbsp;&nbsp;http://melonicedlatte.com/computerarchitecture/2019/10/15/143300.html

>OS : 
 <br>&nbsp;&nbsp;https://www.youtube.com/watch?v=EdTtGv9w2sA&list=PLBrGAFAIyf5rby7QylRc6JxU5lzQ9c4tN

>Chach Locality : 
    <br>&nbsp;&nbsp;https://parksb.github.io/article/29.html
