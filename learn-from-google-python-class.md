---
title: learn from google python class
date: 2018-05-27
---

## -tt

<pre>
#!/usr/bin/env python -tt
</pre>

-tt的作用是当你的python文件里存在空格和tab键混用时会报错提示你


## string前两个字符 s[:2],后两个字符s[-2:]


## Provided simple test() function used in main() to print

<pre>
# what each function returns vs. what it's supposed to return.
def test(got, expected):
  if got == expected:
    prefix = ' OK '
  else:
    prefix = '  X '
  print(('%s got: %s expected: %s') %(prefix, repr(got), repr(expected)))
  #print(prefix)
</pre>
  
  
## repr(got) 获得函数got的return值
  
 a = [1,2,3] 
 要把list a复制一份给b，需要执行 b = a[:], 而b=a指向同一份数据

## sorted()
1. sorted(a)
2. sorted(a,reverse=True)
3. sorted(a,key=len)
4. sorte(a,key=Last)
5. sorted(tuples,key=lambda x: x[-1])
6. sorted(new_jpg_list,key=url_sort_key)

<pre>
def Last(s):
     return s[-1]
</pre>

## dictionary 
1. d.get('x') 如果k x不存在，则返回None，不会报错，d['x'] 如果x不存在则会报错
2. d.keys()
3. d.values()
4. d.items()

## 正则表达式 regular expression
1. . (dot) any char
2. \w word character is just A through Z, zero through nine
3. \d digit
4. \s whitespace
5. \S 空白之外的所有符号 match any non-space char
    * pattern = r'"GET (\S+)'
6. []
7. () 分组
8. * 0或多个
9. + 1或多个
10. r python不解释里面的字符
11. re.search()
    * match = re.search(r'-\w+-(\w+)\.\w+',url)
12. re.findall()

## os
1. import os
    * os.listdir()
    * os.path.join(dir,filename)
    * os.path.abspath()
    * os.path.exist()
2. import shutil
    * shutil.copy(f,to_dir+'/'+os.path.basename(f))
3. import subprocess
    * cmd = 'ls ' + dir
	* (status,output) = subprocess.getstatusoutput(cmd)
	* print(output)
4. Deprecated since version 2.6: The commands module has been removed in Python 3. Use the subprocess module instead.

## urllib
1. import urllib.request
    * urllib.request.urlopen('http://www.google.com')
	* urllib.request.urlretrieve(urls,fullfilename)




 
 