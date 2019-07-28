

# 简单谈谈vim使用过程的坑和爽点
vim使用有几年了，从最开始不会使用各种懵逼，到输入第一个字符，再到能简单的插入删除复制粘贴，再到自定义一些简单的配置，再到使用插件和插件管理器，再到配置成一个ide，再到能写一些简单的脚本（再到能写简单的插件，需要有vim脚本和python脚本的基础，目前我还不会写）：中间经历了无以言说的喜悦，也同样经历了各种踩不完的坑被打击的痛苦到谷底。。。期间滋味就像加入了一个教， “vim教”，虐我千百遍，我依然爱她如初恋，早已忘记了当初为什么爱上了ta，ta已经融入了我的“code血夜”中，没有它，我几乎不会code了

## 无处不在的vim
- 类linux系统都会自带vim，尤其是服务器
- [Intellij全家桶](https://www.jetbrains.com/)有插件[ideavim]([https://plugins.jetbrains.com/plugin/164-ideavim](https://plugins.jetbrains.com/plugin/164-ideavim))，当然也包括Android Studio
- chrome有插件[cVim]([https://chrome.google.com/webstore/detail/cvim/ihlenndgcmojhcghmfjfneahoeklbjjh?hl=en](https://chrome.google.com/webstore/detail/cvim/ihlenndgcmojhcghmfjfneahoeklbjjh?hl=en))
- Visual Studio也有vim插件(我还没用过Visual Studio)
- 知乎支持简单的vim浏览模式
- vim配置成ide, 用来看java+cpp的源码还是很舒服的

有了vim，我几乎不再使用鼠标（假的，吹牛逼，鼠标还是要的），两只手完全放在键盘上就可以几乎完成所有的操作，编辑代码更是如德芙巧克力般丝滑（doge）。具体怎么个丝滑法，需要自己体会，若想交流，可以给我发邮件

以上是表象，是vim党的吹逼，下面是坑，高能预警

## 使用vim过程中的坑
注意是我这个vim使用过程中遇到的坑，可能我很菜，所以老是遇到坑

- vim完全定制化，一切尽在掌握中，也意味着对新手非常的不友好，非常的不友好，若想入vim教，请抱着“虔诚”的态度（夸张了，我就是说说）
- vim学习曲线非常的陡峭，不像其他的编辑器和ide几乎不用学习就可以输入编码，简单熟悉就可以使用常用的快捷键进行开发
- vim的高度定制化需要加载很多插件
- vim还是比较小众，他的一些痛点功能只能依靠少数的几个大神看心情写插件，当然如果你牛逼，你完全可以自己写个插件开源出来
- vim插件太多，加载启动速度就很慢，从无插件秒启动到很多插件后启动时间要1s
- vim配置完成度比较高后，作为c/cpp的ide还是非常强大的，对java和android开发就完全被Intelli和android studio秒杀(此处吐槽Intellij的自动补全, 是被vim+youcompleteme秒杀的), vim也可以作为python, go, typescript, rust的ide, 功能也比不上相应的ide
- vim功能强大后, 除了vim自己的快捷键,还有各种插件的快捷键,快捷键多了后,需要解决快捷键冲突
- vim每一个自定义功能背后, 都以为要花不少时间看博客和采坑


## 怎么配置成ide
说了这么多, 还是推荐下一篇写的很不错的博客, [use vim as ide](https://github.com/yangyangwithgnu/use_vim_as_ide), 它使用插件vundle, 建议替换成[vim-plug]([https://github.com/junegunn/vim-plug](https://github.com/junegunn/vim-plug))

## 欢迎入“vim教”
说到底, vim只是一个工具, 就像ide一样, 工具是为目的服务的, 也就是编码服务的. 我们唯一的目的就是编好码, 编完码后若有精力时间和兴趣, 研究一些还是不错的.
