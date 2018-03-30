# 在任意文件夹使用NDK-BUILD命令来构建 NDK 动态库

## 创建空文件夹 ndk-src
创建一个空文件夹，用于存放NDK构建脚本文件及源文件。 

注：也可以在已有源文件目录，添加NDK构建脚本文件。

## 新建NDK构建脚本文件及源文件
ndk-src\Android.mk
```
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)
LOCAL_MODULE    := color_space_util
LOCAL_SRC_FILES := yuv2rgb.c
include $(BUILD_SHARED_LIBRARY)
```
ndk-src\Application.mk
```
APP_PLATFORM := android-14
APP_ABI := armeabi
```
ndk-src\yuv2rgb.c
```
#include <stdio.h>

void test() {
	printf("hello world");
}
```

## 新建批处理文件，用于执行构建命令
ndk-src\build.bat
```
@echo off
cd %~dp0
:BUILD
call ndk-build V=0 NDK_DEBUG=0 NDK_PROJECT_PATH=. APP_BUILD_SCRIPT=./Android.mk NDK_APPLICATION_MK=./Application.mk
:WAIT
set /p KEY=
cls
if "%KEY%"=="C" (
  call ndk-build clean NDK_PROJECT_PATH=. APP_BUILD_SCRIPT=./Android.mk NDK_APPLICATION_MK=./Application.mk
  set KEY=
  goto WAIT

) else (
  goto BUILD
)
```


## 生成SO文件
### 执行批处理后，构建生成的NDK动态库，输出到： 
ndk-src\libs
### 复制到 android studio 工程中
android studio 默认存放 NDK 动态库的路径：
```
$(project-dir)/module-path/src/jniLibs/
```