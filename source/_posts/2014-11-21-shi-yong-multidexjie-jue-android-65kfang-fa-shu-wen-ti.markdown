---
layout: post
title: "使用Multidex解决Android 65K方法数问题"
date: 2014-11-21 18:01:08 +0800
comments: true
categories: 
---
###一、问题
android dex文件方法数进行了限制，不能超过65k，因为google使用了short类型来存储方法数索引。随着工程越来越庞大，已经越来越接近这个数目，即将超越。为了解决这个问题，google在support包中推出了multidex，可以将代码编译成多个dex文件，从而解决这个问题。

###二、加入MultiDex步骤
1. 设置multiDexEnable为true并引入'com.android.support:multidex:1.0.0'

		android {
    	compileSdkVersion 21
    	buildToolsVersion "21.1.0"
 	
    	defaultConfig {
        ...
        minSdkVersion 14
        targetSdkVersion 21
        ...
 
        // Enabling multidex support.
        multiDexEnabled true
    	}
    	...
		}
 
		dependencies {
  			compile 'com.android.support:multidex:1.0.0'
		}
2. Application必须继承MultiDexApplication
具体gradle插件，buildtools版本可参考[这篇文章](https://medium.com/@mustafa01ali/dexs-64k-limit-is-not-a-problem-anymore-well-almost-2b1faac3508)		

###三、效果
1. 编译通过，不报超方法数的错误了
2. 可能会碰到运行时崩溃，有两种可能。一是Multidex还有缺陷，暂不支持手动指定哪些类在首个dex文件中，以至于一些在native中调用java方法的第三方库不被支持
> There are complex requirements regarding what classes are needed in the primary dex file when executing in the Dalvik runtime. The Android build tooling updates handle the Android requirements, but it is possible that other included libraries have additional dependency requirements including the use of introspection or invocation of Java methods from native code. Some libraries may not be able to be used until the multidex build tools are updated to allow you to specify classes that must be included in the primary dex file.

二是gradle插件的问题，请将其版本固定在'com.android.tools.build:gradle:0.14.2'（0.14.3或者0.14.4都有bug）

###四、关于65k方法数的误区
1. 65k的限制只在低版本手机中？并非如此，存在于所有版本中！
2. 低版本运行是发生dexopt的错误是由于方法数超了65k？不一定，发生此错误只表明方法数很多，但不一定超过了65k。在应用的安装过程中，系统会运行一个名为dexopt的程序为该应用在当前机型中运行做准备。dexopt使用LinearAlloc来存储应用的方法信息。Android 2.2和2.3的缓冲区只有5MB，Android 4.x提高到了8MB或16MB。当方法数量过多导致超出缓冲区大小时，会造成dexopt崩溃
