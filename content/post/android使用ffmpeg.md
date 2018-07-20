---
title: "Android使用ffmpeg"
date: 2018-07-20T13:03:45+08:00
draft: false
tags: ["android", "ffmpeg", "多媒体"]
categories: ["android"]

---
# ffmpeg for android shared library

This comes from my [github README](https://github.com/tainzhi/ffmpeg-for-android-shared-library)

  移植ffmpeg到android，编译可用于jni调用的so库.<br>
  编译出的so在android apk中的使用参考我的另一个项目[ffmpeg-jni-sample](https://github.com/tainzhi/ffmpeg-jni-example)

## 环境
  ubuntu ubuntu15.10_64<br>
  ffmpeg 2.6.2<br>

## 获取代码
``` 
git clone https://github.com/tainzhi/ffmpeg-for-android-shared-library
```

## 使用
###Step 1
安装android linux SDK以及NDK，并配置环境变量；

我的是通过Android SDK manager下载， 默认安装在`~/Android/Sdk/ndk-build`<br>

从[ffmpeg官网](http://ffmpeg.org/)下载ffmpeg源码包;也可以直接使用我本项目中的ffmpeg源码，我所使用的是2.6.2版本<br>
如果要使用自己下载的ffmpeg源码，需要先将source/ffmpeg下的所有内容删除，然后将自己所下载的源码包解压到ffmpeg目录下<br>

###Step 2

本项目提供了分别编译arm平台库和x86库和arm64平台的sh文件，分别为
- `source/build_android_arm.sh` 
- `source/build_android_x86.sh`
- `source/build_android_aarch64.sh`

下面以build_android_arm.sh为例进行说明：<br>
将`source/build_android_arm.sh`复制到`ffmpeg`目录下
#####1.指定临时目录
```
export TMPDIR=/tmp
```
指定一个临时目录，可以是任何路径，但必须保证存在，ffmpeg编译要用；<br>
#####2.指定NDK路径
```
NDK=~/Android/Sdk/ndk-build
```
#####3.指定使用NDK Platform版本
```
SYSROOT=$NDK/platforms/android-21/arch-arm/
```
这里指定的ndk platform的路径，一定要`选择比你的目标机器使用的版本低的`，比如你的手机是android-21版本，那么就选择低于21的<br>
#####4.指定编译工具链
```
TOOLCHAIN=$NDK/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64
```
#####5.指定编译后的安装目录
```
CPU=arm
PREFIX=$(pwd)/android/$CPU
```
显然，生成的文件在`source/ffmpeg/android/arm/`目录下

这个目录是ffmpeg编译后的so的输出目录，会有一个include和lib文件夹生成在这里，这也是我们之后要在android apk中使用的.<br>

- `source/ffmpeg/android/arm/lib/`目录下是动态库文件.so

- `source/ffmpeg/android/arm/include/`目录下的是头文件，不仅需要动态库，还需要头文件

#####build_android_arm.sh参数配置

`--enable-shared`和`--disable-static`生成动态库

`--cross-prefix=$TOOLCHAIN/bin/arm-linux-androideabi-`是一些跨平台变异所需要的文件，不同的平台是不一样的

`--target-os=android`指定适配android平台，我之前fork的原库是linux，如果是linux，那么生成的库名中有版本号，还需要重命名指定android后就不需要了

`make -j8`多线程加速编译


具体查看ffmpeg文档.

###Step 3
```
cp source/build_android_arm.sh source/ffmpeg/
cd source/ffmpeg
./build_andrioid_arm.sh
```

###Step 4编译出现错误
如果编译过程中出现错误，错误信息会输出在`source/ffmpeg/config.log`文件中，一般在文件末尾.仔细分析该文件，可以找到编译出错的原因

###Step 5
等待编译完成后,在`source/android/arm/`目录下分别有动态库`lib`和头文件`include`.


###Step6 重新configure&&compile
修改了`build_android_arm.sh`文件，发现参数没有起作用，原来没有清除之前configure生成的文件.这个命令，值得拥有
```
make distclean      #delete files created by configure
#then
. ./build_android_arm.sh
```

## Reference & Thanks
- [ffmpeg-for-android](https://github.com/dxjia/ffmpeg-for-android-shared-library)

