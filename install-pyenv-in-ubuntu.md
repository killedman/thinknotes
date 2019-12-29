---
title: 使用pyenv配合virutalenv安装和管理多个python版本 
date: 2019-12-29
tag: [2019年, pyenv, virutalenv, python]
category: 技术笔记
---

# 使用 pyenv 配合 virtualenv 安装和管理多个 Python 版本

## Best way to run python 3.7 on Ubuntu 16.04 which comes with python 3.5

> I would not recommend manually fiddling around with source code installations and paths. Use pyenv and save yourself the trouble.
>
> All you have to do is:
>
>     Run the pyenv installer
>     Follow the instructions
>     Install the Python versions you need
>     Choose which Python version you want to use for a given directory, or globally
>
> For example, to install 3.7, check which versions are available:
>
> pyenv install -l | grep 3.7
>
> Then run:
>
> pyenv install 3.7.1
>
> Now, you can choose your Python version:
>
> pyenv global 3.7.1
>
> This switches your python to point to 3.7.1. If you want the system python, run:
>
> pyenv global system
>
> To check which Python versions are available, run pyenv versions.
>

## ubuntu 16.04上安装配置pyenv


### 准备工作： 

1. 安装curl： sudo apt -y install curl 
2. 安装pip，这里安装sudo apt -y install python3-pip，安装后需要执行pip3，使用别名实行输入pip执行pip3，添加alias pip='pip3'到~/.bashrc，为了在sudo到root时也生效，需要添加到/root/.bashrc中；然后执行source ~/.bashrc就会生效，Or you can make a link ln -s /usr/local/bin/pip3 /usr/local/bin/pip

3. ubuntu pyenv install python 3 缺很多包？ 需要先执行以下命令安装必要的包，否则会出现很多报错，我遇到的可以看后面备忘中的内容

   ```shell
   sudo apt-get install make build-essential libssl-dev zlib1g-dev
   sudo apt-get install libbz2-dev libreadline-dev libsqlite3-dev wget curl
   sudo apt-get install llvm libncurses5-dev libncursesw5-dev
   sudo apt-get update
   ```

4. pyenv太慢？手动下载pyenv install -v 3.7.3提示的安装包，放到~/.pyenv/cache这个目录下，再次执行pyenv install -v 3.7.3 ；网上查到的使用国内镜像解决pyenv安装太慢的方法，实际操作时，发现提供的国内镜像源访问不了

### 开始安装配置：
1. 执行curl -L https://raw.github.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash自动安装配置pyenv,

2. 安装结束后会提示将以下内容添加到~/.bashrc

  ```shell
  export PATH="/home/dreamsn/.pyenv/bin:$PATH"
  eval "$(pyenv init -)"
  eval "$(pyenv virtualenv-init -)"
  ```

3. 执行source ~/.bashrc 或者  exec $SHELL 使配置生效

## pyenv 结合 virtualenv 创建虚拟环境

> pyenv virtualenv 3.4.3 TEST
>
> pyenv activate TEST
>
> pyenv deactivate
>
> pyenv uninstall TEST
>
> For example, to install 3.7, check which versions are available:
>
> pyenv install -l | grep 3.7
>
> Then run:
>
> pyenv install 3.7.1
>
> Now, you can choose your Python version:
>
> pyenv global 3.7.1
>
> This switches your python to point to 3.7.1. If you want the system python, run:
>
> pyenv global system
>
> To check which Python versions are available, run pyenv versions.




以上是网上的指导，以下是具体操作：

```shell
pyenv install -l | grep 3.7
pyenv install 3.7.3
pyenv global 3.7.3
pyenv versions
pyenv virtualenv 3.7.3 my-virtual-env-3.7.3
pyenv virtualenvs
pyenv activate  my-virtual-env-3.7.3
```

命令解释：

1. 创建虚拟环境 $ pyenv virtualenv 2.7.10 my-virtual-env-2.7.10
2. 若不指定python 版本，会汇报认使用当前环境python版本。
3. 列出当前虚拟环境 pyenv virtualenvs
4. 激活虚拟环境 pyenv activate
5. 退出虚拟环境 pyenv deactivate
6. 删除虚拟环境 pyenv uninstall my-virtual-env


##  pyenv 常用的命令说明

