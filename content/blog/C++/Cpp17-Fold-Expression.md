---
title: 'C++ 17: Fold Expression'
date: 2021-04-15 01:21:00
category: 'C++'
draft: false
---

# π· <b>μμ½</b>
- binary μ€νΌλ μ΄ν°μ ν΅ν΄ νλ¦¬λ―Έν° ν©(Parameter Pack) μ€μ¬μ νννλ ννμ
- binary νΉμ unary fold λ μ’λ₯κ° μ‘΄μ¬.
<br>

# π· <b>νμ λ°°κ²½</b>
 C++ 11μ λμλ κ°λ³ κΈΈμ΄ ννλ¦Ώ(variadic template)μ μ¬κ· ν¨μ ννλ‘ κ΅¬μ±μ, λ°λμ μ¬κ· νΈμΆ μ’λ£λ₯Ό μν ν¨μ(terminator)λ₯Ό λ§λ€μ΄μΌνλ λΆνΈν¨μ΄ μμλ€.
 
 <br>

```cpp
template<typename Type1, typename... Type>
int SumAll(Type1 s, Type... ts>
{
    return s + SumAll(ts...);
}

// νλΌλ―Έν° ν©μ΄ μμ κ²½μ°μ λ°μ΄λλ¦¬ μ»¨λμ
int SumAll() { return 0; }
```
<br>
κΈ°μ‘΄ λ°©μμΌλ‘λ μ΄ λ ν¨μμ μ°κ΄μ±μ μμλ³΄κΈ° μ΄λ ΅λ€λ λ¬Έμ κ° μμλ€.

μ΄λ₯Ό ν΄κ²°νκΈ° μν΄, C++ 17μ Fold Expressionμ΄ λμ λμκ³ , μμ μμ λ₯Ό μλμ κ°μ΄ μ½κ² ννν  μ μκ² λμλ€.
<br>
```cpp
template <typename... Ints>
int SumAll(Ints... nums)
{
    return (... + nums);
}
```
<br>

# π· <b>Fold</b>
Foldλ ν¨μν νλ‘κ·Έλλ°μ μ‘΄μ¬νλ κ°λμΌλ‘, κ³ μ°¨ ν¨μ κ³μ΄ μ€ νλμ΄λ€.<br>
ν¨μμ μΈμμ λνμ¬ νΉμ  μ°μ°μμ λν΄ μ¬κ·μ μΌλ‘ μ²λ¦¬ν κ²°κ³Ό κ°μ λ°ννλ€.

![](./images/Right-fold-transformation.png)

Foldμ μ’λ₯λ Unary, Binary λ μ’λ₯λ‘ λλλ©°, μ΄ λν right/leftλ‘ λλμ΄μ§λλ°,<br>
Right/Left κΈ°μ€μ pack(...)μ μμΉμ΄λ©°, packμ μμΉμ λ°λΌ μ€νΌλ μ΄ν°μ μμκ° κ²°μ λ¨.
<br>

## <b>Unary Fold</b>
1. Unary Right Fold (pack op ...)
    ```cpp
    template<typename... Args>
    auto Sum(Args... args)
    {
        return (args + ...);
    }

    auto value = Sum(3, 5, 7, 9); // value == 24
    ```

    unpackμ operatorκ° μ°μΈ‘λΆν° μ μ© μν¨λ€.<br><br> 
    e.g. (3 + (5 + (7 + 9)))
<br>

2. Unary Left Fold (... op pack)
   ```cpp
    template<typename... Args>
    auto Sum(Args... args)
    {
	    return (... + args);
    }

    auto val = Sum(3, 5, 7, 9); // value == 24
   ```
   unpackμ operatorλ₯Ό μ’μΈ‘λΆν° μ μ© μν΄. <br><br>
   e.g. (((3 + 5) + 7) + 9)
<br>

## <b>Binary Folds</b>
- binary fold expressionμ init κ°μ κ°μ§λ€.
- μ΄ κ°μ parameter packμλ ν¬ν¨λμ§ μλ κ°μ΄λ©°, Left/Right μ¬λΆ κ΄κ³μμ΄ κ°μ₯ λ¨Όμ  operatorμ λμμ΄ λλ€.
- Binary Fold Expressionμ μ¬μ©μ opκ° νμ€νκ² λ€λ₯Έ opμ μ°μ μμμ κ΅¬λΆ λκ²λ ν΄μΌνλ€.

<br>

1. <b>Binary Right Fold `(pack op .. op init)` </b>

    ```cpp
    template<typename... Args>
    auto Sum(Args... args)
    {
    	// 1 -> init
    	return (args + ... + 1);
    }
    ```

    μμ: (3 + (5 + (7 + (9 + 1))))

2. <b>Binary Left Fold `(init op ... op pack)`</b>

    ```cpp
    template<typename... Args>
    auto Sum(Args... args)
    {
    	// 1 -> init
    	return (1 + ... args);;
    }
    ```
<br>

- Fold Expressionμ μ¬μ© κ°λ₯ν Binary Operatorλ‘λ 32κ°μ§κ° μλ€.

- μ μ²΄ λͺ©λ‘μ [**μ¬κΈ°**](https://en.cppreference.com/w/cpp/language/fold)λ₯Ό μ°Έμ‘°

<br>

# π· <b>Fold Expressionλ₯Ό νμ©ν΄ ν¨μ μ¬λ¬λ² νΈμΆνκΈ°</b>
```cpp
#include <iostream>

class A 
{
public:
    void DoSomething(int x) const 
    {
        std::cout << "Do something with " << x << std::endl;
    }
};

template <typename T, typename... Ints>
void DoManyThings(const T& kT, Ints... nums) 
{
  // κ°κ°μ μΈμλ€μ λν΄ do_something ν¨μλ€μ νΈμΆνλ€.
    (t.DoSomething(nums), ...);
}

int main() 
{
    A a;
    DoManyThings(a, 1, 3, 2, 4);
}
```
μ μ½λλ₯Ό μ»΄νμΌνλ©΄... 
```cpp
Do something with 1
Do something with 3
Do something with 2
Do something with 4
```
μ΄μ κ°μ κ²°κ³Όκ° λμ¨λ€.

```cpp
(t.DoSomething(nums), ...);
```
μλ μ¬μ€μ λͺ¨λ  μΈμλ€μ λν΄μ <b>```t.DoSomething(arg)```</b>λ₯Ό μ€ννλ κ²κ³Ό κ°λ€.

<br>

# π λ νΌλ°μ€
[https://www.modernescpp.com/index.php/fold-expressions](https://www.modernescpp.com/index.php/fold-expressions)

[https://baptiste-wicht.com/posts/2015/05/cpp17-fold-expressions.html](https://baptiste-wicht.com/posts/2015/05/cpp17-fold-expressions.html)

[http://filipjaniszewski.com/2016/11/24/c17-folding-expression/](http://filipjaniszewski.com/2016/11/24/c17-folding-expression/)

[https://blog.tartanllama.xyz/exploding-tuples-fold-expressions/](https://blog.tartanllama.xyz/exploding-tuples-fold-expressions/)

[https://modoocode.com/290](https://modoocode.com/290)