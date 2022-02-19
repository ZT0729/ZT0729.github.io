---
title: hexo所遇问题
tags:
  - hexo
abbrlink: '155e3338'
date: 2021-12-19 21:14:31
---
# 如何对一篇文章增加多个标签，应该使用什么分隔符？
 ```
 tags: [aaa, bbb]  #要加这个括号！
 ```
 或
使用YAML語法。
http://zh.wikipedia.org/zh-hant/YAML
例如：
```
tags:
  - foo
  - bar
```




# Hexo 生成永久文章链接
Hexo 默认文章链接生成规则是按照年、月、日、标题来生成的。一旦文章标题或者发布时间被修改，URL 就会发生变化，之前文章地址也会变成 404，而且 URL 层级很深，不利于分享和搜索引擎收录。

如果文章标题中有中文，URL 被转码后会很长，比如：https://lujiahao0708.github.io/2020/04/11/%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9B%B8%E5%85%B3/GitHub%20Actions%20%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2%20Hexo/。 接下来介绍一个插件 hexo-abbrlink，该插件会为每篇生成一个唯一字符串，并不受文章标题和发布时间的影响，比如：https://lujiahao0708.github.io/p/df27ccfb.html。

## 1.安装
点击即可访问插件源码地址<u> [hexo-abbrlink](https://github.com/rozbo/hexo-abbrlink) </u>。
```
npm install hexo-abbrlink --save
```
可能会出现依赖，依据提示安装即可。

## 2.配置
修改博客根目录配置文件 `_config.yml` 的 `permalink`：
```
# permalink: :year/:month/:day/:title/
permalink: p/:abbrlink.html  # p 是自定义的前缀
abbrlink:
    alg: crc32   #算法： crc16(default) and crc32
    rep: hex     #进制： dec(default) and hex
```
不同算法和进制生成不同的格式：
```
crc16 & hex
https://post.zz173.com/posts/66c8.html
crc16 & dec
https://post.zz173.com/posts/65535.html

crc32 & hex
https://post.zz173.com/posts/8ddf18fb.html
crc32 & dec
https://post.zz173.com/posts/1690090958.html
```
## 3.验证
先清理下本地的文件 `hexo clean`，然后重新生成 `hexo g`，启动博客 `hexo s`。该插件会在每篇文章的开头增加内容：
```
abbrlink: df27ccfb
```
这个字符串就是这篇文章的唯一标识，无论修改标题还是发布文章都不会改变。浏览器打开 `http://localhost:4000/` 查看成果吧！

# 导航菜单跳转网址要带协议！
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/pictureQQ图片20211226230357.png)
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/pictureQQ图片20211226230410.png)
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/pictureQQ图片20211226230415.png)
如上，要添加上协议
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/pictureQQ图片20211226230449.png)