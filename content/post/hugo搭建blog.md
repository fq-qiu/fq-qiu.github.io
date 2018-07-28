---
title: "Hugo搭建blog"
date: 2018-07-20T12:10:44+08:00
lastmod: 2018-07-28 14:33:35
tags: ["hugo"]
draft: false
categories: ["blog"]
author: ""

---
## Hugo quick start
 
下载[最新的hugo](https://github.com/gohugoio/hugo/releases)并安装
```
#for ubuntu
sudo dpkg -i hexo-version-xxx.deb
```
生成站点
```
hugo new site blog-hugo
```
下载[hugo-theme-even](https://github.com/olOwOlo/hugo-theme-even)
```
cd blog-hugo
git submodule add https://github.com/olOwOlo/hugo-theme-even themes/even
```
替换`blog-hugo/config.toml`为`hugo/themes/even/exampleSite/config.toml`，然后生成主题
```
hugo -t even
```
创建测试blog到`hugo-blog/content/post`目录下
```
hugo new post/first-post.md
```
本地部署打开服务器测试
```
hugo server -D
```
默认地址为`http://localhost:1313`，使用浏览器访问
 

### hugo博客todo
更改配套色彩网站图标(gimp和windows拾色器)
参照[tainzhi](https://tainzhi.github.io/)添加搜索框

### 内容展示
 
 
##放弃使用hexo的原因
需要安转node-js，而且生成blog速度没有hugo快；hugo是编译语言，速度快。
[hexo 2 hugo](http://nodejh.com/post/Migrate-to-Hugo-from-Hexo/)
##原来使用的hexo博客地址，发布在coding.net
[hexo @ coding.net](https://coding.net/u/qfq/p/blog-hexo/git)
[qfq.coding.me](https://coding.net/u/qfq/p/qfq.coding.me)
##博客使用hugo even主题
[hugo even](https://github.com/olOwOlo/hugo-theme-even)
### 博客访问量统计
[友盟](https://web.umeng.com/main.php?c=site&a=getcode&siteid=1274212813)
[google analytic](https://analytics.google.com/analytics/web/?authuser=0#/a122485224w180581455p178705584/admin/tracking/tracking-code/)
 
### 添加评论系统
[gitment](https://github.com/imsun/gitment)
#### gif
 
#### 视频
[asciinema](https://asciinema.org/)
 
### 七牛CDN加速
[使用七牛CDN加速](https://blahgeek.com/qiniu-cdn-serve-static/)
[hexo plugins](https://hexo.io/plugins/)搜索CND
 
### 博客优化
[SEO优化](https://blog.csdn.net/sunshine940326/article/details/70936988)
### 博客内搜索
[1 lucenen](http://blog.csdn.net/wuyinggui10000/article/details/45602341)
 
[2 python django实现](http://www.phantask.net/blog/13)
 
3 Swiftype注册后, 实现博客内搜索
 
 
### hugo好看的themes
[hugo-academic](https://github.com/gcushen/hugo-academic)
 
 
### 一些博客toc的实现方案
http://x-wei.github.io/pelican_github_blog.html#
http://blog.liuxianan.com/download-apk-from-google-play.html
http://devlu.me/2016/07/29/open-android-debug-log-in-device/
http://blog.coderclock.com/

