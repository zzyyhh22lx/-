
大家都有过这样的经历，拨打10086客服电话，可能一个地区的10086客服有几个或者几十个，你永远都不需要关心在电话那头的是哪一个，叫什么，男的，还是女的，漂亮的还是帅气的，你都不关心，你关心的是你的问题能不能得到专业的解答，你只需要拨通了10086的总机号码，电话那头总会有人会回答你，只是有时慢有时快而已。那么这里的10086总机号码就是我们说的反向代理。客户不知道真正提供服务人的是谁。反向代理隐藏了真实的服务端，当我们请求[http://ww.baidu.com](https://link.zhihu.com/?target=http%3A//ww.baidu.com) 的时候，就像拨打10086一样，背后可能有成千上万台服务器为我们服务，但具体是哪一台，你不知道，也不需要知道，你只需要知道反向代理服务器是谁就好了，[http://www.baidu.com](https://link.zhihu.com/?target=http%3A//www.baidu.com) 就是我们的反向代理服务器，反向代理服务器会帮我们把请求转发到真实的服务器那里去。Nginx就是性能非常好的反向代理服务器，用来做负载均衡。

两者的区别在于代理的对象不一样：正向代理是为客户端代理，反向代理是为服务端代理。



![img](https://pic2.zhimg.com/80/v2-8c4054a09e4fa02d5a2c62a65304d37d_720w.jpg)



nginx能实现负载均衡，什么是负载均衡呢？就是我的项目部署在不同的服务器上，但是通过统一的域名进入，nginx则对请求进行分发，减轻了服务器的压力。

在上面这两种情况下，nginx服务器的作用都只是作为分发服务器，真正的内容，我们可以放在其他的服务器上，这样来，还能起到一层安全隔壁的作用，nginx作为隔离层。

其次，nginx还能解决跨域的问题。

之前我们搭建网站的时候，把war包放到tomcat下就能运行起来了，为什么部署上线的时候，又用到了nginx呢？
nginx可以做多台服务器的负载均衡，当用户非常少的时候，可以用一台服务直接部署web环境，那么当用户达到百万级别，千万级别的时候，就需要增加服务器，多台服务器又如何管理协作的呢？

nginx有以下功能：
1.静态HTTP服务器-Nginx是一个HTTP服务器，可以将服务器上的静态文件（如HTML、图片）通过HTTP协议展现给客户端。
2.反向代理服务器-客户端本来可以直接通过HTTP协议访问某网站应用服务器，网站管理员可以在中间加上一个Nginx，客户端请求Nginx，Nginx请求应用服务器，然后将结果返回给客户端，此时Nginx就是反向代理服务器。
3.负载均衡-当网站访问量非常大，网站站长开心赚钱的同时，也摊上事儿了。因为网站越来越慢，一台服务器已经不够用了。于是将同一个应用部署在多台服务器上，将大量用户的请求分配给多台机器处理。同时带来的好处是，其中一台服务器万一挂了，只要还有其他服务器正常运行，就不会影响用户使用。
4.虚拟主机-有的网站访问量大，需要负载均衡。然而并不是所有网站都如此出色，有的网站，由于访问量太小，需要节省成本，将多个网站部署在同一台服务器上。
5.FastCGI-Nginx本身不支持PHP等语言，但是它可以通过FastCGI来将请求扔给某些语言或框架处理（例如PHP、Python、Perl）。



## **什么是nginx?**

Nginx是一款自由的、开源的、高性能的HTTP服务器和反向代理服务器；同时也是一个IMAP、POP3、SMTP代理服务器；
Nginx可以作为一个HTTP服务器进行网站的发布处理，另外Nginx可以作为反向代理进行负载均衡的实现。

正向代理，代理的是客户端，比如小伙伴们平常科学上网,访问google网站就是用到的正向代理。

![img](https://pic1.zhimg.com/80/v2-62f39264c8db5db6b7a00523e0165900_720w.jpg)

反向代理，它代理的是服务端，主要用于服务器集群分布式部署的情况下，反向代理隐藏了服务器的信息。



![img](https://pic1.zhimg.com/80/v2-4506019643375c32e0de37d1b25cb590_720w.jpg)