---
title: "Android Studio Problems"
layout: post
date: 2015-11-28
tags:
- android studio
categories:
- android studio
---

## Unable to run mksdcard SDK tool

![Unable to run mksdcard SDK tool](/assets/image/androidstudio/unable-to-run-mksdcard-sdk-tool.png)

假设你的机器是64位的，那么执行下面命令就OK：

```bash
sudo apt-get install lib32z1 lib32ncurses5 lib32bz2-1.0 lib32stdc++6
```

## Change background color

`Preferences`->`Editor`->`Colors & Fonts`->`General`->`Default text`->`Checkout Background`

## Android Studio Proxy by Lantern

Host name: 127.0.0.1, Port number: 8787

## Extract Hard-Coded String Literals

```bash
Alt + Enter
```

## Delete a module in Android Studio

`File`->`Project Structure`, Select `Modules`, Click `Minus` icon

## ImageView in ListView, placeholder can not use the same ColorDrawable

width and height may be fixed

## Android Test

```java
Running tests
Test running started
junit.framework.AssertionFailedError: No tests found in com.ppmessage.ppcomlib.PPComSDKStartupTest
at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:191)
at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:176)
at android.test.InstrumentationTestRunner.onStart(InstrumentationTestRunner.java:555)
at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:1879)
```

`build.gradle` forget add `testInstrumentationRunner`

```
apply plugin: 'com.android.library'

android {
    compileSdkVersion 23
    buildToolsVersion "23.0.3"

    defaultConfig {
        minSdkVersion 15
        targetSdkVersion 23
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:23.2.1'
    androidTestCompile 'com.android.support.test:runner:0.4'
    androidTestCompile 'org.hamcrest:hamcrest-library:1.3'
    compile project(':sdk')
}
```

## Use `Gradle` wrapper:

`Preferences`->`Build, Execution, Develoyment`->`Build Tools`->`Gradle`:

## Android Studio Build:

`Gradle` 指定版本号: [Android Plugin for Gradle Release Notes](https://developer.android.com/studio/releases/gradle-plugin.html)

`Android 编译过程`: [Configure Your Build](https://developer.android.com/studio/build/index.html)

`Error:Cause: org/gradle/api/publication/maven/internal/DefaultMavenFactory Android`: [cnblogs](http://www.cnblogs.com/NKlaus/p/4730787.html).

原来 Project 下面的 `build.gradle` 为(一部分)：

```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.0'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
        classpath 'com.github.dcendents:android-maven-plugin:1.2'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```

修改后错误消失

```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.0'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```

## Android Studio Module 发布至 jCenter

[Android拓展系列(12)--使用Gradle发布aar项目到JCenter仓库](http://www.cnblogs.com/qianxudetianxia/p/4322331.html)

[How to distribute your own Android library through jCenter and Maven Central from Android Studio](https://inthecheesefactory.com/blog/how-to-upload-library-to-jcenter-maven-central-as-dependency/en)

## Github JCenter Badge

[![Jcenter Status](https://api.bintray.com/packages/{YOUR_NAME}/maven/{YOUR_PROJECT}/images/download.svg)](https://bintray.com/{YOUR_NAME}/maven/{YOUR_PROJECT})