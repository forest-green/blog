---
title: photo
date: 2021-02-20 15:36:43
tags: hexo
---

# HEXO 插入图片

此帖子非原创，属于查到此帖子，留下记录，以便随时查看

参考链接：https://www.jianshu.com/p/f72aaad7b852

下方为原文：

## 第一步，安装插件，在hexo根目录大概Git bash执行

```
npm install hexo-asset-image --save
```

## 第二步，调整配置文件_config.yml

找到post_asset_folder，把此选项从false更改为true

## 第三步，更改第一步安装的静态资源

找到此文件

```
/node_modules/hexo-asset-image/index.js
```

将此文件的内容，整体更换为下方的代码

（在此感谢Ericam_ 大神：[https://blog.csdn.net/xjm850552586](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fxjm850552586)）

```js
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
        var link = data.permalink;
    if(version.length > 0 && Number(version[0]) == 3)
       var beginPos = getPosition(link, '/', 1) + 1;
    else
       var beginPos = getPosition(link, '/', 3) + 1;
    // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
    var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];
 
      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
        if ($(this).attr('src')){
            // For windows style path, we replace '\' to '/'.
            var src = $(this).attr('src').replace('\\', '/');
            if(!/http[s]*.*|\/\/.*/.test(src) &&
               !/^\s*\//.test(src)) {
              // For "about" page, the first part of "src" can't be removed.
              // In addition, to support multi-level local directory.
              var linkArray = link.split('/').filter(function(elem){
                return elem != '';
              });
              var srcArray = src.split('/').filter(function(elem){
                return elem != '' && elem != '.';
              });
              if(srcArray.length > 1)
                srcArray.shift();
              src = srcArray.join('/');
              $(this).attr('src', config.root + link + src);
              console.info&&console.info("update link as:-->"+config.root + link + src);
            }
        }else{
            console.info&&console.info("no src attr, skipped...");
            console.info&&console.info($(this));
        }
      });
      data[key] = $.html();
    }
  }
});
```

## 然后可以插入图片

我们在使用hexo new post photo后，会在source/_posts生成一个photo.md文件和photo文件，我们把要插入的文件复制到photo中

在photo.md文件中正常引入即可，(我的文件名是head.jpeg)如

```
![这是代替图片的文字，随便写](head.jpeg)
```

此处本地查看，可能会提示图片引入错误，但是你可以执行 hexo server 进行本地编译后进行查看操作