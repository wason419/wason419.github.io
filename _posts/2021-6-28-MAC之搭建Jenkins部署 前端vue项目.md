---
layout: post
title: MAC之搭建Jenkins部署 前端vue项目  
subtitle: 笔记
date: 2021-06-28
author: Wason
header-img: img/bg/post-bg-20.JPG
catalog: true
tags:
  - Jenkins
---

# MAC之搭建Jenkins部署 前端vue项目 #

系统：Mac  
组合：Jenkins + GitHub + NodeJS  
版本：我使用的nodejs版本是  

![](http://wason419.github.io/img/20210628/2021062801.png)  

注：npm还是不要用6.5.0版本的（包括6.5.next.0，虽然说是这两个没什么区别），电脑的npm一开始是安装了最新版本6.5.0的，但总是一直报you must update following to modules: npm: 6.5.0-next.0 should be >= 3.0.0问题，一开始以为Jenkins是直接使用自己电脑的node，所以一直在更新自己电脑的node/npm版本，但总是会报错。折腾大半天后，问了技术群，有人说是缓存问题，建议全局处理。后面就通过清缓存、重启电脑、选了10.15版本重装node，终于定位到了问题，如下图，Jenkins里选择的NodeJS版本原来也是有关系的，这里需要选跟自己电脑一样的，就可以了  

![](http://wason419.github.io/img/20210628/2021062802.png)
![](http://wason419.github.io/img/20210628/2021062803.png)

## 一. 搭建并安装插件 ##
可以参考该博文 [Jenkins安装 for Mac][1]，先下载搭建Jenkins.  
其中，如下图，每次Retry点击，都会有些插件因下载失败而报红；但是多点击几次，会发现报红的插件会越来越少的。另外，不确定用不用翻墙下载，反正我是翻墙下的。

![](http://wason419.github.io/img/20210628/2021062804.png)

同时，也不确定哪些插件是必须的，反正就在这一步先把插件下载好，缺的后面再补。要补充插件的，如下图，可以到 `[ 系统管理 ]` → `[ 插件管理 ]` → `Available` 中下载

![](http://wason419.github.io/img/20210628/2021062805.png)

当然，有些插件总是出现下载失败的情况，这里可以自己下载，再本地上传,网址如下：      
[jenkins安装publish over ssh插件][2]  
[插件下载的网址][3]

![](http://wason419.github.io/img/20210628/2021062806.png)

至于到底需要哪些插件，也不是很清楚，网上整理的大概如下  
```
jenkins的常用插件：
① Git plugin
② Git client plugin
③ Subversion Plug-in
④ Subversion Release Manager plugin
⑤ Subversion Tagging Plugin
⑥ SVN Publisher plugin
⑦ SSH Credentials Plugin
⑧ Gradle plugin: android专用
⑨ Xcode integration: iOS专用

```
而我明确有印象专门去下载插件就是 `SSH Slaves plugin` 、`NodeJS Plugin`、`GitHub Plugin`、`Git plugin`，在搭建过程中，到处找文章找方法，比较混乱，也可能还有其他的插件有所遗忘哈


## 二. Jenkins的运行流程大概是 ##
1. 本地搭建Jenkins  
2. Jenkins连接Git,从Git上下拉代码，到Jenkins本地的workspace  
3. workspace里编译/打包  
4. scp命令 Jenkins的workspace里的压缩包到目标服务器的指定位置  
5. 连接目标服务器  
6. 到指定位置，删除原项目文件，解压压缩包   

注：由于本地Jenkins服务器位置跟目标服务器是两个不同地方，需要做两个之间的.ssh/id_rsa.pub信任，这样才能scp文件到目标服务器。因此，Jenkins跟目标服务器是同一个服务器下的话，就可省去信任配置环节，搭建使用起来少好多麻烦  

## 三. 自动化部署项目 ##
[使用Jenkins自动化部署Vue.js项目——兼容Vue CLI3生成的项目][4]  
参考上文，新建项目  

![](http://wason419.github.io/img/20210628/2021062808.png)

`其中，最关键的是project的配置(Configure)上，如下图`

![](http://wason419.github.io/img/20210628/2021062809.png)
![](http://wason419.github.io/img/20210628/2021062810.png)
![](http://wason419.github.io/img/20210628/2021062811.png)
![](http://wason419.github.io/img/20210628/2021062812.png)
![](http://wason419.github.io/img/20210628/2021062813.png)
![](http://wason419.github.io/img/20210628/2021062814.png)
![](http://wason419.github.io/img/20210628/2021062815.png)

在 Build 里面做命令配置即可，另外我使用了两个shell，因为网上有说法是，像npm install操作时间比较久的，就要分开shell配置，故我便放到两个shell里，并按执行次序做排列。

上图中的命令内容如下：  
```
npm install
npm run build
cd dist
zip -r blue-print-web.zip *
scp blue-print-web.zip root@xxxx:/opt/xxxxx/webapps

cd /opt/xxxxx/webapps
rm -rf xxxxx
unzip -o blue-print-web.zip -d xxxxx/
rm -rf blue-print-web.zip

```

若是服务器上搭建的，可以考虑在npm install 前加npm版本显示并安装npm，命令配置如下，参考该文：[Jenkins一键部署vue项目][5] 
```
echo $PATH
node -v
npm -v #检查编译环境
npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
npm install
npm run build #编译项目
cd dist
tar -zcvf dist.tar.gz *
scp /root/.jenkins/workspace/xxx/dist/dist.tar.gz /usr/local/xxx/dist  #把包发送到指定服务器目录
cd /usr/local/xxx/dist && tar -zxvf dist.tar.gz
cd /root/.jenkins/workspace/xxx
rm -R dist
```

配置好后，save保存后，就可进行构建工作了

---

## 四. 问题 ##
1. mac系统里，执行shell的命令，会去读写Jenkins里的文件,这操作会遇到用户权限问题，可直接更改Jenkins的workspace文件夹的读写操作权限来解决  
![](http://wason419.github.io/img/20210628/2021062816.png)
![](http://wason419.github.io/img/20210628/2021062817.png)

2. 执行shell中的 scp x1.zip root@xxxxxxx:/opt/…/webapps 命令，用于上传文件到目标服务器时，报错： host key verification failed.  
参考该文解决方案 [Jenkins系列_使用scp命令进行远程文件复制遇到的坑][6]，采用的是配置免密登录的方式，处理方式参考下文，   
[Linux配置SSH公钥认证与Jenkins远程登录进行自动发布][7] , 方法采用的是C方法

![](http://wason419.github.io/img/20210628/2021062818.png)
![](http://wason419.github.io/img/20210628/2021062819.png)

不过参照上文，创建的私钥公钥都是别的文件里，且在别的文件夹里是无效的，故我又把生成的3个文件复制到了Jenkins的ssh文件下，一定要3个都复制过来！！  

![](http://wason419.github.io/img/20210628/2021062820.png)

且还得在Jenkins的 `[系统管理]` → `[系统设置]` → `Publish over SSH` 里配置  

![](http://wason419.github.io/img/20210628/2021062821.png)

3. 报错：[Error: Cannot find module 'chalk'][8]  
也是报上面错误，原因是我们前端Vue项目上传时，在GIT上是不会上传node_model文件的，Jenkins下拉代码后，执行npm run build便会报错，只要执行下npm install，安装完资源后，再打包即可。


[1]: https://www.jianshu.com/p/48d3d8293376
[2]: https://blog.csdn.net/ludengji/article/details/79174216
[3]: https://updates.jenkins-ci.org/download/plugins/  
[4]: https://blog.csdn.net/wangzl1163/article/details/83018630
[5]: https://blog.csdn.net/qq_34479912/article/details/82417869
[6]: https://blog.csdn.net/kingboyworld/article/details/78905553
[7]: https://www.cnblogs.com/jager/p/5986563.html
[8]: https://www.cnblogs.com/yu-709213564/p/7469293.html