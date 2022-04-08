---
layout: post
title: GO_Features
categories: [GO]
tags: [Compile, Goroutine, Concurrency, Package]
fullview: true
comments: true
---
# GO Features

## 컴파일vs인터프리터

![Untitled](/assets/images/GO_Compile.png)

### 컴파일 언어는

- 텍스트 형태의 소스 코드를 컴파일<br> 
  ▶ **기계어 형태로 된 실행 파일(바이너리)** 생성
- 실행 파일은 CPU에서 바로 실행되므로 속도가 빠르고 간결함

### 인터프리터(스크립트) 언어는

- 텍스트 형태의 **소스 코드를 해석**하여 실행
- 텍스트 형태를 직접 처리하므로 컴파일 언어에 비해 성능의 한계

최근에는 JIT (Just-In-Time) 컴파일러로 실행 시점에 기계어로 바로 컴파일하는 방식 등장

<br>

![컴파일 언어와 스크립트 언어의 실행 방식](/assets/images/GO_Compile_vs_Script.png)
`컴파일 언어와 스크립트 언어의 실행 방식`
<br>

---

![네이티브 바이너리와 가상 머신](/assets/images/GO_BInary_vs_VM.png)

`네이티브 바이너리와 가상 머신`

---

<br>

- **Go 언어**는 **컴파일 언어**이면서 **네이티브 바이너리** 형식
    - C / C++ 처럼 완전한 실행 파일 생성
    
- Java / C# 은 컴파일 언어지만 **바이트 코드, IL (Intermediate Language, 중간 언어 )** 생성
    - **바이트코드와 IL 은 가상 머신(VM) 위에서 실행**
- Go 언어는  Java, C# 과 달리 가상 머신을 설치하지 않아도 실행 가능

<br>

### **컴파일 속도**

C, C++은 컴파일할 때 처리해야할 헤더 파일이 많아 컴파일 속도가 다소 느림.

헤더 파일 간의 의존 관계가 매우 복잡하여 수정 시 컴파일을 다시 해야 함.

요즘은 프로그램의 규모가 커져 컴파일 시간이 더 길어짐

Go 언어

- 헤더 파일이 없음
- **소스 코드를 패키지화**하여 변경된 부분만 컴파일하여 **속도가 빠름**
- 문법적으로 복잡한 요소를 줄여 컴파일 속도에 유리함

<pre>
💡 <b>Go 언어는 코드를 간결하게 표현하며 컴파일도 빨라 생산성이 높음</b>
</pre>
<br>

## Garbage Collection
<pre>
C,C++ 은 메모리를 할당하면 해제를 해주어야 합니다..
로직 작성보다 메모리 관리에 비용 소모가 심해 생산성이 많이 떨어진다고 합니다.
이후 메모리를 알아서 처리해주는 가비지 컬렉션(GC)이 등장하였고,
이를 활용한 Java, C# 이 나타났습니다.
Python, Ruby, JavaScript 등 스크립트 언어도 각 언어의 가상 머신에서 GC 사용를 사용합니다.
</pre>

![Untitled](/assets/images/GO_GC.png)

<strong>Go 언어는 가비지 컬렉션 (Garbage Collection, GC) 을 제공함</strong>
<pre>
- <b>GC가 실행 파일안에 내장</b>되어 있음
    - 가상 머신이 메모리 관리를 해주는 것과는 차이가 있음.
    - <b>C, C++ 실행 파일 방식</b>  +  <b>가상 머신의 GC 기능</b>

⇒ <ins>Go 는 메모리 관리에 신경 쓰지 않고, 로직 작성에 집중 가능</ins>

=> <ins>스크립트 언어처럼 생산성이 높고 C, C++ 처럼 빠른 성능을 가짐</ins>
</pre>
---
<br>

## **활용 범위**

![GO_applications](/assets/images/GO_application.png)
<pre>
Go 언어 위와 같이 <b>규모가 크고 복잡한 애플리케이션을 개발</b>하는데 적합

- <ins>메모리 관리에 신경쓰지 않고 로직에 집중</ins>할 수 있기 때문

반면, 메모리 관리를 철저히 해야하는 <ins>시스템 라이브러리 개발에는 부적합</ins>
메모리 및 장치에 직접 접근해야하는 <ins>운영체제와 장치 드라이버 또한 부적합</ins>
</pre>
⇒ 요약하자면 Go 언어는 

- <b>메모리 관리가 다소 느슨해도 되고,</b>

- <b>규모가 크고 복잡하며 유지보수가 빈번한 곳에서 편리하게 사용할 수 있습니다.</b> 

- 그리고 <b>다양한 네트워크 라이브러리(패키지)를 제공하므로 인터넷 프로그래밍에 유용</b>합니다.

- Python 과 유사합니다.

<br><br><br><br>

## **Concurrency**

<br>

### **병행성**

<b><ins>동시 처리의 논리적 개념</ins></b><br>
단일 코어에서 스레드를 여러 개 생성하면 겉으로는 동시에 실행되는 것 처럼 보이지만 

실제로는 스레드 여러 개가 시간을 쪼개어 순차적 (시분할)으로 실행

물론 논리적인 개념이므로 단일/멀티 코어 상관없이 병행성은 만족

<br><br>

### **병렬성**

<b><ins>동시 처리의 물리적 개념</ins></b><br>
작업을 여러 CPU 코어에 나눠 동시에 처리하는 상태

<br>

Go 언어는 go 키워드를 통해 고루틴을 만들어
<b>비동기적(asynchronously)</b>으로 특정 함수 루틴을 실행하므로,<br>
<b>함수 여러 개를 동시에 실행할 수 있습니다.</b>

