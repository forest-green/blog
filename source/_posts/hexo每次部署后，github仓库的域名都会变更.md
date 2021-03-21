---
title: hexo每次部署后，github仓库的域名都会变更
date: 2020-09-23 14:28:36
tags: hexo
---
## 解决方案

之所以每次推送都会变更是因为我们没有配置CNAME文件，所以每次推送后会重置域名，进行三步操作
1. 在source根目录创建CNAME文件，文件内写自己的域名或者ip地址
2. hexo g重新生成静态文件
3. hexo clean && hexo deploy部署之github
就可以正常访问了

```
保留问题：
CNAME的作用原理是什么？
```

