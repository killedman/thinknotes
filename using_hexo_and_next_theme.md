---
title: 安装hexo和next主题
date: 2018-10-02
tag: hexo
category: 技术笔记
---

##  一、linux系统下安装hexo

参考[hexo官方文档](https://hexo.io/docs/)安装即可

## 二、更换hexo默认主题

使用了[next主题](https://github.com/iissnan/hexo-theme-next)


###下载安装next主题

进入你的hexo博客所在目录执行以下命令（命令来自next github页面）：


```Python
mkdir themes/next
curl -s https://api.github.com/repos/iissnan/hexo-theme-next/releases/latest | grep tarball_url | cut -d '"' -f 4 | wget -i - -O- | tar -zx -C themes/next --strip-components=1
```

### 修改主题

修改你的hexo博客所在目录下的“_config.yml”配置文件，将theme: 后面的内容修改为next

### 测试

增加内容后可以使用以下命令进行测试修改,浏览器中默认预览地址： http://ip:4000

```Python
hexo server --debug
```

### 部署

测试没有问题后，使用以下命令产生静态文件并部署，一般使用nginx进行代理访问

```Python
hexo generate
hexo deploy
```

如出现缓存引起的异常，可以在生成命令前执行清除缓存命令: hexo clean

参考[hexo主题next的中文readme](https://github.com/iissnan/hexo-theme-next/blob/master/README.cn.md)安装配置了next主题

### nginx静态页面托管配置

```
server {
server_name thinknotes.cn; # managed by Certbot
    root         <blog所在目录绝对路径>/public/;
    index        index.html;

    location / {
        root <blog所在目录绝对路径>/public;
    }
```


## 三、配置相关

### 使用next主题时md文件注意事项

1. 每篇md文件的title、date、tag、category等需要放在两个---之间
2. 多个tag需要使用格式 tag: [tag1, tag2, tag3]
3. 修改hexo博客目录下_config.yml文件中的language选项可修改博客语言，具体值参考使用主题页面的值
4. 通过注释和取消next主题目录下的_config.yml文件中的scheme的注释可以更换next的样式，总共有4种可供更换，修改后需要重启hexo服务
5. 通过注释和取消next主题目录下的_config.yml文件中menu下的内容可以打开或关闭相应的菜单功能，修改后需要重启hexo服务

### 代码显示配置

1. 主题的_config.yml文件
  * 你的blog/themes/next/_config.yml中找到highlight_theme: 将后面的值分别更改为normal | night | night eighties | night blue | night bright 中的任何一个，看自己喜欢那种就选哪一种；
2. 站点的_config.yml文件
  * 修改站点的_config.yml文件中的hightlight，将其中的auto_detect设置为true
3. 代码块的语言标注,注意： 三个点是英文输入法下敲击esc键下面的键三次，不是单引号

```Python
'''Python
code here
'''
```


### 评论区配置

使用来比力，参考[hexo添加评论功能](https://blog.csdn.net/ganzhilin520/article/details/79048010)

使用邮箱注册，选择免费的city版本，将获取的uuid粘贴到：你的blog/themes/next/_config.yml字段livere_uid的值中




