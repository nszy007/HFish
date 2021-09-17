### 网络要求

> HFish控制端会主动访问如下网络域名

HFish支持IPv4和IPv6地址环境，可以在完全隔离互联网的内部网络工作，但为了最大限度感知真实威胁和对接云端接口消费威胁情报，以及接受自动化升级服务，微步在线强烈建议客户允许HFish控制端访问互联网，为兼顾安全性和服务可用性，推荐用户仅允许HFish控制端主动访问如下网络域名、地址和端口：

| 目的IP                                                   | 协议/端口 | 对应域名            | 访问目的                                         |
| ------------------------------------------------ ------- | --------- | ------------------- | ------------------------------------------------ |
| 103.210.21.74                                            | TCP/443   | hfish.io            | **用于官网升级功能，建议开启**                       |
| 该域名使用CDN解析，建议在实际网络中解析后开放权限        | TCP/443   | hfish.cn-bj.ufileos.com   | 用于分发安装和升级包                          |
| 106.75.36.224  123.59.72.253  123.59.51.113  106.75.36.226  117.50.17.104 | TCP/443   | api.threatbook.cn   | 用于威胁情报查询，如果未启用该功能，无需开放     |
| 该域名使用CDN解析，建议在实际网络中解析后开放权限        | TCP/443   | open.feishu.cn      | 用于飞书告警功能，如果未使用该功能，无需开放     |
| 该域名使用CDN解析，建议在实际网络中解析后开放权限        | TCP/443   | oapi.dingtalk.com   | 用于钉钉告警功能，如果未使用该功能，无需开放     |
| 该域名使用CDN解析，建议在实际网络中解析后开放权限        | TCP/443   | qyapi.weixin.qq.com | 用于企业微信告警功能，如果未使用该功能，无需开放 |

 

注意：

1. HFish控制端仅需要通过NAT模式访问互联网，基于安全考虑，不建议用户将HFish控制端控制端口TCP/4433暴露在互联网；

2. 如果使用邮件通知，请开启相应邮件服务器的访问权限；

3. HFish支持 5 路syslog日志发送，便于与安全设备联动，请根据实际情况开放权限；



### 安全配置

##### 控制端安全要求

> 控制端应部署在安全区，只向少部分有网络管理权限和安全分析能力工作的人员和设备开放web和ssh端口

控制端用于配置管理的web页面开启了https，默认访问端口为tcp/4433，端口和登录URL，可以在config.ini中自行配置。

如果控制端主机的IP为192.168.11.11，那么应该在浏览器中输入如下的URL进行访问。

https://192.168.11.11:4433/web/


此外控制端还会开放另外两个端口，节点数据回传端口tcp/4434和SSH服务默认访问端口tcp/22。

强烈建议：

- **tcp/4433端口和tcp/22端口 “只能” 被安全区的管理设备访问**；
- **tcp/4434端口“必须能”被蜜罐节点访问**;



##### 节点端安全要求

> 节点直接面对攻击者，建议遵循以下安全配置：

1. 如果用户同时有外网和内部需求，强烈建议用户分别在外网和内网部署完全独立的两套控制端和节点端；
2. 如果有节点需要能被外网访问，那么建议把节点和控制端部署在DMZ区；
3. 外网节点除了能访问控制端的tcp/4434端口外，不能有权限访问内网中的任何资产；
4. 内网节点除了开放蜜罐服务相应端口外，其它任何端口都不应该在网络中能被用户访问到。考虑安全区设备有维护节点主机的需求，可以向有限的设备开放ssh端口；