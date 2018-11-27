---
title: 统计纯文本文件每个英文单词出现的频率
date: 2018-10-21
tag: [python, 编程]
category: Python练习册
---

第 0004 题： 任一个英文的纯文本文件，统计其中的单词出现的个数。

```Python
#/usr/bin/env python
# -*- coding: utf8 -*-

# author: ywx556386
# date: 2018-10-21
# description： 统计文章中每个英文单词出现的频率
import sys
import os

# 文章原始单词列表
source_words = []
# 文章每个单词出现频率统计字典
count_of_words = {}

#读取文件并以空格符作为分隔符存储在列表中
def read_file(artical):
    with open(artical) as file:
        content = file.read()
    return content.split()


if __name__ == "__main__":
    if len(sys.argv) > 1:
        if sys.argv[1] and os.path.exists(sys.argv[1]):
             # 待处理文件
            #artical = './DevOps_RoadMap.txt'
            artical = sys.argv[1]
            source_words = read_file(artical)
            for word in source_words:
                if count_of_words.get(word):
                    count_of_words[word] += 1
                else:
                    count_of_words[word] = 1
            #无排序输出
            #print(count_of_words)
            #按照word出现的频率排序
            print(sorted(count_of_words.items(), key=lambda d: d[1], reverse=True))
        else:
            print('请检查输入的文件名及路径是否正确')
    else:
        print('请输入要处理的文件名，E.g: python count_number_of_english_words.py ./Devops_RoadMap.txt')
```