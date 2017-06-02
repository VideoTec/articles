## 错误描述
使用 Android Studio 的 'Debug' 按钮构建并安装应用后，无法启动，错误信息
```
06/02 11:36:28: Launching app
$ adb install-multiple -r C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\dep\dependencies.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_3.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_0.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_2.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_1.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_7.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_6.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_4.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_5.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_8.apk C:\work\FVideoDemo\app\build\intermediates\split-apk\debug\slices\slice_9.apk C:\work\FVideoDemo\app\build\outputs\apk\app-debug.apk 
Split APKs installed
$ adb shell am startservice work.wangxiang.fvideodemo/com.android.tools.fd.runtime.InstantRunService
Error while executing: am startservice work.wangxiang.fvideodemo/com.android.tools.fd.runtime.InstantRunService
Starting service: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] cmp=work.wangxiang.fvideodemo/com.android.tools.fd.runtime.InstantRunService launchParam=MultiScreenLaunchParams { mDisplayId=0 mFlags=0 } }
Error: Not found; no service started.
```
使用的 Android Studio 版本是 2.3.0

## 出错原因
项目的 root-build.gradle 指定使用的 gradle 版本号为：2.3.2
```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.2'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }

}
```
## 解决方法
1. 把 Android Studio 升级到 2.3.2
2. 把 gradle 版本号改为 2.3.0
## 如果不是这里所描述的原因，可以参考以下方法
1. 解决方案是打开Setting(shortcut : Ctrl+Alt+S) 输入Intant Run，禁用它
2. Developer Settings > Turn on MIUI optimization and turn it off
3. give that app Auto-Start permission with Auto Start Manager.