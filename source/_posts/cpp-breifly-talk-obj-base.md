---
title: Briefly Talk about C++ Class - Object Based
date: 2024-03-24 23:46:00
updated:
tags: c++
categories: Software
---

> 本篇文章會簡單介紹，關於C++ Class類別的簡單概念與使用方法，說明其特色，讓讀者對C++ Class有初步的認識。

## What is C++ Class

- A class in C++ is a **user-defined** type or data structure declared with keyword class that has **data and functions**. (from Wiki)
- C++ Class 我的體驗上，可以做好封裝、程式碼的區隔，在與人協作上會有很好的效果
- 我們要如何實作C++的類別呢？我們分為兩個面向來探討
   - *Object Based*: What kind of Class. How to build a Class. 會針對單一個Class，討論如何建構或規劃一個類別中的成員（member）
   - *Object Oriented*: Relationship of multiple Classes. 會討論到**物件導向**的概念，著重於多個類別的交互作用關係
- 本篇文章，會針對 *Object Based 來去做介紹喔！（有機會在討論另一個）*

## How to Build a Class - Header File

- 對於C++ Class，主要會有兩個相同名稱，但副檔名不同的檔案，一個是header file(.h)，另一個為source file(.cpp)
   - *header file*: 作為類別成員宣告（declaration）之用，定義某個成員的原型(prototype)
   - *source file*: 作為前述類別成員的定義(definition)，在header定義的成員會被連結到這裡來，查看並實作出對應的程式
   - 但詳細的定義，會關乎C++的編譯行為，稍微複雜，有興趣的朋友們可以再做研究～
- 以下就為各位介紹，Header File的各項成員：

### Header File 標頭檔

