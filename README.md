# 天鹏广告Android SDK接入文档

## 1.概述

  尊敬的开发者朋友，欢迎您使用天鹏广告sdk平台。通过本文档，您可要轻松的在几分钟之内完成广告的集成过程。
  
## 2.Demo以及sdk下载
  
  Demo下载地址(https://github.com/tianpengco/tpad)
  
## 3.SDK接入流程
 
 ### 3.1 添加sdk到工程
 
    接入环境：Androidstudio
    
    可以选择依赖库添加：[![](https://jitpack.io/v/tianpengco/tpad.svg)](https://jitpack.io/#tianpengco/tpad)
    或者复制Demo中libs文件目录下的依赖包到项目中。
    >```
    >android {
    >....
    >//1.添加依赖目录
    >repositories {
      >flatDir {
       > dirs 'libs'
      >}
      >}
    >}
     > dependencies {
      >....
      >//2.添加依赖包
       > compile(name: 'tpadsdk-release', ext: 'aar')
      >}
    >```
