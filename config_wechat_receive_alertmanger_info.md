---
title: 通过企业微信接收alertmanager信息
date: 2018-06-09
---

PC端浏览器访问 https://work.weixin.qq.com/login 使用手机微信扫描进入企业微信管理界面，需要是企业微信管理员，依次进入：菜单栏>企业应用>创建应用，完成创建应用的操作，获取agent_id；在菜单栏>我的企业，获取corp_id

## alertmanger 配置-通过企业微信接收alertmanager信息

<pre>
route:
  group_by: ['alertname']
  receiver: 'wechat
  
receivers:
  - name: "wechat"
    wechat_configs:
      - send_resolved: true
        to_user: "" # 发送给哪些用户 @all发给 该应用的全部关注者,可以参考微信api文档,可以写具体某人微信号,多人用|分割
        to_party: "" # 发给某个部门组织
        to_tag: ""
        agent_id: "xxx" # 发给到具体哪个应用, 联系微信管理员可获得
        corp_id: "xxxx" ## 企业号的corp id 找企业号管理员可以获得,同全局设置
<pre>

## 参考： 

1. https://songjiayang.gitbooks.io/prometheus/content/alertmanager/wechat.html
2. https://berlinsaint.github.io/blog/2018/01/15/Alertmanager%E6%94%AF%E6%8C%81%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E5%95%A6/