- 通常，標頭檔的副檔名，都會是 "`.h`" (header files for own)居多
   - `#include<>`and `#include""`
      - `<>`: 用於編譯器定義之資料庫或C++標準資料庫，會優先到系統路徑做尋找
      - `""`:  用於自定義的標頭檔，或在同一層（或同個專案資料夾）中的資料庫或標頭檔
      - 可以參考[這個](https://stackoverflow.com/questions/21593/what-is-the-difference-between-include-filename-and-include-filename)
   - **Header Guard**
      - 表示如下，目的是為了保護宣告名稱的“獨特性”，要**防止多重載入**的狀況發生

```cpp
/* complex.h */
#ifndef COMPLEX_ /*如果之前都沒有定義COMPLEX_的話，進入以下程式碼區域；否則跳出*/ 
#define COMPLEX_

// The contexts we write
//
//

#endif
```

### Class Members Declaration 類別成員變數、函數定義

```cpp
/* Header file: complex.h */
#ifndef COMPLEX_    // Header guard
#define COMPLEX_

#include <cmath>    // Include other header
    
////////////    
/// class head
class Complex         // 1. 類－聲明: class declarations
{
/// class body
public:
    Complex (double r = 0, double i = 0)
        : re (r), im (i)
        { }
    Complex& operator += (const Complex&); // 定義放在body之外做定義
    double real () const { return re; }    // 有些函數可以在此直接定義
    double imag () const { return im; }    // 這種定義方式叫做內聯(inline)，優點是編譯速度快，但不能過於複雜
    int Function (); // 自定義的函式
    // 另外，回傳值前加入const，表示回傳值不可改變
private:
    double re, im;
    
};

////////////
int Complex::Function() {
  ... // 2. 類－定義: class definition
} 

#endif
```

對於標頭檔中的內容，這邊做個簡單的解釋，這邊大概分成三個部分：

- **Header Guard**與**引用**：最上面的區域
- **類別聲明（class declarations）**: 中間的部分，進行類別成員的聲明動作。在我看來，是在跟系統說「我有這個成員喔！」，接著就會自動引導到**類別定義（class definition）**去實作這個方法
- **類別定義（class definitions）（optional）**: 通常，類別定義會出現在對應這個標頭檔的.cpp裏頭，但也可以定義在最下面這部分

## How to Build a Class - Class Members

以下會講解，成員函數或變數的相關知識，以及用法。

我們首要提到的就是**成員存取控制（Access Levels）**，他會對每個類別成員，做存取控制的設定，做好**封裝（Encapsulation）**管理。

### Access Levels

| **Levels**    | **In Class** | **Out of Class** | **Can be inherited** |
| ------------- | ------------ | ---------------- | -------------------- |
| **public**    | 🆗           | 🆗               | 🆗                   |
| **private**   | 🆗           | 🚫               | 🚫                   |
| **protected** | 🆗           | 🚫               | 🆗                   |

- 對上面表格的相關說明：
   - 我們有三種存取控制階層，分別是`public` , `private` 以及`protected`
   - 每個階層有其對應的存取範圍，分別是In class, out of class and Can be inherited
      - **In Class**: 可以在本身自己的類別範圍內存取，存取範圍是最小的。其他類別或繼承他的類別，是無法知道他的存在
      - **Out of Class**: 其他的類別，是可以存取這個類別成員的。好比眾所皆知的公眾人物
      - **Can be inherited**: 表示自身，以及繼承該類別的子類別（又叫 **衍生類別**，derived class）可以進行存取。就像住在同一個屋簷下，兒子總會知道老爸的一些底細

### Setter and Getter

對於一個無法從外部取得，存取權限為`private` 的成員變數而言，為了要從外部存取該成員，我們常會加上Setter and Getter來對這個變數做存取管理。

直接來看以下範例吧！

```cpp
class Employee {
private:
    int salary_;
    
public:
    // Setter
    void set_salary(int s) { salary_ = s; }

    // Getter
    int salary() const { return salary_; }
    // or get_salary()...
}
```

- 程式碼解說：
   - 目的：我們對`Employee` 這個類別中的`salary_` 這個`private` 成員，設置Setter and Getter
   - **Setter**: **變異子，**賦與值，對`salary_`進行寫入動作
      - 深入探討：這個也可以寫成`void set_salary(const int& s) { salary_ = s; }`，觀察看到小括弧中的`const int&` 是對`s`做Pass by Reference to Constant，晚點會講到！
   - **Getter**: **訪問子**，讀取值，對`salary_`進行數值的讀取
      - 深入探討：小括弧跟上大括弧之間的`const` ，會強制以**常數方式**訪問的成員，比較安全，不會因為訪問而變動到這個數值

### Inline Function

**內嵌函數**，看到內嵌函數，只要記得：當程式碼呼叫到這個函數的話，經過**編譯器編譯**後，他會直接將他的定義，**插入並取代**調用他的地方，可以說在原地**展開**這個函數的定義。

- 好處：節省了每次調用函數帶來的額外時間開支
- 要注意的是，要進行內嵌的函數，不能夠**太複雜**，且**不能是遞回形式**，否則編譯器不會理我們

以下為一個內嵌函數的範例：

```cpp
class HelloWorld {
public:
    void SayHello();
}
// Out of class
inline void HelloWorld::SayHello() {
    std::cout << "Hello World!" << std::endl;
}
```

下面程式碼為編譯器的行為：

```cpp
///////// Before Compiled //////////
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

### Constructor and Destructor

**建構子**，在類別中是一個重要的函數，當這個類別的物件（Object）被創建時（也就是在程式碼中要呼叫並創建該類別），編譯器總是會尋找跟這個類別匹配的建構子，自動執行建構子中的函數內容。

這邊簡單講解幾點關於建構子的重點：

- 建構子函數中，通常會執行：初始化類別中的參數，以及執行其他函數（檢查數值啊，讀取資料等等）
- 建構子名稱必須跟類別名稱一樣；且建構子沒有回傳值（沒有return）
- 相反的函數為**解構子**，意思是當這個類別的物件要被刪除（解構）時，會自動呼叫解構子
- 如果一個類別沒有定義建構子或解構子時，會使用**編譯器預設**的建構子或解構子

下面程式碼為String的建構子與解構子範例：

```cpp
String::String(const char* cstr = 0) // 建構子
{
    if (cstr) {    // 判別是否有值
        m_data = new char[strlen(cstr)+1];    // 最後在+1 為結束符號'\0'
        strcpy(m_data, cstr);
    }
    else {    // 未指定初始值
        m_data = new char[1];
        *m_data = '\0';
    }
}

String::~String() // 解構子
{
    delete[] m_data;    // 之前是new一個array，因此這邊要刪除array []
}
```

另，關於建構子，也會提到一個深入的觀念，叫做**Big of Three**三大函數：

- **Constructor** (建構子):`String(const char* cstr=0)`
1. **Destructor** (解構子): `~String()`
2. **Copy constructor** (複製建構子): `String(const String& str)`
3. **Copy assignment operator** (設定運算子): `String& operator=(const String& str)`

在這邊不在做細述，有機會可以在做介紹～

### Initialization List

建構子中，初始化參數時，我們會較常使用Initialization List來對參數初始化，會有比較好的執行效能

```cpp
Class Point {
private:
    int x;
    int y;
public:
    Point(int i = 0, int j = 0)
        :x(i), y(j)  // <== This is Initialization List
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

## Passing and Returning Values

撰寫函數時，總會遇到**傳遞/回傳**數值的相關問題，以下列出主要的兩種傳遞方式：

- **Pass/Return by value**: 被傳遞或回傳的參數值，是不會變動的，會維持原先的初始值。因為只是**複製並傳遞**當前的參數值進行操作
   - 好處：原先的數值不會被變動，只是拷貝一份物件進到此函數中操作
   - 可能的壞處：如果該物件過大時，如一個非常大的字串或struct，可能會影響執行效能，因為每次使用到都要拷貝一份過去
- **Pass/Return by reference**: 傳遞或回傳的對象為這個物件（變數）的地址（Reference），因此當函數對此變數進行數值改動的話，原先的初始值會遭到變動，因為是直接到這個物件的地址去讀取或修改數值
   - 好處：傳遞快速，不需要拷貝就可以參考到該變數
   - 可能的壞處：一不小心會將數值修改到，可能會造成隱性的bug; 但可以依照使用情境，實作**Pass by Reference to Constant**來迴避這個壞處

以下就來看看，他們怎麼實作吧！

### Pass by Value

```cpp
//建構子中，傳遞的double值 r 以及 i
complex (double r = 0, double i = 0)    // Here
    : re (r), im (i)
    { }
```

### Pass by Reference

```cpp
// ostream傳遞位址，os這個變數將會被改變
ostream&
operator << (ostream& os, const Complex& x){    
    return os << '(' << real (x) << ','
              << imag (x) << ')';
}
```

### Pass by Reference to Constant

```cpp
Complex& operator += (const Complex&);
// += 右邊的變數，是被加的，不會改變其量值，因此加上const
```

### Return by Value

```cpp
double real () const { return re; }    // 回傳值，因其沒有位址
```

### Return by Reference

```cpp
Complex& operator += ( const Complex& );    // 回傳的值需要"賦值" 給變數
```

## Appendix - Smart Pointer

可以用來取代傳統的pointer方法，更具有強健性、更加可靠，也可以自動做好內存管理，以避免memory leaking的問題

```cpp
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

## Appendix - Enum Class

用來取代傳統的enum方法

![WPBFLSn.png](https://i.imgur.com/WPBFLSn.png)

## Appendix - Coding Style

**Reference：**

[https://tw-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents.html](https://tw-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents.html)

## Reference

[C++ 物件導向高級程式學習筆記](https://hackmd.io/@1_KXOGKTSJ-h5DpyHTXySg/rk5ki3auK)

[科技讀蟲 C++ 建構子](https://yhtechnote.com/constructor/)

[When do we use Initializer List in C++?](https://www.geeksforgeeks.org/when-do-we-use-initializer-list-in-c/)

[[C++]內嵌函數（inline　function）筆記](https://dotblogs.com.tw/v6610688/2013/11/27/introduction_inline_function)

[C++ Constructor後面的":"是什麼鬼意思？ (Initialization List 教學)](https://vinesmsuic.github.io/2020/01/09/c++-initializationlists/#initializer-list)

[使用 enum class 取代傳統的 enum](https://kheresy.wordpress.com/2019/03/27/using-enum-class/)

## 小結

這篇文章的初稿，其實是我在公司與同事分享的ＰＰＴ；自己希望將這些重要的概念，寫成文章，將使用方法記錄下來，以便未來自己可以去重複複習，在創建一個類別時，可以有所依據。也希望可以幫助到有需要的朋友們！

如果文章內容有錯誤的話，歡迎留言與我指教指教！