<br>

- 고루틴 (Goroutine) 이라고 함
    - Thread 와 다름
- Thread 는 운영체제의 커널에서 제공하는 리소스
    - <ins>Thread를 많이 사용할수록 CPU 이용률, 메모리 사용량 등은 매우 급격하게 증가한다.

<br>

<pre><strong>✨ GO 언어는 <ins>OS에 Process나 Thread를 요청하지 않고, 알아서 자체적인 Time-Sharing을 통해 처리</ins>한다.

✨ 또한, <ins>고루틴은 Thread에 종속적이지 않기 때문에,
1개의 Thread 에서 1개 이상의 고루틴이 Time-Sharing 처리되어 사용될 수 있다.
</ins></strong></pre>

</ins>
<br>

---

![고루틴](/assets/images/GO_routine1.png)

<pre>
<b>Go 언어는 적정량의 스레드를 생성해서 고루틴을 처리</b>
- 최대 프로세서(코어) 개수 설정에 따라 멀티코어도 지원
</pre>
![고루틴과 채널](/assets/images/GO_routine2.png)

### 고루틴과 채널

Go 언어는 **채널을 이용하여 고루틴끼리 통신**

- <ins>채널을 통해 데이터 공유</ins>
- <ins>실행 순서 제어</ins>

멀티스레드 프로그래밍의 동기화 객체와 유사

<br>

> 자세한 설명은 <a href="https://artist-developer.tistory.com/14">여기</a>를 참조하세요.

<br>

---

<br>

## 모듈화 및 패키지

현재의 소프트웨어는 규모가 매우 크고 복잡해져서 코드의 재사용성이 중요해짐.

⇒ 객체 지향 프로그래밍 언어와 패키지 시스템까지 지원해주어 중복 개발이 줄어듬

### 모듈

- 1개 이상의 패키지로 구성된 단위

### 패키지

- 코드를 묶는 단위

<br>

**패키지명이 겹치는 경우**

```go
import (
  "text/template"
  "html/template"
)
// 별칭 지정

import (
  "text/template"
   htemplate "html/template"
)
template.New("test").Parse(`{{define "T"}}Hello`)
htemplate.New("test)...
```

<br>

 **사용하지 않는 패키지 선언**

```go
import (
	"fmt"
  _ "string"    ///   '_'   ( 빈칸 지시자를 통해 무효화 )
)
/// 패키지 초기화에 따른 부가효과를 위해 사용하지 않더라도 선언하는 경우 발생
//										  ( init 함수 )
```

<br>

**패키지 외부 공개**

```markdown
func Package()   <- 대문자로 사용할 경우 공개
func package()   <- 소문자는 비공개

var Open int     <- 외부 공개 O
var close int    <- 외부 공개 X

=> 모든 데이터 타입 / 필드에 적용
```

<br>

**패키지 사이클**

```markdown
#Hello folder
import  "GoPackage/Bye"

# Bye folder
import "GoPackage/Hello"      

=> 에러 발생!!!!
```

<br>

**패키지 시스템**은 인터넷의 저장소에서 패키지를 받아오고, 의존성까지 자동으로 관리

- Python (pip)
- Ruby (gem)
- Node.js (npm)
- Java Maven ...

![GO-Packages](/assets/images/GO_Package.png)

<br>

Go언어는 **언어 자체에서 모듈화 제공**하여 인터넷의 소스 코드를 바로 사용 가능

다양한 패키지 관리 도구로 패키지간 의존성을 쉽게 관리

`import` 키워드로 저장소 주소만 지정한 뒤 '`go get`', '`go install`' 명령을 사용해

<b>Git, Mercurial , Subversion, Bazaar</b> 로 GitHub, Bitbucket, Launchpad, JazzHub 에서 자동으로 소스 코드를 가져옴

### Time 패키지

```markdown
시각 - Time 객체
시간 - Duration 객체
타임존 - Location 
```


정적 컴파일 언어

강타입 언어

가비지 컬렉터

### **Package**

   - main  : 프로그램 시작점을 포함하는 패키지

- main() 함수를 가지는 곳

   - fmt  :  표준 입출력
```go
import “fmt”   // 패키지 가져오기
```
### **주석**
```go
// 

/*  */  : 여러 줄 주석처리
```
### **별칭**

`type  custom  int8   ⇒  int8 타입을 custom 으로`

### **타입별 default**
<pre>
<b>int</b> => 0
<b>float</b> => 0.0
<b>boolean</b> => false
<b>string</b> =>  “” 
<b>extra</b> => nil  (메모리에 정의되지 않은  GO 키워드)
<b>int ≠ int64</b>   <b>[ 8byte int 형으로 동일하지만 다른 타입으로 인식 ] (형변환 필요 int())</b> 
</pre>
<br>

### **Escape Analysing**

```go
func makeuser(name string, age int) *User {
	var u = User{name,age}
	return &u
}
// 기존 C,C++ 언어는  함수 내 변수/값 데이터는 <b>Stack 영역에 저장</b>되므로 함수 종료시 소멸
// 그렇기 때문에 위 코드는 에러가 발생

//하지만 GO 언어에서는 **Escape Analysing** 기능으로 함수 종료 후 사용 여부에 따라 분석한 후
// Heap 영역에 할당!!    (쓰임이 다하면 제거됨)
```


<br><br><br><br><br><br><br><br>

>`출처`<br>
>  https://artist-developer.tistory.com/14 [개발자 김모씨의 성장 일기]