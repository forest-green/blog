---
title: hexo部署之github page报错
date: 2020-09-23 13:51:17
tags: hexo
---

根据hexo[官网文档]('https://hexo.io/zh-cn/docs/github-pages') 配置_config.yml文档，配置deploy，配置内容如下

```
type: git
    repo: github地址
    branch: gh-pages
```

在执行

```
hexo clean && hexo deploy
```

一直报错，报错信息如下

![异常信息1]https://image-static.segmentfault.com/197/161/1971616242-5f6afa1f4e3a2_articlex)

![异常信息2](https://image-static.segmentfault.com/338/098/3380985914-5f6afa2ea4dd7_articlex)

之前关注的点是

```
ExperimentalWarning: The fs.promises API is experimental
```

这个异常信息提示，是node版本过低有一些组件不支持最新版的hexo（5.0.0），但是无伤大雅；

经过排查，发现是deploy配置项的缩进错误，然后调整这部分代码为

```
type: git
repo: githu地址
branch: gh-pages
```

这里特别注意 前面的空格没了，此时能够正常执行

```
hexo clean && hexo deploy
```

如果只是这样配置，那么会出现一个问题，就是hexo d 正常执行了，但是github page没有变化，我们执行的时候也没有报错，只能在本地服务器看到正常的页面，到此处我们需要完善我们的配置信息，配置为：

```
deploy:
 type: git 
 repo: 
  github: github page地址 
 branch: gh-pages 
```

完善之后，再执行hexo d就可以正常看到我们所推送的页面信息了

