---
title: ADT
date: 2018-05-10
---

# 地址
http://home.ustc.edu.cn/~huang83/ds/Data%20Structures%20and%20Algorithms%20Using%20Python.pdf

# ADT == Abstract Data Types

1. Computer programs are constructed using a programming language appropriate to the problem. 计算机程序使用适合于该问题的编程语言来构建。
2. While programming is an important part of computer science, computer science is not the study of programming. Nor is it about learning a particular programming language. 虽然编程是计算机科学的重要组成部分，但计算机科学并不是编程的研究。 也不是学习特定的编程语言。
3. Instead, programming and programming languages are tools used by computer scientists to solve problems. 相反，编程和编程语言是计算机科学家用来解决问题的工具。
4. To distinguish between the different types of data, the term type is often used to refer to a collection of values and the term data type to refer to a given type along with a collection of operations for manipulating values of the given type 为了区分不同类型的数据，术语类型通常用于指代值的集合，术语数据类型用于指代给定类型以及用于操作给定类型的值的操作的集合
5. Two common types of abstractions encountered in computer science are procedural, or functional, abstraction and data abstraction. 计算机科学中遇到的两种常见类型的抽象是程序性的或功能性的抽象和数据抽象。
6. Procedural abstraction is the use of a function or method knowing what it does but ignoring how it’s accomplished 程序抽象是使用知道它做什么但忽略它如何完成的函数或方法 。书中拿数学中开平方根的函数来举例。
7. Data abstraction is the separation of the properties of a data type (its values and operations) from the implementation of that data type. 数据抽象是将数据类型的属性（其值和操作）与该数据类型的实现分开。书中使用python中的strings数据类型举例
8. One problem with the integer arithmetic provided by most high-level languages and in computer hardware is that it works with values of a limited size. On 32-bit architecture computers, for example, signed integer values are limited to the range −231. . .(231 − 1). What if we need larger values? In this case, we can provide long or “big integers” implemented in software to allow values of unlimited size. 大多数高级语言和计算机硬件提供的整数算术的一个问题是，它与有限大小的值一起工作。 例如，在32位体系结构的计算机上，有符号整数值被限制在-231范围内。。 （231-1） 如果我们需要更大的值呢？ 在这种情况下，我们可以在软件中提供长整形来实现无限大小的值。
9. All data structures store a collection of values, but differ in how they organize the individual data items and by what operations can be applied to manage the collection. 所有数据结构都存储一组值，但是它们组织各个数据项的方式不同，以及可以应用哪些操作来管理集合。
10. Some data structures are better suited to particular problems. For example, the queue structure is perfect for implementing a printer queue, while the B-Tree is the better choice for a database index.  一些数据结构更适合特定的问题。 例如，队列结构非常适合实现打印机队列，而B-Tree是数据库索引的更好选择。
11. No matter which data structure we use to implement an ADT, by keeping the implementation separate from the definition, we can use an abstract data type within our program and later change to a different implementation, as needed, without having to modify our existing code. 无论我们使用哪种数据结构来实现ADT，通过将实现与定义分开，我们可以在我们的程序中使用抽象数据类型，然后根据需要更改为不同的实现，而无需修改我们现有的代码。
12. 看到1.1.4