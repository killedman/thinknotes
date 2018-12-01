---
title: Linux系统下注册node exporter为systemd服务
date: 2018-05-31
---


## add user prometheus

1. 在root下执行以下命令，添加新用户prometheus
    useradd prometheus
    passwd prometheus
    su - prometheus
    mkdir /home/prometheus/.ssh
    cd /home/prometheus/.ssh;touch authorized_keys
2. 将xshell存储的服务器中另外一个账号的的公钥内容写入authorized_keys
    * 在xshell中查看公钥内容的方法：xshell>工具>用户密钥管理者>选中需要的密钥>属性>公钥
3. 修改新建用户prometheus下目录权限： chmod 0700 ~/.ssh/;chmod 0600 ~/.ssh/authorized_keys
    * 如果不修改权限，可能会遇到报错：”所选的用户密钥未在远程主机上注册“，使用root账号查看/var/log/security,看到报错 Authentication refused: bad ownership or modes for file /home/prometheus/.ssh/authorized_keys
4. 如果在ssh配置了限制登录的用户记得在/etc/ssh/sshd_config中添加AllowUsers prometheus，然后重启ssh服务


## Running Node Exporter as a Service

1. To make it easy to start and stop Node Exporter, let us now convert it into a service.
2. Use vi or any other text editor to create a unit configuration file called node_exporter.service.
    sudo vi /etc/systemd/system/node_exporter.service
3. This file should contain the path of the node_exporter executable, and also specify which user should run the executable. Accordingly, add the following code:

    <pre>
    [Unit]
    Description=Node Exporter
    [Service]
    User=prometheus
    ExecStart=/home/prometheus/node_exporter/node_exporter
    [Install]
    WantedBy=default.target
    </pre>

4. Save the file and exit the text editor.
5. Reload systemd so that it reads the configuration file you just created.
    sudo systemctl daemon-reload
6. At this point, Node Exporter is available as a service which can be managed using the systemctl command. Enable it so that it starts automatically at boot time.
    sudo systemctl enable node_exporter.service
7. You can now either reboot your server, or use the following command to start the service manually:
    sudo systemctl start node_exporter.service
8. 参考：https://www.digitalocean.com/community/tutorials/how-to-use-prometheus-to-monitor-your-centos-7-server


## 批量注册Node Exporter为系统服务
5. 以上命令在root用户下操作
1. 准备文件node_exporter.service，内容如下

    <pre>
    [Unit]
    Description=Node Exporter
    [Service]
    User=prometheus
    ExecStart=/home/prometheus/node_exporter/node_exporter
    [Install]
    WantedBy=default.target
    </pre>

2. cp node_exporter.service /etc/systemd/system
3. 执行命令： systemctl daemon-reload
4. 执行命令 systemctl enable node_exporter.service


## 使用python脚本进行批量注册操作(需要在root下执行）

    #!/usr/bin/env python
    
    import os
    import shutil
    import subprocess
    import sys
    import getpass
    
    def node(filename,to_dir):
        if not os.path.exists(filename):
            print(filename + 'is not exists')
            sys.exit(1)
        else:
            shutil.copy(filename,to_dir + '/' + os.path.basename(filename))
    
    def register():
        cmd1 = 'systemctl daemon-reload'
        cmd2 = 'systemctl enable node_exporter.service'
        (status1,output1) = subprocess.getstatusoutput(cmd1)
        (status2,output2) = subprocess.getstatusoutput(cmd2)
        if status1:
            sys.stderr.write(output1)
            sys.exit(1)
        if status2:
            sys.stderr.write(output2)
            sys.exit(1)
    def check(filename,to_dir,symlink):
        if os.path.exists(os.path.join(to_dir,os.path.basename(filename))) and os.path.exists(symlink):
            print("register node_exporter as a server success")
        else:
            print("register node_exporter as a server fail")
    
    def main():
        filename = '/home/prometheus/node_exporter.service'
        to_dir = "/etc/systemd/system"
        symlink = '/etc/systemd/system/default.target.wants/node_exporter.service'
        node(filename,to_dir)
        register()
        check(filename,to_dir,symlink)
    
    if __name__ == "__main__":
        if getpass.getuser() != 'root':
            print("the python shell need run as user 'root' ")
            sys.exit(1)
        else:
            main()

## 备忘
    systemctl enable node_exporter.service
    Created symlink from /etc/systemd/system/default.target.wants/node_exporter.service to /etc/systemd/system/node_exporter.service.