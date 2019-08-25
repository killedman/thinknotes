---
title: grep的高级用法 
date: 2019-08-25
tag: [2019年, grep]
category: 技术笔记
---

grep -n pattern files  显示行号
grep -i pattern files  不区分大小写地搜索，默认情况下区分大小写
grep -l pattern files  只列出匹配的文件名
grep -L pattern files  列出不匹配的文件名
grep -w pattern files  只匹配整个单词而不是字符串的一部分
grep -C number pattern files 匹配的上下文分别显示number行
grep -r pattern files ./  遍历子目录，./表示当前目录，可与-l等一起使用搜索含有某个关键词的文件

grep或操作：
grep -E 'game|root' /etc/passwd
egrep 'game|root' /etc/passwd
awk '/game|root/' /etc/passwd

ls /usr/share/doc |grep '4$' 列出/usr/share/doc中以数字4结尾的所有文件
grep '[[:digit:]]' /etc/hosts 打印/etc/hosts中包含数字的所有行
grep '[0-9]' /etc/hosts 打印/etc/hosts中包含数字的所有行
grep '127.0.0.1' /etc/hosts 打印/etc/hosts中包含127.0.0.1的行

grep -P ':\d{3}:' /etc/passwd 打印/etc/passwd中冒号（：）之间是三位数的行
grep -V '^#' /etc/hosts > /tmp/hosts 打印/etc/hosts中不是以#开头的所有行
