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
解决方法: 待解决
