\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[zhuanlan.zhihu.com\](https://zhuanlan.zhihu.com/p/165943024)

> 作者：Kerwin\_ 链接：[https://juejin.im/post/5f18683af265da22f84d7602](https://juejin.im/post/5f18683af265da22f84d7602)

**前言**
------

大家都是做开发的，都有 GitHub 的账号，在日常使用中肯定会遇到这种情况，在不修改任何配置的情况下，有时可以正常访问 GitHub，有时又直接未响应，来一起捋捋到底是为啥。

**GitHub 访问的千层套路**
------------------

以我家里的电脑为例，在不修改任何配置，不启用什么代理工具的情况下，访问 GitHub 会得到以下结果：

![](https://pic2.zhimg.com/v2-744bfd85acc7674b4b7fe0853bacc5a1_r.jpg)

虽然很戳心，但好歹能展示一部分。

从网上搜了一堆乱七八糟的攻略，知道了可以通过修改电脑的 Hosts 文件达到正常访问的能力，于是胡搜了一通，

步骤：**百度经验**

效果如下：

![](https://pic3.zhimg.com/v2-498207c2a75c40517a91d371392e8f82_r.jpg)

访问效果依然很感人，最近活动数据不显示，整个界面加载都快接近 2 分钟了，有什么办法没有咧~

**站长工具 PING PING PING**
-----------------------

都是搞开发的，都会用 F12 看看网络或者资源请求的地址是什么，以上面耗时最慢的地址为例，域名为：[http://github.githubassets.com](http://github.githubassets.com)

打开站长工具的 PING 功能，地址为：[http://ping.chinaz.com/github.githubassets.com](http://ping.chinaz.com/github.githubassets.com)

结果如下：

![](https://pic4.zhimg.com/v2-54ce61d0c5b0a0983acf7720cbb42c2b_r.jpg)

我发现 185.199.108.154 这个 IP 地址速度快的一批，于是立马更换 Hosts 中该域名对应的 IP 地址

再次访问，效果如下：

![](https://pic1.zhimg.com/v2-a4cc883cff4a4630c178cf26df96348c_r.jpg)

那句话怎么说的来着？如什么什么般丝滑，我感觉这就非常丝滑~

**GitHub 项目定时发布最新 Hosts**
-------------------------

当然了，如果每次访问都得折腾一次，那滋味，简直不要太难受，所以网上已经有人开源了相关的项目，会定时发布最新的 GitHub IP 地址，链接：[https://github.com/521xueweihan/GitHub520](https://github.com/521xueweihan/GitHub520)

本文撰写时的 Hosts

```
\# github
185.199.108.154               github.githubassets.com
199.232.68.133                camo.githubusercontent.com
52.168.24.190                 github.map.fastly.net
199.232.69.194                github.global.ssl.fastly.net
140.82.112.4                  github.com
140.82.112.5                  api.github.com
199.232.68.133                raw.githubusercontent.com
199.232.68.133                user-images.githubusercontent.com
199.232.68.133                favicons.githubusercontent.com
199.232.68.133                avatars5.githubusercontent.com
199.232.68.133                avatars4.githubusercontent.com
199.232.68.133                avatars3.githubusercontent.com
199.232.68.133                avatars2.githubusercontent.com
199.232.68.133                avatars1.githubusercontent.com
199.232.68.133                avatars0.githubusercontent.com
复制代码

```

该项目会自动发布在指定的地址上，结合软件使用，可以完全自动化，无需持续更新

当然也可以自行手动更改

**为什么改了 Hosts 就能访问 GitHub**
---------------------------

平常都是百度 + 谷歌，今天非要探究一下原理！咱们一步一步来，首先大家都需要明确一点，在网络的世界中 域名 只是为了便于记忆和识别而存在的一个唯一地址，真正工作的仍然是 IP

**Hosts 文件是干吗的**
----------------

简单来说，Hosts 文件是存储本机网址域名与其对应的 IP 地址的一个文件，在网络请求阶段发挥作用

**为什么改了 Hosts 就能生效**
--------------------

这就涉及到了域名解析，因为 Hosts 文件存放的就是 域名 和 IP 的对应关系，因此它可以在域名解析阶段发挥作用，为什么呢？因为在域名解析的流程中 本机 Hosts 解析处于顺序二

即：浏览器解析 -》本机解析 -》XXXX（后面的稍后再提）

所以有时候我们白嫖软件，都会改一下 Hosts，因为需要把它在线验证的域名指向错误的地址去，另外可能存在一定的浏览器缓存或者本机缓存，可以通过重开浏览器或者 PING 域名来检查更改是否生效。

**DNS 解析到底是什么玩意？**
------------------

上文中多次提到解析，其实说的就是 DNS 解析

同时上文也提到过，在网络世界中真正发挥作用的是 IP，而一般情况下我们访问的都是 域名，为什么能实现这种效果，就是因为域名与 IP 地址的对应关系存储在一个叫做 DNS（Domain Name System） 的系统里。DNS 是一个全球化的分布式数据库，它所提供的服务就是将域名转换为互联网 IP 地址。

**DNS 解析的全部流程**
---------------

网上的关于流程的图很多，我从中借鉴了一副，如下所示：

![](https://pic4.zhimg.com/v2-e747e0fda317805f955a37cb84a9ebb3_r.jpg)

1.  浏览器缓存：一次请求会首先通过浏览器缓存信息寻找域名映射的 IP 地址，这也是为什么有时候我们改了本机 hosts，需要关闭再打开浏览器才能正常使用，如果找到则返回，没找到则继续到下一级
2.  本机系统缓存：即上文中提到的，通过 hosts 文件来映射域名和 IP，在上古时期有很多垃圾软件会悄咪咪的修改系统的 hosts 文件，达到 DNS 劫持 的目的，即把淘宝域名指向另一个 IP，然后部署一个高仿的淘宝商城，静静等你输入账号，密码，然后凉凉...
3.  本地域名解析服务系统：本地域名系统 LDNS 一般都是本地区的域名服务器。离你的位置都比较近，Windows 系统使用命令 ipconfig 就可以查看，在 Linux 和 Mac 系统下，直接使用命令 cat /etc/resolv.conf 来查看 LDNS 服务地址。 LDNS 一般都缓存了大部分的域名解析的结果，大部分的解析工作到这里就差不多已经结束了 以下即是所谓的 递归解析
4.  根域名解析：本地域名解析服务系统无法解析时，会向 13 根 发起域名解析请求 说明： 所谓的 13 根，指的是根域名服务器，是架构因特网所必须的基础设施。根服务器主要用来管理互联网的主目录，由于 DNS 解析中采用的是 UDP 协议，仅能传递 512 字节的有效报文，因此只能构建出 A-M 13 个根服务器，而真正工作运行肯定不止 13 台服务器，而是包含很多服务器镜像的
5.  根域名解析服务器返回 gTLD (Generic top-level domain) 给本地解析服务器，即该域名所属的顶级域及其所在的服务器，顶级域名即如：.com .cn 等等
6.  本地解析服务器已知顶级域名服务器地址后，发起解析请求
7.  顶级域名解析服务器返回 权限域名服务器 信息给本地解析服务器，权限域名服务器 即如：[http://taobao.com](https://link.zhihu.com/?target=http%3A//taobao.com)
8.  本地解析服务器已知权限域名服务器地址后，发起解析请求
9.  权限域名服务器返回域名对应的 IP 地址给本地解析服务器
10.  本地解析服务器缓存相关信息，并返回给用户

是不是有点绕？咱们来整个图吧，递归解析 如下所示：

![](https://pic3.zhimg.com/v2-363c4d3246845b77faaa02db2832da32_r.jpg)

**再问一遍为什么改 Hosts 就可以访问 GitHub**
-------------------------------

了解了上文之后，对于这个问题就更好回答了，因为 GitHub 毕竟为外国的网站，咱们访问时有一层 DNS 污染，即把对应的域名指向了不可达的 IP 上，或者禁止访问的 IP 上，因此很多时候无法使用

修改 Hosts 文件后即避免了 DNS 污染，直达目标 IP，即可正常访问了，当然了，这种方法是全部通用吗？

答案：肯定不是，因为刚才也提到了，网关层是可以控制某些 IP 禁止访问的

整一个工具来验证一下猜想，顺便看看我们的整个请求流程：

软件名：BestTrace

![](https://pic4.zhimg.com/v2-012ac1af2eb932e96f5048e4500e1c0f_r.jpg)

我请求的域名是 [http://github.githubassets.com](https://link.zhihu.com/?target=http%3A//github.githubassets.com)，最终请求接收方 IP 和我 Hosts 配置的 IP 一致，那我换一个 [http://facebook.com](https://link.zhihu.com/?target=http%3A//facebook.com)

![](https://pic4.zhimg.com/v2-7cdefe4229f8d5bc008fd77de3cb75cb_r.jpg)

可以看到，当请求到达 221.183.46.249 这个 IP 时，整个请求就被拦截下来了，因此这并不是万能的办法

除了访问 GitHub，还有什么时候可能用到呢？

比如下载 IDEA 插件时，如果发现老是刷新不出来插件库，或者下载失败，就可以通过 PING 工具去配置最佳 IP，方便下载~

**DNS 除了解析还能做什么**
-----------------

**智能 DNS**
----------

网络请求交由域名解析服务器来处理，分配到最佳的服务器 IP 上

例如：请求的源头是电信还是联通等，如果是电信则将解析的 IP 分流到电信对应的 IP 上，或者返回距离最近的服务器 IP 地址

**反向代理水平扩展**
------------

典型的互联网架构中，可以通过增加 web-server 来扩充 web 层的性能，但反向代理 nginx 仍是整个系统的唯一入口

**如果系统吞吐超过 nginx 的性能极限**，那么将难以扩容，此时就需要 dns-server 来配合水平扩展。

即 DNS 解析服务器有序的把域名解析到不同的网关层，每次 DNS 解析请求，轮询返回不同的 ip，这样就能实现 nginx 的水平扩展，这个方法叫 “**DNS 轮询**”

写下你的评论...  

然而对于 assets 来说这方法没有任何作用。为什么改了就行了呢？运气。