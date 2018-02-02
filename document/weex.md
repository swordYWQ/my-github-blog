# Weex入坑之旅

## 1.Weex上手

### 安装weex-toolkit脚手架
```
npm i -g weex-toolkit
```

### 创建脚手架项目
```
weex create projectname
```

### 创建安卓项目
```
weex platform add android
```

### 在安卓设备上启动项目
```
weex run android
```

### 构建安卓apk包
```
weex build android
```

## 2.开发中遇到的坑
### 2.1修改脚手架项目配置
默认的weex脚手架项目在webpack中递归为每一个vue文件生成一个实例，和vue常用的写法不一致，所以在这里修改脚手架配置，使开发和正常的vue spa项目开发流程一致。

解决方法: 修改脚手架配置
```
const webEntry = {index:path.resolve('src','entry.js?entry=true')}
const weexEntry = {index:path.resolve('src','entry.js?entry=true')}
// 1.修改默认的webEntry和weexEntry,指向一个js入口文件
// 2.然后注释或删除getEntryFile方法调用  // getEntryFile()
--webpack.common.conf.js
```
在指向的js入口文件中
```
import Vue from 'vue';
import weex from 'weex-vue-render';
import App from './App.vue'
import router from './router.js'

weex.init(Vue);
new Vue(Vue.util.extend({ el: '#root', router },App))
// 可以在此处加载路由及vue其他插件
-- entry.js
```

### 2.2安卓环境配置
weex项目进行简单开发后，希望能在安卓手机上看到效果，使用命令生成项目和包,weex run android 或者weex build android出现错误，提示未设置ANDROID_HOME环境变量

解决方法:配置环境变量
```
在计算机->属性->高级系统设置->高级->环境变量中添加环境变量(在系统变量中添加，如果无效的话用户变量和系统变量中都加)
1.添加值ANDROID_HOME value为你的AndroidSDK目录
2.找到PATH变量，在后面加上%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools;(记得分号不能漏)
修改完后运行启动命令提示符窗口，输入adb命令，提示adb的命令帮助则成功，提示找不到命令则失败
(此时可能会有在运行中启动的命令提示符输入命令访问成功，但通过shift+右键进入或者编辑器中的命令提示符输入命令无法识别，
此时需要重启系统使环境变量全局生效)
```

### 2.3发包出现错误
weex run android 或者weex build android出现错误
```
Error: Error: Command failed: call gradlew.bat  assembleRelease
isLibProject: false, isAppProject: true
weex_plugin: []

FAILURE: Build failed with an exception.

* What went wrong:
A problem occurred configuring project ':app'.
> Failed to notify project evaluation listener.
   > com.android.build.gradle.tasks.factory.AndroidJavaCompile.setDependencyCacheDir(Ljava/io/File;)V

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.
```
解决方法: jdk版本为9的话，修改jdk版本为8
安装jdk8并配置环境变量，可能需要重启或注销系统生效。

修改后，之前的报错没了，又出现另一个错误
```
Error: Error: Command failed: call gradlew.bat  assembleDebug
Exception in thread "main" java.lang.NoClassDefFoundError: Could not initialize class java.nio.file.FileSystems$DefaultFileSystemHolder
        at java.nio.file.FileSystems.getDefault(FileSystems.java:176)
        at java.nio.file.Paths.get(Paths.java:138)
        at sun.misc.Launcher$ExtClassLoader.findLibrary(Launcher.java:224)
        at java.lang.ClassLoader.loadLibrary(ClassLoader.java:1830)
        at java.lang.Runtime.loadLibrary0(Runtime.java:870)
        at java.lang.System.loadLibrary(System.java:1122)
        at sun.security.mscapi.SunMSCAPI$1.run(SunMSCAPI.java:52)
        at sun.security.mscapi.SunMSCAPI$1.run(SunMSCAPI.java:50)
        at java.security.AccessController.doPrivileged(Native Method)
        at sun.security.mscapi.SunMSCAPI.<clinit>(SunMSCAPI.java:50)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
        at java.lang.Class.newInstance(Class.java:442)
        at sun.security.jca.ProviderConfig$2.run(ProviderConfig.java:221)
        at sun.security.jca.ProviderConfig$2.run(ProviderConfig.java:206)
        at java.security.AccessController.doPrivileged(Native Method)
        at sun.security.jca.ProviderConfig.doLoadProvider(ProviderConfig.java:206)
        at sun.security.jca.ProviderConfig.getProvider(ProviderConfig.java:187)
        at sun.security.jca.ProviderList.getProvider(ProviderList.java:233)
        at sun.security.jca.ProviderList.getService(ProviderList.java:331)
        at sun.security.jca.GetInstance.getInstance(GetInstance.java:157)
        at java.security.Security.getImpl(Security.java:695)
        at java.security.MessageDigest.getInstance(MessageDigest.java:167)
        at org.gradle.wrapper.PathAssembler.getHash(PathAssembler.java:65)
        at org.gradle.wrapper.PathAssembler.rootDirName(PathAssembler.java:49)
        at org.gradle.wrapper.PathAssembler.getDistribution(PathAssembler.java:42)
        at org.gradle.wrapper.Install.createDist(Install.java:40)
        at org.gradle.wrapper.WrapperExecutor.execute(WrapperExecutor.java:126)
        at org.gradle.wrapper.GradleWrapperMain.main(GradleWrapperMain.java:56)
```
解决方法: 待解决
配置jdk环境和安卓环境,系统变量PATH使用绝对路径，修改后注销或重启电脑。

### weex从入门到放弃
差点放弃，最终还是坚持下来了。
转而使用react-native,建好demo后，在安卓启动报同样的错误，认识到错误不在weex，还是在环境配置有问题。
修改好环境后，发现weex和react-native都能跑起来了。

### 2.4 npm run android报错
运行后打包成功，但是apk打开后render error:-2013 ,命令行报错operation not permitted或者no such file or directory

发现使用修改过脚手架的版本就会报这个错，把脚手架修改回去，可以正常打包，但这样意味着不能使用vue-router管理页面了。

### 项目中使用iconfont字体图标
由于weex在web端和android以及ios端的图片或样式规则不同，获取本地图片或图标的不方便，所以使用iconfont做项目图标库。
在web中我们使用iconfont一般是引入生成的css文件(本地或者网络上的)。但是，这样使用图表的方式在安卓或ios上会有些不兼容，所以这里使用weex的dom模块的addRule方法进行添加font-face
```
var domModule = weex.requireModule('dom');
      //目前支持ttf、woff文件，不支持svg、eot类型,moreItem at http://www.iconfont.cn/
      
      domModule.addRule('fontFace', {
        'fontFamily': "iconfont1",
        'src': "url('http://at.alicdn.com/t/font_562191_fyhx8bsoirnyu8fr.ttf')"
      });
使用时<text style="font-family:iconfont1">&#xe61d;</text>
```
要注意这里的fontFamily的值不能使用'iconfont'，会与其他样式冲突，使用另一个名字即可

如果需要使用本地资源，图片目录位置在assets下时，可以使用'local://xxx.ttf',本地图片使用方式亦然。