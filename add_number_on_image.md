---
title: 给图片右上角添加红色的数字
date: 2018-10-17
tag: [python, 编程]
category: Python练习册
---

第 0000 题： 将你的 QQ 头像（或者微博头像）右上角加上红色的数字，类似于微信未读信息数量那种提示效果。 

```Python
#!  /usr/bin/env python
# -*- coding: utf8 -*-

# author: ywx556386
# date: 2018-10-17
# 在图片上添加数字

from PIL import Image, ImageDraw, ImageFont
import os,sys

#要在图片上添加的数字
added_number = 4

if len(sys.argv) > 1:
    if sys.argv[1] and os.path.exists(sys.argv[1]):
        sourceFileName = sys.argv[1]
        rawimage = Image.open(sourceFileName)
        drawimage = ImageDraw.Draw(rawimage)

        xSize, ySize = rawimage.size
        fontSize = min(xSize,ySize) // 11

        myFont = ImageFont.truetype("./arial.ttf",fontSize)
        drawimage.text([0.9 * xSize, 0.1 * ySize -fontSize], str(added_number), fill = (255, 0, 0),font=myFont)
        del drawimage
        #rawimage.show()
        outfile = os.path.splitext(sourceFileName)[0] + '_' + str(added_number) + '.jpeg'
        rawimage.save(outfile, 'jpeg')
    else:
        print('请确认输入域的图片名称及路径是否正确')
else:
    print('请输入要添加数字的图片名称,E.g: python add_number_on_image.py ./xxx.jpg')
```