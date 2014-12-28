---
layout: post
category: "Reading"
title: "使用Jekyll搭建个人博客"
tags: ["Jekyll"]
---
<h3>1.简介</h3>
<p>
之前一直用新浪博客或者CSDN博客等作为自己的个人博客。然而，这些博客无法随心所欲的
自定制博客的内容，而只能利用系统提供的模板进行博客的创作。为了自定制博客，需要购买
或者寻找一个免费的空间，存放自己的代码，包括html,css等。作为一个屌丝程序员，自然是
不会选择使用收费空间了，毕竟仅仅为了创建一个个人博客而购买一个空间还是有些“费用高昂”的。
然而，现在可用的免费空间越来越少，至少笔者没有找到比较好用的免费空间，如果哪位亲有好
的免费空间推荐，请给笔者留言，供大家分享。
</p>
<p>
<em>
幸运地是，github为我们提供了构建免费博客的空间，虽然空间大小有限制（几百M），但是对于
我们搭建个人博客，完全足够了。本文，笔者将向大家讲述如何利用github提供的空间搭建免费的
个人博客。
</p>
<H3>2.什么是GoAgent</h3>
<p>
简单地讲，GoAgent是部署在谷歌AppEngine平台上的一款网络代理。通过GoAgent，在你的浏览器和待访问的网页之间构建一个可以通过GWF的桥梁。
</p>
<p>
首先，我们对比正常的网络访问和GoAgent的网络访问流程来说明GoAgent的工作原理。由于本人并没有对具体的工作流程进行过深入的研究，以下的对比只是本人对其工作流程的简单的理解。如有错误之处，欢迎大家批评指正。
</p>
<h4>正常的网页访问流程</h4>
<ol>
<li>用户打开浏览器，并输入带访问内容的网址</li>
<li>浏览器向该网址发送访问请求</li>
<li>待访问网站响应浏览器请求，并返回访问内容</li>
<li>用户浏览器向用户呈现要浏览的内容</li>
</ol>

<h4>透过GoAgent的网页访问流程</h4>
<ol>
<li>用户打开浏览器，并输入带访问内容的网址</li>
<li>浏览器向该网址发送访问请求</li>
<li>GoAgent截获请求，加密请求后发送到部署在AppEngine的代理</li>
<li>代理想网址发送访问请求</li>
<li>待访问网站响应代理请求，并返回访问内容</li>
<li>代理返回内容到GoAgent并返回给浏览器</li>
<li>用户浏览器向用户呈现要浏览的内容</li>
</ol>

<p>
对比上述两个流程，由于中间加了一个代理人（GoAgent），并且数据传输是通过HTTPS协议加密传输，所以GWF无法解析其中的内容，从而避免了被GWF过滤的命运。
</p>

<h3>3.安装GoAgent</h3>
要安装GoAgent，首先要注册一个<a href="https://appengine.google.com/">Google App Engine账户</a>
<h4>注册AppEngine账户并创建应用</h4>
<ol>
<li>如果没有Gmail帐号，请先注册一个Gmail帐号，否则转到步骤2.  Gmail帐号注册地址:<a href="http://gmail.com">Gmail</a></li>
<li>登录<a href="https://appengine.google.com">AppEngine网址</a>并使用Gmail账户登录</li>
<p>
<img src="http://7te8gz.com1.z0.glb.clouddn.com/goagent-login.jpg" alt="App Engine登录页面"/>
</p>
<li>点击‘Create an Applicaton按钮’创建一个新的应用，并转入帐号确认页面。</li>
<p>
<img src="http://7te8gz.com1.z0.glb.clouddn.com/create-app.jpg" alt="点击create an Application"/>
</p>
<li>在确认页面输入你的手机号码，格式如下： +86 13888888888, 前面的+86表明中国国家码，请正确填写</li>
<p>
<img src="http://7te8gz.com1.z0.glb.clouddn.com/mobile-verify.jpg" alt="输入手机号码"/>
</p>
<li>点击‘Send’按钮，并等待接收短信。将短信中的代码输入接下来页面的表单</li>
<p>
<img src="http://7te8gz.com1.z0.glb.clouddn.com/mobile-code.jpg" alt="输入短信验证码"/>
</p>
<li>帐号激活后，转入‘My Application’页面，并单击‘Create an Application’按钮创建应用。（注意，一个Gmail账户最多可以创建10个应用，每个应用每天有1G的带宽）</li>
<p>
<img src="http://7te8gz.com1.z0.glb.clouddn.com/create-app2.jpg" alt="创建新应用"/>
</p>
<li>填写应用的信息，可以随便填写，不懂的地方就保持默认</li>
<p>
<img src="http://7te8gz.com1.z0.glb.clouddn.com/create-app3.jpg" alt="填写应用信息"/>
</p>
<li>重复步骤6,创建多个应用</li>
</ol>
<h4>下载并上传应用
<ol>
<li>转到<a href="https://github.com/goagent/goagent">下载地址</a>下载GoAgent并解压。</li>
<li>上传应用</li>
<ul>
<li>windows用户：双击server文件夹下的upload.bat，输入创建的appid和你的Gmail账户密码，并上传</li>
<p>
<img src="http://7te8gz.com1.z0.glb.clouddn.com/app-upload.jpg" alt="填入APP ID"/>
</p>
<li>Linux/Mac用户:在server目录下执行‘python uploader.zip’</li>
</ul>
<li>上传成功后，把local\proxy.ini中的appid=goagent的goagent改成你已经上传的应用的appid(多个appid之间用|隔开）</li>
<li>运行客户端，windows下需要管理员权限运行已正确导入证书</li>
</ol>
<h3>浏览器代理设置</h3>
本节主要以Chrome为例，其他浏览器类似
<ol>
<li>使用管理员权限运行goagent.exe会自动向系统导入chrome证书，也可以双击local文件夹中的CA.crt安装证书。
<p>
<img src="http://7te8gz.com1.z0.glb.clouddn.com/cert1.jpg" alt="证书导入"/><br/>

<img src="http://7te8gz.com1.z0.glb.clouddn.com/cert2.jpg" alt="证书导入"/>
</p>
<li>在Chrome的应用商店搜索Proxy SwitchySharp扩展并安装</li>
<li>单击选项，导入/导出,选择从文件恢复，并选择local目录下的SwitchyOptions.bak文件
<p>
<a href="http://7te8gz.com1.z0.glb.clouddn.com/import.png">
<img src="http://7te8gz.com1.z0.glb.clouddn.com/import-small.png" alt="备份恢复"/>
</a>
</p>
</ol>

<p>
至此，安装完成。浏览Facebook等网页时，点击SwitchySharp图标并选择相应的模式进行浏览。
</p>

<p>
<em>参考地址：https://github.com/goagent/goagent/blob/wiki/InstallGuide.md</em>
</p>