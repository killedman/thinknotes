title: 华硕 B85-PRO GAME + NVIDIA GeForce GTX 750 实现双屏输出
date: 2018-05-09

## 折腾的一天
考虑了很长时间后购买了一个24寸的Dell U2415显示器，可以升级、旋转的显示器，加上大小和色彩都非常好，到货后很激动，也很满意，
接着突然发现早上下单时买的数据线有问题，显示器本身带一根DP的线，我已经提前看了PC主机的接口，独立显卡带的是Mini HDMI接口和
两个DVI的接口，主板上的接口是DVI和HDMI的，所有是要单独买一根Mini HDMI接口的线接到独立显卡上实现双屏输出；原有的Philips 202e
显示器是DVI和VGA接口，使用DVI接口接到独立显卡上

### 使用购买的HDMI-HDMI线测试新购Dell U2415显示器
将HDMI线两头分别接Dell U2415的HDMI 1接口和主板HDMI接口，开机无显示器，需要重启PC按del键进入主板BIOS，高级模式中找到代理选项，显示
项中选择iGPU,并选择启动iGPU,重启之后HDMI连接的显示器正常显示。

### 测试Dell U2415接主板HDMI接口，Philips 202e接主板DVI接口双屏输出

结论：无法实现

### 测试Dell U2415接主板HDMI接口，Philips 202e接独立显卡DVI接口双屏输出

结论：无法实现

### 测试Dell U2415接独立显卡mini HDMI接口，Philips 202e接独立显卡DVI接口双屏输出
购买Mini HDMI-HDMI数据线后，将两个显示器都接到独立显卡上实现双屏输出