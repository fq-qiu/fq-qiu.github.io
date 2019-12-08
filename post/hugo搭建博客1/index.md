 
我之前的静态博客生成器用过jekyll(基于ruby), pelican(基于python)和hexo(基于node.js)，到现在用hugo(基于go语言)。因为hugo有至少3个优点
- 1 生成blog速度最快，因为用的go语言
- 2 安转快捷，不需要安转依赖包，各个平台都有可执行程序下载。尤其是hexo基于node.js，需要npm一堆依赖包，很麻烦
- 3 有很多漂亮的主题。而pelican的主题不够美观，我是个颜值党
 
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

## hugo-theme-even主题自定义

- 替换favicon.ico. 目录在`even/static/`
- 替换自己的微信和支付宝二维码. 目录在`even/stati/img/reward/`.
- 在`blog/config.toml`中修改路径为`/img/reward/alipay.png`. `/img`表示在网站的根目录下的img目录下
 
 
## 把博客地址加入搜索引擎
为了使博客被更多的人看到，需要把博客加入搜索引擎。Hugo默认生成了博客站点图`sitemap.xml`, 需要搜索引擎索引它。Google站长工具加入后，不久就能所引到，而Baidu需要较长的时间，一般半个月起，其他的bing和搜狗我没有添加。
我的百度站长和Google站长，获取确认码后，填写到`config.toml`中，默认使用hugo-theme-even主题
```
baidu_push = true        # baidu push                  # 百度
baidu_analytics = ""      # Baidu Analytics
baidu_verification = "s8Pe1TBqyy"   # Baidu Verification
google_verification = "fNO906OBok1KPg3j_d5IDaHKN-M1MFEldzqW53hQ98E"  # Google_Verification         # 谷歌
```
 
### 添加sitemap到google
 
进入[google webmaster searchconsole](https://www.google.com/webmasters/tools/sitemap-list?hl=zh-CN&siteUrl=https://fq-qiu.github.io/#MAIN_TAB=0&CARD_TAB=-1)
`search console` -> `抓取` -> `站点地图` -> `添加站点地图`, 即`sitemap.xml`
 
### 添加sitemap到百度
 
在[baidu站长](http://zhanzhang.baidu.com/site/siteadd)添加网站, 把meta标签拷贝上面步骤说明的文件, 然后deploy网站.
 
验证成功后, 在`网页抓取` -> `链接提交` -> `自动提交` -> `sitemap`
填写
```
qfq.coding.me/sitemap.xml
```
### 添加sitemap到bing
 
[bing webmaster](https://www.bing.com/toolbox/webmaster/)
 
 
## hugo博客todo
- ~~1 更改配套色彩网站图标(gimp和windows拾色器)~~
- 2 参照hexo主题even添加搜索框, [功能实现](https://gist.github.com/eddiewebb/735feb48f50f0ddd65ae5606a1cb41ae)
- 3 参照`themes/even/layout/_default/404.html`添加404.html
- 4 在页面添加回到顶部的图标按钮
- 5 使用脚本把博客同时发布到CSDN, segmenfault和我的[wiznote](http://www.wiz.cn/wiz-mywiz.html)
- 6  发布的笔记怎么在一个地方修改后，同步到其他地方
- 7 使用百度统计统计博客访问量
 
 
## 使用hugu遇到的问题
### hugo没有自动发布功能
默认执行`hugo`后生成静态网站到`./public`。我写了一个sh脚本，拷贝`./public`到`~/qfq.coding.me`，然后在`~/qfq.coding.me`目录下push到提供pages的网站即可
 
### hugo的设置`publicDir`不起作用
我在`config.toml`中添加`publicDir="~/qfq.coding.me"`不起作用，还是默认生成到`./public`
 
### 关于hugo的blog文章起始配置
- `default: false`发布到网站，否则只能在本地测试服务器上浏览到
- `hugo new post/test.md`怎么添加`lastmod:`到文件起始位置
## 其他
### hugo好看的themes
[hugo even](https://github.com/olOwOlo/hugo-theme-even)
[hugo-academic](https://github.com/gcushen/hugo-academic)
 
 
### 一些博客toc的实现方案
- 1 http://x-wei.github.io/pelican_github_blog.html#
- 2 http://blog.liuxianan.com/download-apk-from-google-play.html
- 3 http://devlu.me/2016/07/29/open-android-debug-log-in-device/
- 4 http://blog.coderclock.com/
