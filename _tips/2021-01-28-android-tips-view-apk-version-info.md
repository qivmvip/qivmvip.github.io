---
layout: post
title: "[Android Tips] - 查看 APK 版本信息"
date: 2021-01-28 03:00
description: 通过 `aapt` 和 `apktool` 工具查看 APK 版本信息
tags:
 - tips
 - android
 - aapt
 - apktool
 - version
 - manifest
---

## [Android Tips] - 查看 APK 版本信息

### 方案一、用 AAPT 查看

1、aapt dump badging

```bash
$ aapt dump badging example.apk | grep version
package: name='com.example.app' versionCode='21036' versionName='7.2.13.8' platformBuildVersionName=''
```

2、aapt dump xmltree

只不过版本号 `versionCode` 是 16 进制。

```bash
$ aapt dump xmltree example.apk AndroidManifest.xml | grep version
    A: android:versionCode(0x0101021b)=(type 0x10)0x522c
    A: android:versionName(0x0101021c)="7.2.13.8" (Raw: "7.2.13.8")
```

### 方案二、用 apktool 反编译查看

反编译后 APK 的版本相关信息存放在 `apktool.yml` 文件里面，不在 decode 后的 `AndroidManifest.xml` 里面。

```bash
$ apktool d example.apk
I: Using Apktool 2.5.0 on example.apk
I: Loading resource table...
I: Decoding AndroidManifest.xml with resources...
I: Loading resource table from file: /Users/example/Library/apktool/framework/1.apk
I: Regular manifest package...
I: Decoding file-resources...
I: Decoding values */* XMLs...
I: Baksmaling classes.dex...
I: Baksmaling classes2.dex...
I: Baksmaling classes3.dex...
I: Baksmaling classes4.dex...
I: Baksmaling classes5.dex...
I: Baksmaling classes6.dex...
I: Baksmaling classes7.dex...
I: Baksmaling classes8.dex...
I: Baksmaling classes9.dex...
I: Copying assets and libs...
I: Copying unknown files...
I: Copying original files...

$ grep -i "version" example/apktool.yml
  minSdkVersion: '18'
  targetSdkVersion: '28'
version: 2.5.0
versionInfo:
  versionCode: '21026'
  versionName: 7.2.12.174
```
