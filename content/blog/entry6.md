---
title: C++函數式🐱紙(C++ functional Programming)
tags: C++
category: Programming Languages
excerpt: C++的函數式介紹
created: 2019-06-28
image: ./images/qingbao-meng-01_igFr7hd4-unsplash.jpg
image_caption: Photo by Qingbao Meng on Unsplash
author: author1
---

# **前言：面向過程編程的定義**

面向過程是一種以步驟為中心的編程設計方法。編程的時候把解決問題的步驟拆分出來，並用函數把這些步驟實現出來，或者說包裝起來，便是面向過程的意思了。那麼，如果我們要用C++進行一個未知數的四則運算，那麼該怎麼拆分步驟呢？

1. 先界定我們要尋找的range
2. 把昨日的Code寫進來
3. 分開 + - x ÷四種情況做運算
4. 把運算結果返回

你會發現都是順序的結果

# **函數需要標明型別的！！**

如果忘記了C++有什麼類型可以會第4天看最底的小表格

# **函數**

我假設大家都聽過數學的函數，數學的函數裡的表示是這樣的： `sin(x)` , `f(y)`,你可以看見函數有一個名字，而在括弧裡的就是你要傳入的數值。如果要定義一個函數做什麼，和返回的數值是什麼。那麼就要用以下的方法。

我們假設要做一個 叫 `add(x,y)` 的函數,當中我們希望他計算出 x + y 的總和，那麼就可以這樣做。

    //return 就是把我們希望要的答案輸出到主函數裡

如此類推，我們可以得出以下的函數

    //return 就是把我們希望要的答案輸出到主函數裡//return 就是把我們希望要的答案輸出到主函數裡//return 就是把我們希望要的答案輸出到主函數裡

那麼我們下一步就是查詢數式究竟是 + - x ÷ 了

    auto calculate(auto x, char operators, auto y){
    	switch(operators){
    		case '+': return sumOf(x,y); break;
    		case '-': return differenceOf(x,y); break;
    		case '*': return productOf(x,y); break;
    		case '/': return quotientOf(x,y); break;
    }

- 我們現在就已經弄了一個最基本的Parser 了

然後再寫一個四則驗算器。

    auto check(int x, char oeprators, int y, int z){
    	return calculate(x,operators,y) == z;
    }

- 這個運算器會在三個數值都正確的時候返回 `true`，反之 `false`

下一步便是寫出計算的For Loops了

    auto evaluateX(char operators, int y, int z){
    	for(auto x = 0; x < 1000 ;x++){
    		if(check(x,operators,y,z)) return static_cast<double>(x);
    		else if(check(-x,operators,y,z)) return static_cast<double>(x);
    		else continue;
    	}

這個程序的範圍從-1000到1000，能應付簡單的計算,static_cast應付小數的

那怎麼使用函數呢？其實上面的函數有不少都有使用其他函數的，而下面就是在Main中使用。

    //輸出了 2//輸出了 1.2//輸出了 180

這樣，我們一個並不是很Flexible的 Function就完成啦。回顧一下函數結構

    -- evaluateX()
    ----check()
    --------calculate()
    ------------sumOf()
    ------------differenceOf()
    ------------quotientOf()
    ----check()
    --------calculate()
    ------------sumOf()
    ------------differenceOf()
    ------------quotientOf()

## Lambda 函數

函數式有三個概念，分別是

1. 一等公民和高階函數
2. 純函數/類似數學函數
3. 宣告式編程

# **特性與語法**

### **1. 第一公民特性**

函數式編程第一個特性就是當函數式是變數一樣傳遞。例如以下的例子

    auto sum = [](auto x, auto y){
    		auto sum = x + y;
    		return sum;
    };
    int main(){
    	auto sums = sumOf(1,2);
    }

### **2. 高階函數特性**

可以當函數式是參數傳入到另一個函數式，甚至是Vector/Map等等。改動Day 6的函數式為例子。

    #include <unordered_map>#include <functional>using namespace std;
    
    auto difference = [](auto x, auto y){ return x-y; };
    auto product = [](auto x, auto y){ return x * y; };
    auto quotient = [](auto x, auto y){ return x/y; };
    auto sum = [](auto x, auto y){ return x + y; };
    
    unordered_map<char, function<double(double,double)>>  calculate = {
        {'+',sum},
        {'-',difference},
        {'*',product},
        {'/',quotient}
    };

### **3. 宣告式編程**

宣告式編程是較為抽象的程式碼，描述該在哪做什麼以及資料流程，而且很少用到If，While，for loop等等。你可以很容易猜到他在做什麼，例如下面的程序。

    int main(){
    	calculate['*'](3.5,2);
    	calculate['/'](9.5,2);
    }

輸出

    7
    4.75

### **4. 高階函數（二）**

Lambda也擁有高階函數的第二個特性，就是能自己返回函數了

    using namespace std;
    
    auto chooseSign = [](char sign){ return calculate[sign]; };
    
    auto equation = [](auto x, auto y,char sign, auto z){
    	auto evaluate = chooseSign(sign);
    	
    	for(auto i = 0;i<2000;i++){
    		if(evaluate(x,y) == z) return x;
    		else if(evaluate(x,y) == z) return -x;
    	}
    	return -1;
    };

### **5. 純函數**

純函數的意思是，將相同的數值放到函數裡會輸出一樣的結果，就像數學函數一樣。在C++裡，大部分基本函數都是純函數，包括Day 6的函數和今日的所有函數。

好了，其實函數式編程已經講完了...