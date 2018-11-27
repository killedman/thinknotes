---
title: python脚本：删除行内所有空格
date: 2018-10-03
tag: [python, 编程]
category: 我爱编程
---


```Python
#/usr/bin/env python

'''
该脚本用于删除文件每行中存在的空格;尤其是从kindle中复制出来的的笔记带有很多空格符，手动去除很麻烦，所以写了这个脚本
auther: dreampython
date: 2018-10-3
'''
import re
def replace():
    pattern = r'\s+'
    with open('notes_read_Country_Driving.md',encoding='utf8') as f:
        lines = f.readlines()
        for line in lines:
            print(re.sub(pattern,'',line))

def main():
    replace()

if __name__ == '__main__':
    main()
```