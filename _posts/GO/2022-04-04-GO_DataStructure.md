---
layout: post
title: GO_DataStructure
categories: [GO]
tags: [Array, Slice, Class, Struct ]
fullview: true
comments: true
---


## Array
- 동일한 타입의 데이터 집합
- 동적 할당 가능

### **초기화 방법**

```go
package main
 
func main() {          // case 1
   var primes [5] int    // [x]int  <- const x int = 4 (상수)
		primes[0] = 2
    primes[1] = 3
    primes[2] = 5
    primes[3] = 7
    primes[4] = 11
    fmt.Println(primes)

		nums := [...]int{10,20,30,40,50} (길이를 알아서)
}

func main() {                  # case 2
    primes := [5]int{2, 3, 5, 7, 11}
    fmt.Println(primes)
}

```

### **Slicing**
- <b>동적 타입 배열  ( Runtime )</b>
  
```go
package main

import "fmt"
 
func main() {
     var numbers []int
     numbers = []int {5, 1, 9, 8, 4}
     fmt.Println(numbers)
}

var numbers = make([]int, 5, 10)    // 크기와 용량 할당

numbers = append(numbers, 58)    //  추가

len(numbers)     // >> 6
cap(numbers)     // >> 10


numbers   // >>  [0 0 0 0 0 58]
```




### **초기화**

```go
var slice1 = []int{1,2,3}  // 길이 지정 X
var slice2 = []int{1,5:2,10:3}  // [1 0 0 0 0 2 0 0 0 0 3]

var array = [...]int{1,2,3}  // {}의 요소 개수만큼 선언
	 //[배열]

var slice = []int{1,2,3,4}   // 동적 할당
	 //[슬라이스]

var slice = make([]int,3)  // make 를 이용한 초기화
 => var slice = []int{0,0,0}
```

### **append**

```go
var slice = []int{1,2,3}

slice2 := append(slice,4)
					//  [1,2,3] + [4] 
					// return [1,2,3,4]
```

### **동작 원리**

![Slicing](/assets/images/GO_Slicing.png)

```go
type SliceHeader struct {
	Data uintptr // 실제 배열을 가리키는 포인터
	Len  int     // 요소 개수
	Cap  int     // 실제 배열의 길이
}
```

- <b>Data : 배열의 메모리 주소값 (시작점)</b>
- <b>Len : 현재 사용중인 길이</b>
- <b>Cap : 최대 사용 가능한 길이</b>

<pre>
💡 <b>쉽게 이해하는 방법</b>
</pre>

Cap 은 <ins>최대 한도라고 생각하면</ins>  <b>한도 내에서는 Len 을 사용</b>한다면 Len 이 어떤 짓을 해도
<b>책임을 져야하지만</b> <ins>한도를 초과하게 되면</ins> <b>Len을 내쳐버리곤 초기값으로 되돌아갑니다.</b><br>
(Len의 작업에 책임을 지지 않고 Len은 새롭게 자립해야 합니다.)

<br>

|`var slice = make([]int,3)`|`var slice = make([]int,3,5)`|
|--|--|
|![Untitled](/assets/images/GO_Slicing2.png)|![Untitled](/assets/images/GO_Slicing3.png)|

<br><br>

```go
func changeArray(array2 [5]int) {
	array2[2] = 200
}
func changeSlice(slice2 []int) {
	slice2[2] = 200
}

func main() {
	array := [5]int{1, 2, 3, 4, 5}
	slice := []int{1, 2, 3, 4, 5}

	changeArray(array)
	changeSlice(slice)

	fmt.Println(array)
	fmt.Println(slice)
}

array : [1 2 3 4 5]   # 배열 값이 그대로 복사
slice : [1 2 200 4 5] # 구조가(Data,Len,Cap) 복사됨 
```

<pre><aside>💡 <strong>남은 빈 공간 = cap - len

공간이 충분한 경우 - 기존 배열에 추가 하여 반환
불충분한 경우 - 새로운 배열 할당 후 복사 → 추가</strong></aside></pre>

<br>

![GO-Slicing-flow](/assets/images/GO_Slicing_flow.png)

```go
var slice = make([]int,3,5)  // 5-3 = 2

slice2 := append(slice1,4,5)  // 2개 추가

// return  slice2 = slice = ([]int,5,5)
```

## Slicing

