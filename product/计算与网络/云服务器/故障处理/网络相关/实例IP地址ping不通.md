## 现象描述

### 前提条件

本地网络正常，即可以正常 ping 通其他网站。

### 故障现象

本地主机 ping 不通实例。

## 可能原因

目标服务器的设置不正确、域名没有正确解析、链路故障等问题引起。

## 处理步骤

### 检查实例是否配置公网 IP

>? 实例必须具备公网 IP 才能与 Internet 上的其他计算机相互访问。若实例没有公网 IP，内网 IP 外部则无法直接 ping 通实例。
>
1. 登录 [云服务器控制台](https://console.cloud.tencent.com/cvm/index)。
2. 在“实例列表”页面中，选择需要 ping 通的实例 ID/实例名，进入该实例的详情页面。如下图所示：
![](https://main.qcloudimg.com/raw/133d6bd76dd3eb2a4da2ef0e657f7bee.png)
3. 在“网络信息”栏，查看实例是否配置了公网 IP。
若该实例未配置公网 IP，可 [绑定弹性公网 IP](https://cloud.tencent.com/document/product/213/16586#.E5.BC.B9.E6.80.A7.E5.85.AC.E7.BD.91-ip-.E7.BB.91.E5.AE.9A.E4.BA.91.E4.BA.A7.E5.93.81)。

### 检查安全组设置

>? 安全组是一个虚拟防火墙，可以控制关联实例的入站流量和出站流量。而安全组的规则可以指定协议、端口、策略等。由于 ping 使用的是 ICMP 协议，实例关联的安全组须允许 ICMP 协议。
>
1. 登录 [云服务器控制台](https://console.cloud.tencent.com/cvm/index)。
2. 在“实例列表”页面中，选择需要安全组设置的实例 ID/实例名，进入该实例的详情页面。
3. 选择【安全组】页签，进入该实例的安全组管理页面，即可查看该实例所使用的安全组以及详细的入站和出站规则。如下图所示：
![](https://main.qcloudimg.com/raw/3885ca805ee77d7d7f4843e28eba68b3.png)

### 检查系统设置

判断实例的操作系统类型，选择不同的检查方式。
- Linux 操作系统，请 [检查 Linux 内核参数和防火墙设置](#CheckLinux)。
- Windows 操作系统，请 [检查 Windows 防火墙设置](#CheckWindows)。

<span id="CheckLinux"></span>
#### 检查 Linux 内核参数和防火墙设置

>? Linux 系统是否允许 ping 由内核和防火墙设置两个共同决定，任何一个禁止，都会造成 ping 包 “Request timeout”。

##### 检查内核参数 icmp_echo_ignore_all

1. 登录实例。
2. 执行以下命令，查看系统 icmp_echo_ignore_all 设置。
```
cat /proc/sys/net/ipv4/icmp_echo_ignore_all
```
 - 若返回结果为0，表示系统允许所有的 ICMP Echo 请求，请 [检查防火墙设置](#CheckLinuxFirewall)。
 - 若返回结果为1，表示系统禁止所有的 ICMP Echo 请求，请执行 [步骤3](#Linux_step03)。
3. <span id="Linux_step03">执行以下命令，修改内核参数 icmp_echo_ignore_all 的设置。</span>
```
echo "1" >/proc/sys/net/ipv4/icmp_echo_ignore_all
```

<span id="CheckLinuxFirewall"></span>
##### 检查防火墙设置

执行以下命令，查看当前服务器的防火墙规则以及 ICMP 对应规则是否被禁止。
```
iptables -L
```
若返回如下结果，表示 ICMP 对应规则未被禁止，请 [检查域名是否备案](#CheckDomainRegistration)。
```
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     icmp --  anywhere             anywhere             icmp echo-request

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination  
ACCEPT     icmp --  anywhere             anywhere             icmp echo-request
```

<span id="CheckWindows"></span>
#### 检查 Windows 防火墙设置

1. 登录实例。
2. 打开【控制面板】，选择【Windows 防火墙设置】。如下图所示：
![](https://mc.qcloudimg.com/static/img/e5e6a914dbdaf1f0dab5e89440d7662e/image.png)
3. 在 “Windows 防火墙”界面，选择【高级设置】。如下图所示：
![](https://mc.qcloudimg.com/static/img/247440c6c79697133685cbf16544d2cc/image.png)
4. 在弹出的 “高级安全 Windows 防火墙”窗口中，查看 ICMP 有关的出入站规则是否被禁止。
如下图所示，ICMP 有关的出入站规则被禁用，请启用该规则。
![](https://main.qcloudimg.com/raw/8bf6bc333e172425de6ede53d70d5978.png)

<span id="CheckDomainRegistration"></span>
### 检查域名是否备案

>? 如果您可以 ping 通公网 IP，而 ping 不通域名，可能是域名没有备案或者域名解析的问题导致。
>
国家工信部规定，对未取得许可或者未履行备案手续的网站不得从事互联网信息服务，否则就属于违法行为。为不影响网站长久正常运行，如需开办网站，建议您先办理网站备案，待备案成功取得通信管理局下发的 ICP 备案号后，才开通访问。
- 如果您的域名没有备案，请先进行 [域名备案](https://console.cloud.tencent.com/beian)。
- 如果您使用的是腾讯云的域名服务，您可以登录 [域名服务控制台](https://console.cloud.tencent.com/domain) 查看相应的域名情况。
- 如果您的域名已备案，请 [检查域名解析](#CheckDNS)。

<span id="CheckDNS"></span>
### 检查域名解析

ping 不通域名的另外一个原因是由于域名解析没有正确地配置。如果您使用的是腾讯云的域名服务，您可以执行以下操作，检查域名解析。
1. 登录 [域名服务控制台](https://console.cloud.tencent.com/domain)。
2. 在“我的域名”管理页面，选择需检查域名解析的域名行，单击【解析】，查看域名解析详情。如下图所示：
![](https://main.qcloudimg.com/raw/c350c1587af72d8a3529bcd7a98da856.png)

<span id="OtherOperations"></span>
### 其它操作

若上述步骤无法解决问题，请参考：
- 域名 ping 不通，请检查您网站配置。
- 公网 IP ping 不通，请附上实例的相关信息和双向 MTR 数据（从本地到云服务器以及云服务器到本地），[提交工单](https://console.cloud.tencent.com/workorder/category) 联系工程师协助定位。
MTR 的使用方法请参考 [服务器网络延迟和丢包处理](https://cloud.tencent.com/document/product/213/14638)。
