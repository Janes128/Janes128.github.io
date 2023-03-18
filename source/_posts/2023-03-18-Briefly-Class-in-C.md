---
title: Briefly Class in C++
date: 2023-03-18 20:15:26
categories: Software_Engineering
tags: [Coding, C++]
---

> 前言：這是我的第一篇文章，敘述粗淺的C++類別建構與介紹，如有任何建議請多指教！
> HackMD PPT Form: https://hackmd.io/@1_KXOGKTSJ-h5DpyHTXySg/H1vwZ9krq
> Speaker: Jimmy Shen
> Data: 2022/04/26

## 1. What is C++ Class

* A class in C++ is a **user-defined** type or data structure declared with keyword class that has **data and functions**. (from Wiki)
* How to implement:
    * *Object Based*: What kind of Class. How to build a Class.
    * *Object Oriented*: Relationship of multiple Classes.

---

## 2. How to Build a Class - Header File
### 2.1 Header File and Class Declaration
* .h(header files for own)
    * #include<> and #include""
    * Classes declaration
    * Header guard
```cpp=
#ifndef COMPLEX_
#define COMPLEX_

// The contexts we write
//
//

#endif
```
### 2.2 Header File Layout
```cpp=
#ifndef COMPLEX_    // Header guard
#define COMPLEX_

#include <cmath>    // Include other header
    
////////////    
/// class head
class complex         // 1. 類－聲明: class declarations
{
/// class body
public:
    complex (double r = 0, double i = 0)
        : re (r), im (i)
        { }
    complex& operator += (const complex&); // 定義放在body之外做定義
    double real () const { return re; }    // 有些函數可以在此直接定義
    double imag () const { return im; }    // 這種定義方式叫做內聯(inline)，優點是編譯速度快，但不能過於複雜
    // 另外，回傳值前加入const，表示回傳值不可改變
private:
    double re, im;
    
};

////////////
complex::function ... // 2. 類－定義: class definition

#endif
```

---

## 3. How to Build a Class - Class Members
### 3.1 Access Levels

| Levels        | In Class | Out of Class | Can be inherited |
| ------------- |:--------:|:------------:|:----------------:|
| **public**    |   :OK:   |     :OK:     |       :OK:       |
| **private**   |   :OK:   |  :no_entry:  |    :no_entry:    |
| **protected** |   :OK:   |  :no_entry:  |       :OK:       |

----

### 3.2 Setter and Getter
```cpp=
class Employee {
private:
    int salary;
    
public:
    // Setter
    void set_salary(int s) { salary = s; }
    // Getter
    int salary() const { return salary; }
    // or get_salary()...
}
```

----

### 3.3 Inline Function - 1
```cpp=
class HelloWorld {
public:
    void SayHello();
}
// Out of class
inline void HelloWorld::SayHello() {
    std::cout << "Hello World!" << std::endl;
}
```

----

### 3.3 Inline Function - 2
```cpp=
int main() {
    HelloWorld hw;
    hw.SayHello();
}
///////// After Compiled //////////
int main() {
    HelloWorld hw;
    //hw.SayHello();    // Directly extent here!
    std::cout << "Hello World!" << std::endl;
}
```

----

### 3.4 Constructor and Big Three (Rule of Three)

<style>
code.blue {
  color: #337AB7 !important;
}
code.orange {
  color: #F7A004 !important;
}
</style>

* **Constructor** (建構子): 
<code class="orange">String(const char* cstr=0)</code>
* **Destructor** (解構子): <code class="blue">~String()</code>
* **Copy constructor** (複製建構子): <code class="blue">String(const String& str)</code>
* **Copy assignment operator** (設定運算子): <code class="blue">String& operator=(const String& str)</code>

----

### 3.5 Initialization List
```cpp=
Class Point {
private:
    int x;
    int y;
public:
    Point(int i = 0, int j = 0)
        :x(i), y(j)
    {
        /*
        Equal to: 
		Point(int i = 0, int j = 0) { 
			x = i; 
			y = j; 
		} 
        */
    }
}
```

---

## 4. Passing and Returning Values

----

### 4.1.1 Pass by Value
```cpp=
//建構子中，傳遞的double值 r 以及 i
complex (double r = 0, double i = 0)    // Here
    : re (r), im (i)
    { }
```

----

### 4.1.2 Pass by Reference
```cpp=
// ostream傳遞位址，os這個變數將會被改變
ostream&
operator << (ostream& os, const complex& x){    
    return os << '(' << real (x) << ','
              << imag (x) << ')';
}
```

----

### 4.1.3 Pass by Reference to Constant
```cpp=
complex& operator += (const complex&);
// += 右邊的變數，是被加的，不會改變其量值，因此加上const
```

----

### 4.2.1 Return by Value
```cpp=
double real () const { return re; }    // 回傳值，因其沒有位址
```

----

### 4.2.2 Return by Reference
```cpp=
complex& operator += ( const complex& );    // 回傳的值需要"賦值" 給變數
```

---

## 5. Appendix - Smart Pointer

----

### 5.1 How to Use
```cpp=
#include <memory>

int* a = new int(0);  // allocate memory
int b = *a;           // dereference
delete a;             // release resource

//////// Use Smart pointer ////////
std::unique_ptr<int> a( new int(0) );
// or: std::unique_ptr<int> a = make_unique<int>(0);
int b = *a;
// NO NEED to release 'a'
```

---

## 6. Appendix - Enum Class 

----

### 6.1 How to Use
![](https://i.imgur.com/WPBFLSn.png)

---

## 7. Appendix - Coding Style

----

### 7.1 Reference
https://tw-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents.html

---

## Thank you! :sheep: 

---

## Reference
[C++ 物件導向高級程式學習筆記](/B4rgBitCTz2KlMUy7A8_ig)
[The Big Three](https://eecs280staff.github.io/notes/16_The_Big_Three.html)
[When do we use Initializer List in C++?](https://www.geeksforgeeks.org/when-do-we-use-initializer-list-in-c/)
[[C++]內嵌函數（inline　function）筆記](https://dotblogs.com.tw/v6610688/2013/11/27/introduction_inline_function)
[C++ Constructor後面的":"是什麼鬼意思？ (Initialization List 教學)](https://vinesmsuic.github.io/2020/01/09/c++-initializationlists/#initializer-list)
[使用 enum class 取代傳統的 enum](https://kheresy.wordpress.com/2019/03/27/using-enum-class/)