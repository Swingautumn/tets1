---
title: Nginx笔记
copyright_author: 王耀福
abbrlink: 41784
---
# Nginx
**Nginx**

是一种使用范围广、运维必备的技能，它出现的背景是互联网数据不断增长、Apache处理请求逐渐低效。它采用了模块化设计，具有高聚合低耦合的特点，既是一种服务器软件、也是一种负载均衡软件。

关键词：

《高并发、高性能》《扩展性好》《异步非阻塞的事件驱动模型》《高可靠性》《热部署、平滑升级》《BSD许可证》《静态资源服务器》、反向代理》《单线程，多进程》

**关于master 与worker**

处理请求的是work进程，work进程由master进程进行管理控制

**热部署**

《kill -s SIGUSR2 pid号》、《kill -s SIGWINCH pid号》、《kill -s SIGQUIT pid号》、《kill -s SIGHUP pid号（回滚）》、

原理：master读取新配置生产新进程，新旧进程并存，然后master进程发送命令等待旧进程工作完后将其结束

一对多，非阻塞式——一个进程处理多个请求并且非阻塞式，Apache是多对一，阻塞式

./configure --prefix=/usr/local/nginx --with-http\_stub\_status\_module --with-http\_ssl\_module 编译设置

缓存服务器、开启http服务加密数据保障业务安全、快速搭建开发测试环境对代码进行测试

**反向代理、负载均衡、动静分离、处理高并发能力强**单位web服务器

正向代理——客户端配置代理服务器

反向代理——客户端无配置，直接将请求发送到反向代理服务器，由代理服务器寻找内容并返回，反向代理服务器与相应的服务器的IP暴露统一为前者

**常见负载均衡**——集群的使用

搭配tomcat使用

需要在对应的/usr/local/nginx/conf/nginx.conf文件操作，节点为upstream，涉及的知识点包括：轮询、weight、ip\_hash、fair等

这里既需要配置nginx文件也需要配置tomcat文件

**所谓动静分离**——将请求分为动态请求和静态请求 ，并进行分别解析

nginx处理静态，tomact处理动态

1、将静态资源文件独立成单独的域名放在独立的服务器上(主流)

2、动态静态资源不分离，直接混合发布，之后通过nginx分开

sbin目录下操作——./nginx、./nginx -s stop、./nginx -s reload

原理：通过location设置分流不同的请求，同时通过expires参数设置浏览器缓存资源过期时间减少与服务器之间的请求和流量(注意该资源通常为不常变动更新的资源)

**配置文件路径**：/usr/local/nginx/conf/nginx.conf    

**文件组成**：

全局块(main)、events块、http 块（http全局与Server核心）、server块

全局块(main)：

user/pid/work\_rlimit\_nofile/worker\_rlimit\_core/worker\_cpu\_affinity/worker\_priority/....worker子进程与CPU绑定

events块：

use、worker\_connections、accept\_mutes、accept\_mutes\_delay、muti\_accept	影响 Nginx 服务器与用户的网络连接

http 块（http全局与Server核心）：

server\_name的值可以是IP地址，域名，通配符域名 ，server\_name指令优先级精确匹配等级最高，越模糊等级越低，右通配符>左0

location块：

注意alias与root的使用，以及其他的一些寻找用法：=、~(正则匹配语法)、^~、url的反斜线处理原则，不带/，表示先将对应文件作为目录如果无则作为整个文件，带的话容易返还404、

高可用的效果——搭建副服务器进行数据备份，当主服务器宕机副服务武器自动接入、两个服务器、keepalive、虚拟IP

**nginx网络配置**

limit\_conn模块、——常用指令包括：limit\_conn\_zone/\_status/\_log\_level/...

limit\_req模块——常用指令包括：limit\_req\_zone/\_status/\_log\_level/...

allow与deny允许或拒绝IP主机的访问

auth\_basic模块限制指定用户访问

auth\_request模块 基于子请求收到的响应码进行访问控制

return指令、rewrite指令、if指令、一些限制字段：break、last

**autoindex模块**

autoindex/\_exact\_size/\_format/\_localtime

**stub\_status模块**

`	`**官**方提供的用于实时监控Nginx服务器状态信息的模块

`	`常见状态参数包括：active connections、accepts/handlied/requests、$connections\_active/$connections\_reading/...等

关于变量

TCP连接变量、HTTP请求变量、nginx处理请求产生的变量、nginx返回响应变量、nginx内部变量

remote\_addr、remote\_port、server\_addr、server\_port、......

**nginx反向代理**

动静分离、upstream模块——upstream、server、keepalive、queue

处理接受到的用户请求包体的方法：接收完再发送、边接收边发送

与上游服务器建立连接keepalive、proxy、

**负载均衡**

将请求代理到多台服务器处理

upstream、keepalive、queue、weight权重、

负载均衡策略：

`	`轮询（Round Robin）：最简单的策略，按顺序将请求分配给后端服务器。

加权轮询（Weighted Round Robin）：基于轮询的扩展，允许为不同服务器分配不同权重，权重越高处理请求越多。

IP哈希（IP Hash）：基于客户端IP地址的哈希值分配请求，确保同一IP的请求发送到同一服务器。

最少连接（Least Connections）：将请求分配给当前连接数最少的服务器，以平衡负载。

负载均衡算法：

`	`哈希算法 ，根据请求特定的值，响应回复的值也进行特定指定

`	`ip\_hash算法，根据IP地址进行哈希运算

`	`最少连接算法，least\_conn指令、《worker与多台服务器、共享内存》、zone指令、

**容错机制**

`	`nginx连接服务器，服务器因故宕机或无法处理请求、error、《将处理失败的请求转发给其他服务器处理》、《通过指令设置超时时间和跳转其他服务器处理》、《error与超时，前者是服务器本身后者可能是网络请求》

**关于缓存**

`	`便于回应信息，提高用户体验

`	`proxy\_cache指令集——操作缓存

`	`不缓存特定内容——proxy\_no\_cache、proxy\_cache\_bypass

`	`缓存失效降低上游压力机制——合并源请求：proxy\_cache\_lock、proxy\_cache\_lock\_timeout/\_age

启用陈旧缓存：proxy\_cache\_use\_stale，将一个请求获取到的资源对相应的缓存进行同步更新以满足后续相同请求减少服务器压力

第三方清除模块——proxy\_cache\_purge、清除缓存、增强灵活性

**https原理基础**、

`	`HTTPS=http+TLS/SSL、

`	`数据加密：对称加密算法（解密效率高但是传输的安全性无法保障，数目难于管理，无法校验提供的信息的完整性 ）、非对称加密算法（公钥私钥，优势是服务器仅维持一个私钥即可，劣势是公钥是公开的，非对称加密算法加解密过程中会耗费一点时间，公钥不包含服务器信息存在中间人攻击的可能）

`	`HTTPS是混合使用两种加密方法，连接建立阶段使用非对称，传输使用对称、添加数字签名、报文被纂改，身份被伪装

私有CA服务器

`	`**CA服务器**

`	`openssl、csr文件、pem文件、key文件、crt文件、证书生成并签发——《crt文件、key文件》；ssl\_certificate(\_key)指令、《TSL/SSL协议》

nginx高可用

`	`服务可用判定三要素：IP地址、监听端口、数据文件

**配置vue项目**

视情况而定，在对应的location路径中添加改语句

try\_files $uri $uri/ /index.html