> ```
> > 使用方式: pyenv <命令> [<参数>]
> >
> > 命令:
> > commands    查看所有命令
> > local       设置或显示本地的Python版本
> > global      设置或显示全局Python版本
> > shell       设置或显示shell指定的Python版本
> > install     安装指定Python版本
> > uninstall   卸载指定Python版本)
> > version     显示当前的Python版本及其本地路径
> > versions    查看所有已经安装的版本
> > which       显示安装路径
> 
> > pyenv
> > pyenv 1.2.8
> > Usage: pyenv <command> [<args>]
> >
> > Some useful pyenv commands are:
> > commands    List all available pyenv commands
> > local       Set or show the local application-specific Python version
> > global      Set or show the global Python version
> > shell       Set or show the shell-specific Python version
> > install     Install a Python version using python-build
> > uninstall   Uninstall a specific Python version
> > rehash      Rehash pyenv shims (run this after installing executables)
> > version     Show the current Python version and its origin
> > versions    List all Python versions available to pyenv
> > which       Display the full path to an executable
> > whence      List all Python versions that contain the given executable
> >
> > See `pyenv help <command>' for information on a specific command.
> > For full documentation, see: https://github.com/pyenv/pyenv#readme
> ```
>
> 
>



### python 配置

> pyenv versions -- 查看系统当前安装的python列表
>
> pyenv install --list -- 查看可安装的版本
>
> pyenv install -v 3.5.1 -- 安装python
>
> pyenv uninstall 2.7.3 -- 卸载python
>
> pyenv rehash -- 创建垫片路径（为所有已安装的可执行文件创建 shims，如：~/.pyenv/versions/*/bin/*，因此，每当你增删了 Python 版本或带有可执行文件的包（如 pip）以后，都应该执行一次本命令）

### python 切换

> pyenv global 3.4.0 -- 设置全局的 Python 版本，通过将版本号写入 ~/.pyenv/version 文件的方式。
>
> pyenv local 2.7.3 -- 设置面向程序的本地版本，通过将版本号写入当前目录下的 .python-version 文件的方式。通过这种方式设置的 Python 版本优先级较 global 高。
>
> pyenv 会从当前目录开始向上逐级查找 .python-version 文件，直到根目录为止。若找不到，就用 global 版本。
>
> pyenv shell pypy-2.2.1 -- 设置面向 shell 的 Python 版本，通过设置当前 shell 的 PYENV_VERSION 环境变量的方式。这个版本的优先级比 local 和 global 都要高。--unset 参数可以用于取消当前 shell 设定的版本。
> pyenv shell --unset

### python 优先级

shell > local > global

### pyenv 插件: pyenv-virtualenv

使用自动安装 pyenv 后，它会自动安装部分插件，通过 pyenv-virtualenv 插件可以很好的和 virtualenv 结合：

```shell
[root@linux3311 ~]# cd .pyenv/plugins/
[root@linux3311 plugins]# ll
insgesamt 24
drwxr-xr-x. 4 root root 4096 19. Jun 05:17 pyenv-doctor
drwxr-xr-x. 5 root root 4096 19. Jun 05:18 pyenv-installer
drwxr-xr-x. 4 root root 4096 19. Jun 05:18 pyenv-update
drwxr-xr-x. 7 root root 4096 19. Jun 05:18 pyenv-virtualenv
drwxr-xr-x. 4 root root 4096 19. Jun 05:18 pyenv-which-ext
drwxr-xr-x. 5 root root 4096 19. Jun 05:17 python-build
```

使用 pyenv 来管理 python，使用 pyenv-virtualenv 插件来管理多版本 python 包。

此时，还需注意，当我们将项目运行的 env 环境部署到生产环境时，由于我们的python 包是依赖python 的，需要注意生产环境的python版本问题

## 参考：

- https://www.daimafans.com/article/d15590909394550784-p1-o1.html
- https://yq.aliyun.com/articles/59243
- https://www.uedbox.com/post/9306/
- https://www.v2ex.com/t/420216
- https://blog.csdn.net/github_35817521/article/details/53467610


## pyenv安装太慢？使用国内镜像解决

参考链接： https://www.uedbox.com/post/9306/

```
pyenv 搜狐镜像源加速：http://mirrors.sohu.com/python/ 
下载需要的版本放到 ~/.pyenv/cache 文件夹下面
然后执行 pyenv install 版本号 安装对应的 python 版本
傻瓜式脚本如下，其中 v 表示要下载的版本号

v=3.6.5;wget http://mirrors.sohu.com/python/$v/Python-$v.tar.xz -P ~/.pyenv/cache/;pyenv install $v  
```

### 备忘: 因为没有提前安装必要的依赖包出现的报错

出现的报错（如果提前执行上面安装包的操作则不会出现以下问题）：
使用pyenv install -v 3.7.3报错，如下：
zipimport.ZipImportError: can't decompress data; zlib not available

需要执行 sudo apt install zlib1g-dev 安装缺失的包，注意：使用 sudo apt install zlib或者zlib-devel提示找不到包。 参考： https://github.com/jiansoung/issues-list/issues/13

再次报错： ModuleNotFoundError: No module named '_ctypes'，执行sudo apt-get install libffi-dev尝试 参考： https://stackoverflow.com/questions/27022373/python3-importerror-no-module-named-ctypes-when-using-value-from-module-mul

再次出错：

WARNING: The Python bz2 extension was not compiled. Missing the bzip2 lib?
WARNING: The Python readline extension was not compiled. Missing the GNU readline lib?
ERROR: The Python ssl extension was not compiled. Missing the OpenSSL lib?
