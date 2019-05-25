---
title: sed命令
date: 2019-01-20
tag: [2019年, sed]
category: 技术笔记
---

## 替换某个关键词开头的整行内容

```shell
sed -i '/^keyword/cnewkeyword' file.txt
```

-i参数表示直接修改file.txt文件，keyword是要替换的行的关键词，这里使用^keyword的意思是寻找keyword开头的行，然后使用newkeyword替换，①注意newkeyword前面有一个字母c是关键，②newkeyword后面没有左斜线这个也需要特别注意，③这个替换是全局行替换

## 替换某个关键词，仅替换该关键词，而不是整行替换

```shell
sed -i 's/keyword/newkeyword/g' file.txt
```

-i参数表示直接修改file.txt,后面的g表示替换行内所有keyword为newkeyword,如果不加g表示仅替换行内第一个匹配的keyword为newkeyword

