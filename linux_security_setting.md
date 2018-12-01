---
title: linux服务器安全设置
date: 2018-05-12
---

## 给普通用户添加sudo权限

### 添加普通用户

    useradd commonuser
    passwd commonuser

### 使用root账号执行visudo

    [root@www ~]# visudo
    ....(前面省略)....
    root    ALL=(ALL)       ALL  <==找到这一行，大约在 76 行左右
    commonuser  ALL=(ALL)       ALL  <==这一行是你要新增的！
    ....(前面省略)....
    
### SUDO 与 su的区别
sudo命令不需要输入root的密码，只需要输入自己的密码就可以拥有root的权限，su命令需要输入root账号密码；普通用户执行sudo su -可以只输入普通用户密码切到root账号；
    
    
### sudo su -

很多时候我们需要大量运行很多 root 的工作，所以一直使用 sudo 觉得很烦人！那有没有办法使用 sudo 搭配 su ， 一口气将身份转为 root ，而且还用用户自己的口令来变成 root 呢？是有的！而且方法简单的会让你想笑！ 我们创建一个 ADMINS 帐户别名，然后这样做：
        [root@www ~]# visudo
        User_Alias  ADMINS = pro1, pro2, pro3, myuser1
        ADMINS ALL=(root)  /bin/su -
    	
接下来，上述的 pro1, pro2, pro3, myuser1 这四个人，只要输入『 sudo su - 』并且输入『自己的口令』后， 立刻变成 root 的身份！不但 root 口令不会外流，用户的管理也变的非常方便！ 这也是实务上面多人共管一部主机时常常使用的技巧呢！这样管理确实方便，不过还是要强调一下大前提， 那就是『这些你加入的使用者，全部都是你能够信任的用户』！

From [Linux 账号管理与 ACL 权限配置](http://cn.linux.vbird.org/linux_basic/0410accountmanager.php)

## Linux修改ssh端口22

修改端口配置文件,取消Port前的#注释，并将端口22改成65535，可以改成从1025到65536之间的任意一个整数     

## 公私钥认证

1. 执行命令ssh-keygen -t rsa -b 4096，根据需要可以输入密码，在~/.ssh目录下生成公钥私钥id_rsa.pub和id_rsa
2. copy公钥id_rsa.pub到连接这台服务器的其他服务器上
3. 添加公钥到.ssh/authorized_keys中，cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
4. 设置权限
    * chmod 0700 ~/.ssh/
    * chmod 0600 ~/.ssh/authorized_keys
5. 保险起见，删除公钥 rm id_rsa.pub
6. 接着修改ssh配置文件，开启如下内容
    * RSAAuthentication yes
	* PubkeyAuthentication yes
	* AuthorizedKeysFile      .ssh/authorized_keys
	* 禁止password方式登录 PasswordAuthentication no
	* 禁止root登录 PermitRootLogin no
	* 禁止空密码 PermitEmptyPasswords no
	* 添加允许登录的用户名（在最后加上） AllowUsers abc
	* 重启sshd服务,sudo service sshd restart
3. 在ssh工具，比如xshell，配置证书登录
    1. 新建连接
	2. 点击Authentication, --Method选择Public Key，点击Browse
	3. 点击导入私钥，需要输入之前生成秘钥时设置的密码；如果出现导入秘钥错误，尝试导入公钥时会出现此问题
4. 以后登录只能使用证书登录，密码无法登录

### 公钥登录原理
所谓"公钥登录"，原理很简单，就是用户将自己的公钥储存在远程主机上。登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回来。远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录shell，不再要求密码。
