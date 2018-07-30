---
title: "Hugo搭建博客<二>:使用Travis.ci自动部署到github pages和coding.net pages"
date: 2018-07-29T16:05:50+08:00
lastmod: 2018-07-30 08:01:35
tags: ["hugo"]
draft: false
categories: ["blog"]
author: ""

---
## 写在前面
使用Hugo生成静态博客后，需要手动把内容长传到对应pages的仓库，这种操作全程手动。现在有[Travis.ci]( https://www.travis-ci.org/ )，一个自动测试和部署工具网站，你只需要把hugo处理前的文件上传到github，Travis.ci就会自动通过Hugo生成静态博客内容，并上传到pages的对应仓库。
需求有3个
- 1 保存博客内容的github上仓库hugo-blog
- 2 用户对应的github-pages的对应仓库 user.github.io，我的是tainzhi.github.io
- 3 Travis.ci上设置的hugo-blog与tainzhi.github.io的对应trigger

具体流程如下：
- 1 本地编写博客，并上传到github仓库hugo-blog
- 2 因为Travis.ci对hugo-blog设置了trigger，它检测到hugo-blog有推送，就执行一系列操作，生成静态博客内容，把内容推送到github上pages对应的仓库tainzhi.github.io
- 3 在浏览器中输入`https://tainzhi.github.io`就可浏览到新博客

## Travis.ci注册与使用
[Travis.ci]( https://www.travis-ci.org/ )用github账号注册。注册后，会看到自己github上的仓库列表，选择hugo-blog并打开。在[github token](https://github.com/settings/tokens)网站生成tokens，复制到刚才Travis.ci上hugo-blog对应的设置中的环境变量，命名为`GithubToken`。同理，在[coding.net](https://coding.net/user/account/setting/tokens)上生成token拷贝到环境变量，命名为`CodingNetToken`


## 自动部署到github pages
在hugo-blog中需要添加一个配置文件`.travis.yml`, 配置内容如下
```
# https://docs.travis-ci.com/user/deployment/pages/
# https://docs.travis-ci.com/user/languages/python/

install:
    - uname -a
    - wget https://github.com/gohugoio/hugo/releases/download/v0.45.1/hugo_0.45.1_Linux-64bit.deb
    - sudo dpkg -i hugo*.deb
    - hugo version
    - ls
    - pwd

script:
    - hugo
    - echo 'Hugo build done!'
after_script:
  - git clone "https://tainzhi:${GithubNetToken}@${GithubNet_REF}" github-pages
  - rm github-pages/public -rf
  - cp ./public/* github-pages -rf
  - cd github-pages
  - git add .
  - git commit -m "Update Blog By TravisCI With Build $TRAVIS_BUILD_NUMBER"
  - git tag v0.0.$TRAVIS_BUILD_NUMBER -a -m "Auto Taged By TravisCI With Build $TRAVIS_BUILD_NUMBER"
  # Github Pages
  - git push --force --quiet "https://tainzhi:${GithubToken}@${Github_REF}" master:master 
  - git push --quiet "https://tainzhi:${GithubToken}@${Github_REF}" master:master --tags

env:
 global:
   # Github Pages
   - Github_REF: github.com/tainzhi/tainzhi.github.io.git
```
配置全是一些常见操作，很容易理解：先安转hugo，然后在当前目录下用hugo生成静态博客内容到`public`默认目录，接着clone仓库tainzhi.github.io，把public里面的内容拷贝到刚下载的仓库，最后上传并打上tag。
**注意token的用法**: 用token操作仓库`git clone https://username:${Token}@${repo_ref}`
## 部署到coding.net pages
```
# 在after_script中添加
  # Coding Pages
  - git push --force --quiet "https://qfq:${CodingNetToken}@${CodingNet_REF}" master:master
  - git push --quiet "https://qfq:${CodingNetToken}@${CodingNet_REF}" master:master --tags
# 在env中添加
   # Coding Pages
   - CodingNet_REF: git.coding.net/qfq/qfq.coding.me.git
```

最终内容请参考我的[博客配置](https://github.com/tainzhi/blog-hugo/blob/master/.travis.yml)
