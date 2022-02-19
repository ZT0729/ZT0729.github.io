---
title: hexo目录结构
tags: hexo
abbrlink: bd36e87b
date: 2021-12-27 00:29:11
---
hexo 初始化后会在所在文件夹生成以下目录
![](https://zt0729-picture-bed.oss-cn-beijing.aliyuncs.com/beijinguTools_1640537222407.png)

### .github 文件夹
.github 文件夹：hexo是从github上拉下来的，所以有github仓库文件夹，不用管。

### node_modules 文件夹
node_modules 文件夹：用来存放用包管理工具下载安装的包的文件夹。

### scaffolds 文件夹
scaffolds 文件夹：模版文件夹，当您新建文章或页面时，Hexo 会根据 scaffold 来建立文件。

### source 文件夹
source 文件夹：资源文件夹是存放用户资源（文章页面等）的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

### themes 文件夹
themes 文件夹：主题文件夹，存储主题

### .gitignore
.gitignore：.gitignore 文件作用是声明不被 git 记录的文件，`hexo init <folder>` 也会产生一个 .gitignore 文件，可以先删除或者直接编辑，对hexo不会有影响。

### _congfig.landscape.yml
_congfig.landscape.yml：hexo 的主题配置文件

### _congfig.yml
_congfig.yml：网站的配置信息，设置包括网站标题、副标题、作者、关键字和描述信息等。

### package.json
package.json： package.json记录当前项目所依赖模块的版本信息，更新模块时锁定模块的大版本号（版本号的第一位）。在 npm 安装时使用 --save 保存进去。

### package-lock.json
package-lock.json：package-lock.json 记录了 node_modules 目录下所有模块的具体来源和版本号以及其他的信息。