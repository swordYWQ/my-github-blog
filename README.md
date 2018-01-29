# my-github-blog

我的github个人博客

## weex入坑之旅
1. 安卓环境配置

weex项目进行简单开发后，希望能在安卓手机上看到效果，使用命令生成项目和包
```
weex platform add android
--生成安卓项目
```

```
weex run android
--启动安卓项目(需要连接安卓设备)
```
>启动安卓项目出现错误，提示未设置ANDROID_HOME环境变量

解决方法:配置环境变量
```
在计算机->属性->高级系统设置->高级->环境变量中添加环境变量(在系统变量中添加，如果无效的话用户变量和系统变量中都加)
1.添加值ANDROID_HOME value为你的AndroidSDK目录
2.找到PATH变量，在后面加上%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools;(记得分号不能漏)
修改完后运行启动命令提示符窗口，输入adb命令，提示adb的命令帮助则成功，提示找不到命令则失败(此时可能会有在运行中启动的命令提示符输入命令访问成功，但通过shift+右键进入或者编辑器中的命令提示符输入命令无法识别，此时需要重启系统使环境变量全局生效)
```

```
npm run clean && weex build android
--清理项目并发布安卓包
```

>发包出现错误
>```
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

解决方法:待解决
