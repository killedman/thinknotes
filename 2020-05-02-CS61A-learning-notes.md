---
title: CS61A learning notes
date: 2020-05-02
tag: [2020年, Python]
category: 我爱编程
---

> 1.3.5   Choosing Names
>
> The interchangeability of names does not imply that formal parameter names do not matter at all. On the contrary, well-chosen function and parameter names are essential for the human interpretability of function definitions!
>
> The following guidelines are adapted from the style guide for Python code, which serves as a guide for all (non-rebellious) Python programmers. A shared set of conventions smooths communication among members of a developer community. As a side effect of following these conventions, you will find that your code becomes more internally consistent.
>
>     Function names are lowercase, with words separated by underscores. Descriptive names are encouraged.
>     Function names typically evoke operations applied to arguments by the interpreter (e.g., print, add, square) or the name of the quantity that results (e.g., max, abs, sum).
>     Parameter names are lowercase, with words separated by underscores. Single-word names are preferred.
>     Parameter names should evoke the role of the parameter in the function, not just the kind of argument that is allowed.
>     Single letter parameter names are acceptable when their role is obvious, but avoid "l" (lowercase ell), "O" (capital oh), or "I" (capital i) to avoid confusion with numerals.
>
> There are many exceptions to these guidelines, even in the Python standard library. Like the vocabulary of the English language, Python has inherited words from a variety of contributors, and the result is not always consistent.
>

翻译： 函数名字要全部小写，使用下划线分隔单词，鼓励使用描述型的名字; 通过好的函数名字可以看出函数的作用或者返回的结果是什么，比如print、add、square、max、abs、sum；函数参数也要全部小写，使用下划线分隔单词，单个单词的名字作为参数更好；好的参数名字可以看出参数在函数的作用；单个字母作为参数名称是被接受的，但要避免容易引起歧义的 "l" (lowercase ell), "O" (capital oh), or "I" (capital i) 

> When it comes to division, Python provides two infix operators: / and //. The former is normal division, so that it results in a floating point, or decimal value, even if the divisor evenly divides the dividend:
>
> >>> 5 / 4
> >>> 1.25
> >>> 8 / 4
> >>> 2.0
>
> The // operator, on the other hand, rounds the result down to an integer:
>
> >>> 5 // 4
> >>> 1
> >>> -5 // 4
> >>> -2
>
> These two operators are shorthand for the truediv and floordiv functions.
>
> >>> from operator import truediv, floordiv
> >>> truediv(5, 4)
> >>> 1.25
> >>> floordiv(5, 4)
> >>> 1
>
> You should feel free to use infix operators and parentheses in your programs. Idiomatic Python prefers operators over call expressions for simple mathematical operations.
>

翻译: 说到除法，python提供了两个中缀符，/ 和 // ，前面的是普通的除法，结果是浮点数或小数值，即使除数除尽了被除数；// 操作符，将结果处理为一个整数返回，这个除法经常叫做地板除；
这两个操作符，是truediv和floordiv两个函数的快捷方式。
python更喜欢使用操作符的快捷方式简化数学运算。

> We now turn to the topic of what makes a good function. Fundamentally, the qualities of good functions all reinforce the idea that functions are abstractions.
>
>     Each function should have exactly one job. That job should be identifiable with a short name and characterizable in a single line of text. Functions that perform multiple jobs in sequence should be divided into multiple functions.
>     Don't repeat yourself is a central tenet of software engineering. The so-called DRY principle states that multiple fragments of code should not describe redundant logic. Instead, that logic should be implemented once, given a name, and applied multiple times. If you find yourself copying and pasting a block of code, you have probably found an opportunity for functional abstraction.
>     Functions should be defined generally. Squaring is not in the Python Library precisely because it is a special case of the pow function, which raises numbers to arbitrary powers.
>
> These guidelines improve the readability of code, reduce the number of errors, and often minimize the total amount of code written. Decomposing a complex task into concise functions is a skill that takes experience to master. Fortunately, Python provides several features to support your efforts
>

翻译： 这是关于写一个好的函数的主题。
每个函数只做一件事；一个执行多个工作的函数应该分成几个函数；
不要重复你自己 是软件工程师的一个格言。
应该定义通用的函数，Squaring 没有在python函数库中定义是因为它不是通用的，python函数库中有pow函数，它提供了数字的各种幂；
以上的指南提高了代码的可读性、减少了错误的数量，可以大大减少要写的代码量。将一个复杂问题分解为简单的问题是很好的技能；幸好，Python提供了很多好的特性。



表达式赋值语句中的变量，如果稍后有变化，不会在当前的这个表达式中同步，也就是不会影响当前的表达式的值；

```python
from math import pi
radius = 10
area, circ = pi * radius * radius, 2 * pi * radius
# area的值是314.15926...
radius = 20
# 这个是的area依然是314.15926,而不会变成628.31852...

# 如果计算area的不是expression，而是一个fuction
def area():
    return pi * radius * radius
# 以上计算aera的函数，结果会随着radius值的变化而变化。
```

how assignment works we are evaluating this expression to get a single value and  that's what get bound to area , it doesn't remember that area was once defined in terms of a radius assignment 

以上英文的表达更加的精确，解释了在表达式中，赋值语句的工作方式，评估赋值符号右边的表达式获取到一个单独的值，然后将赋值语句左边的name绑定到这个value，赋值语句不会记得是radius这个变量。



Assignment is a simple means of abstraction: binds names to values

Function definition is a more powerful means of abstraction: binds names  to expressions