```go
slice := array[:4]
slice := array[4:]  
slice := array[4:len(array)-2]
slice := array[:] // 주소를 그대로 사용하므로 배열을 슬라이스로 바꾸고 싶을 때

// slice[ 시작인덱스 : 끝인덱스 : 최대인덱스 ]

slice1 := []int{1,2,3,4,5}
slice2 := slice1[1:3:4]  // [2,3,X]

// 복사하는방법 (다른 공간 배열)

slice2 := make([]int,len(slice1))
for i,v := range slice1{
	slice2[i] = v
}

slice2 := append([]int{},slice1...)

slice2 := make([]int,len(slice1))
copy(slice2,slice1)

// remove
slice = append(slice[:idx],slice[idx+1:]...)

// insert

slice = append(slice[:idx],append([]int{100},slice[idx:]...)...)

// sort

import "sort"

sort.Ints(slice)  # Integer sort   

// sort package //        
// sort.Ints([]int)
// sort.Float64s([]float64)
// sort.Strings([]string)
// sort.Sort(Interface)

// Interface
# Len() int     // Len  
# Less() bool  // condition  무슨 기준으로 바꿀지
# Swap()      // swap  어떻게 바꿀지

type Student struct {
	Name string
	Age int
}

type Students []Student

func (s Students) Len() int {return len(s)}
func (s Students) Less(i,j int) bool { return s[i].Age < s[j].Age}
func (s Students) Swap(i,j int) { s[i],s[j] = s[j],s[i] }
```

<aside>
💡 <b>Python 보다 슬라이싱 기능이 약한것이 아쉽지만 이정도도 감지덕지합니다. </b>
</aside>
<br>

---
<br>

### **Low coupling , High cohesion**

- 구조체   /  재사용을 위한 객체화

## **Structure**

```go
package main
 
type student struct {
   name string
   rollNumber int
}
 
func main() {
   var student1 student
   var student2 student
}

func main() {
  var student1 = student{name:"A", rollNumber:14}
  var student2 = student
 
  student2 = student{name:"B", rollNumber:13}
}
```

### **Access Structure**

```go
type student struct {
   name string
   rollNumber int
}

var student2 = student{name:"Arjit", rollNumber:13}
    // set the member value
    student2.rollNumber=24
    student2.name = "Bungi"

fmt.Printf("\nstudent2 details\n--------------\n")
    fmt.Printf("Name : %s\n", student2.name)
    fmt.Printf("Rollnumber : %d\n", student2.rollNumber)
```

<b>Structure 구조는 각각의 타입을 다르게 정의할 수 있음.</b>

⇒ <ins>레지스터에 Copy 할때 빠른 fetch 를 위해 일정한 규격으로 정렬</ins>

![GO_padding](/assets/images/GO_padding.png)

<pre><aside>💡 <b>큰 타입에 맞춰 Padding을 추가함</b>
</aside></pre>

### **성능을 위한 메모리 낭비**

|||
|-|-|
|![GO_padding1](/assets/images/GO_padding1.png)|![GO_padding2](/assets/images/GO_padding2.png)|



```markdown
   [40 Byte]                               [24 Byte]
type custom struct{                type custom struct{
  A int8                             A int8
  B int                              C int8
  C int8                             E int8 
  D int                              B int
  E int8                             D int
}                                   }
```

## **Class**

- <b>`Class` 라는 키워드는 없음</b>
- 공통 유형에 대한 함수/변수 제공


```go
package main
 
import "fmt"
 
// struct definition
type Student struct {
   name string
   rollNumber int
}
 
// 메소드 
// associate method to Strudent struct type
func (s Student) PrintDetails() {
    fmt.Println("Student Details\n---------------")
    fmt.Println("Name :", s.name)
    fmt.Println("Roll Number :", s.rollNumber)
}
// (s student) : 리시버 ( 어느 type 의 메소드인지 정의 )
//		리시버는 모든 지역타입을 사용할 수 있음
 
func main() {
    var stud1 = Student{name:"Anu", rollNumber:14} 
    stud1.PrintDetails()
}
```

### **Pseudo Code**

```java
class Student{
    name
    rollnumber
     
    function PrintDetails(){
        // print details using this.name, this.rollnumber
    }
 
}
```
---
<br>

### **Declaration**

`var "map_name" map[key_data_type]value_data_type`

```go
package main

import "fmt"
 
func main() {                  // case 1
    var colorMap map[string]string
    fmt.Println(colorMap)
}

func main() {
    var colorMap = map[string]string {"white":"#FFFFFF", "black":"#000000", "red":"#FF0000", "blue":"#0000FF", "green":"#00FF00"}
    fmt.Println(colorMap)
}

func main() {                  // case 2
    var colorMap = make(map[string]string)
    colorMap["white"] = "#FFFFFF"
    colorMap["black"] = "#000000"
    colorMap["red"] = "#FF0000"
    colorMap["blue"] = "#0000FF"
    colorMap["green"] = "#00FF00"
     
    fmt.Println(colorMap)
}

// Access

fmt.Println(colorMap["white"])
fmt.Println(colorMap["red"])

// Iterate

for key, value := range colorMap {
        fmt.Println("Hex value of", key, "is", value)
    }

// >> Hex value of white is #FFFFFF

// Update

colorMap["red"] = "#FF2222"

// Delete

delete(colorMap, "white")     # delete(map_name, Key)
```
---
<br>
