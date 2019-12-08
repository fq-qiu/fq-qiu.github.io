## android library
android library从结构上来看和module没有区别, 都含有源码, 资源文件和android manifest文件. 不过前者被编译成apk运行在设备上, 后者被编译成aar(Android Archive)文件, 作为其他app模块的依赖存在.

不像java的包文件jar, jar只包含源码.

aar和jar都可以作为app module的依赖包

## 创建library或者jar
从android studio进入, File > New > New Module > Android Library/Java Library

### 添加library/jar到项目依赖中
File > New > New module选择要导入的library/jar

### 在项目根目录的手动添加
在项目的根目录中`settings.gradle`. 
```
include ':app', ':my-library-module'
```

### 在使用library的module的`build.gradle`添加
```
dependencies {
    implementation project(":my-library-module")
}
```

## convert an app module to an library module
- 在项目的`build.gradle`中删除`applicationId`, 因为只有app module才有applicationId
- 把build.gradle中的`apply plugin: 'com.android.application'`修改成`apply plugin: 'com.android.library'`
- 保存文件, 然后File > Sync Project with Gradle Files

## 组件化过程中遇到, 黄油刀butterknife在Library上的使用（元素值必须为常量表达式）
原因是library中注解生成的R文件没有final表示, 导致无法识别.
解决办法是把使用R2

### 1 在project gradle中添加
```
dependencies {
    classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
    classpath 'com.jakewharton:butterknife-gradle-plugin:8.6.0'
}
```

### 2 在需要使用R2的module的gradle中添加
```
apply plugin: 'com.android.library'
apply plugin: 'android-apt'
apply plugin: 'com.jakewharton.butterknife'
```

### 3 把整个项目中的`BindView(R.id)`替换成`BindView(R2.id)`

## 批量解决ButterKnife组件化中R类问题
使用反射, 详见参考

## 可能遇到的问题

### cannot access xxx.class
原因是没有指定某个library的依赖, 解决办法是
```
dependencies {
    implementation project(":my-library-module")
}
```

## 总结
组件化开发中, ButterKnife使用R2.当然AndroidStudio也有相应的插件自动生成

## 参考
- [android developers: create an android library](https://developer.android.com/studio/projects/android-library)
- [butterknife组件化开发l使用ibrary中R类问题的批量解决方案](https://www.jianshu.com/p/a101cec2c960)
- [Android组件化aar躺坑记：ButterKnife 报 元素值必须为常量表达式错误](https://www.cnblogs.com/slma/p/9359375.html)

