---
title: 生成 200 个激活码（或者优惠券）
date: 2018-10-21
tag: [python, 编程]
category: Python练习册
---

第 0001 题： 做为 Apple Store App 独立开发者，你要搞限时促销，为你的应用生成激活码（或者优惠券），使用 Python 如何生成 200 个激活码（或者优惠券）？

```Python
#! /usr/bin/env python
# -*- coding: utf8 -*-

# author: ywx556386
# date: 2018-10-21
# description： 生成200个促销码
import string
import random

#产生促销码数据源
source_list = []
# 促销码位数
number = 10
# 存储200个促销码的list
promo_code_list = []

def gen_promo_code():
    # 包含大小写的26个字母、0-9的列表
    source_list = list(string.ascii_letters+string.digits)
    # 随机返回number位的字符串
    return ''.join(random.sample(source_list,number))

# 生成200个促销码
for i in range(200):
    promo_code_list.append(gen_promo_code())

print(promo_code_list)
```