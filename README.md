# 天鹏广告Android SDK接入文档

## 1.概述

  尊敬的开发者朋友，欢迎您使用天鹏广告sdk平台。通过本文档，您可以轻松的在几分钟之内完成广告的集成过程。
  
## 2.Demo以及sdk下载
  
  [Demo下载地址](https://github.com/tianpengco/tpad)
  
## 3.SDK接入流程
 
 ### 3.1 添加sdk到工程
 
   接入环境：Androidstudio
    
   可以选择依赖库添加：
   [![](https://jitpack.io/v/tianpengco/tpad.svg)](https://jitpack.io/#tianpengco/tpad)</br>
   或者复制Demo中libs文件目录下的依赖包到项目中。
   
   ```Java
   android {
    ....
    //1.添加依赖目录
    repositories {
      flatDir {
        dirs 'libs'
      }
      }
    }
      dependencies {
      ....
      //2.添加依赖包
        compile(name: 'tpadsdk-release', ext: 'aar')
      }
   ```
   #### 注意事项
   
   *fastjson҅org.apache.http.legacy҅jaredrummler.AndroidProcesse，如果你的应用中使用以上几个第三方库，需要移除避免冲突。
   *如果项目中集成了其他广告平台sdk也尽量移除避免冲突
   
   ### 3.2权限申请
   
   使用sdk时可能需要以下权限，为了保证使用广告的正确，请在6.0以及以上的手机中使用sdk前及时申请
   ```Java
   <uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /> <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /> <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" /> <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" /> <uses-permission android:name="android.permission.GET_TASKS" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" /> <uses-permission android:name="android.permission.ACCESS_COARSE_UPDATES" /> <uses-permission android:name="android.permission.REQUEST_INSTALL_PACKAGES" />
```
PS:ACCESS_COARSE_LOCATION̵READ_PHONE_STATE̵WRITE_EXTERNAL_STORAGE̵
ACCESS_NETWORK_STATE̵ACCESS_WIFI_STATE这几个权限请确保获取，否则可能无法获取广告
（可参考Demo中的SplashActivity）

## 4. 接入代码

### 4.1 sdk初始化

提示：appid请联系商务获取，并在Application的onCreat()方法中进行SDK初始化
```Java
 // TODO: 2018/10/22 修改为自己的appId 测试id = 123
        // TODO: debug参数 默认传入false.
        TPADMobSDK.instance().initSdk(getApplicationContext(), APP_ID, debug);
```
### 4.2 开屏广告示例

提示：</br>
1.开屏广告默认为屏幕高度的75%，可自定义高度比例，但不能低于0.75</br>
2.注意加载开屏广告时，请保证开屏view控件处于可见状态，否则会出现获取不到广告的情况

```Java
 // 开屏广告高度默认为屏幕高度的75%,可自定义高度比例,但不能低于0.75;
        adMobGenSplashView = new ADMobGenSplashView(this,0.8);
        adMobGenSplashView.setListener(new ADMobGenSplashAdListener(){
            @Override
          public void onADFailed(String error) {
             Log.e(TAG, "广告获取失败了 ::::: " + error);
          }

          @Override
          public void onADReceiv() {
            Log.e(TAG, "广告获取成功了 ::::: ");
          }

          @Override
          public void onADClick() {
              Log.e(TAG, "广告被点击了 ::::: ");
        
          }

          @Override
          public void onAdClose() {
       
              Log.e(TAG, "广告被关闭了 ::::: ");
        
          }

          @Override
          public void onADExposure() {
              Log.e(TAG, "广告展示曝光回调，但不一定是曝光成功了，比如一些网络问题导致上报失败 ::::: ");
          }
        });
        flContainer.addView(adMobGenSplashView);
        adMobGenSplashView.loadAd();
        
 ```
 
### 4.3 banner广告示例

Banner广告控件容器保证不低于50dp，建议使用自适应

```Java
 // 初始化Banner广告
        adMobGenBannerView = new ADMobGenBannerView(this);
        // 不设置banner广告尺寸大小则默认比例为: 640*100;
//        adMobGenBannerView.setADSize(640,100);
        // 设置广告监听（不设置也行）
        adMobGenBannerView.setListener(new ADMobGenBannerAdListener() {
            @Override
            public void onADExposure() {
                Log.e(TAG, "广告展示曝光回调，但不一定是曝光成功了，比如一些网络问题导致上报失败 ::::: ");
            }

            @Override
            public void onADFailed(String s) {
                Log.e(TAG, "广告获取失败了 ::::: " + s);
            }

            @Override
            public void onADReceiv() {
                Log.e(TAG, "广告获取成功了 ::::: ");
            }

            @Override
            public void onADClick() {
                Log.e(TAG, "广告被点击了 ::::: ");
            }

            @Override
            public void onAdClose() {
                Log.e(TAG, "广告被关闭了，改回调不一定会有 ::::: ");
            }
        });
        // 把广告控件添加到容器
        flContainer.addView(adMobGenBannerView);
        // 开始获取广告
        adMobGenBannerView.loadAd();
  ```
  
  ### 4.4 信息流广告示例
  
  ```Java
  adMobGenInformation = new ADMobGenInformation(this,adType);
        adMobGenInformation.setListener(new SimpleADMobGenInformationAdListener() {
            @Override
            public void onADExposure(IADMobGenInformation adMobGenInformation) {
                Log.e(TAG, "广告展示曝光回调，但不一定是曝光成功了，比如一些网络问题导致上报失败 	::::: ");
            }

            @Override
            public void onADReceiv(IADMobGenInformation adMobGenInformation) {
                Log.e(TAG, "广告获取成功 ::::: ");
                
            }

            @Override
            public void onADClick(IADMobGenInformation adMobGenInformation) {
                Log.e(TAG, "广告被点击 ::::: ");
            }

            @Override
            public void onADFailed(String error) {
                Log.e(TAG, "广告数据获取失败时回调 ::::: " + error);
                
            }
        });
        adMobGenInformation.loadAd();
        // 纯图信息流
        //adMobGenInformation.loadOnlyImageAd()
   ```
   
   ### 4.5 插屏广告示例
   
   ```Java
   adMobGenInsertView = new ADMobGenInsertView(MainActivity.this);
                adMobGenInsertView.setListener(new ADMobGenInsertitailAdListener() {
                    @Override
                    public void onADExposure() {
                        Log.e(TAG, "广告展示曝光回调，但不一定是曝光成功了，比如一些网络问题导致上报失败 ::::: ");
                    }

                    @Override
                    public void onADOpen() {
                        Log.e(TAG, "广告打开成功了 ::::: ");
                    }

                    @Override
                    public void onADLeftApplication() {
                        Log.e(TAG, "广告onADLeftApplication ::::: ");
                    }

                    @Override
                    public void onADFailed(String s) {
                        Log.e(TAG, "广告获取失败了 ::::: " + s);
                    }

                    @Override
                    public void onADReceiv() {
                        Log.e(TAG, "广告获取成功了 ::::: ");
                    }

                    @Override
                    public void onADClick() {
                        Log.e(TAG, "广告被点击了 ::::: ");
                    }

                    @Override
                    public void onAdClose() {
                        Log.e(TAG, "广告被关闭了，改回调不一定会有 ::::: ");
                    }
                });
                adMobGenInsertView.loadAd();
   ```
   
   注意事项：
   
   在不使用的时候（退出activity等）记得及时释放广告资源
   
   
   ## 5.适配Android7.0
   
   如果您的应用需要适配7.0以上，请在AndroidManifest中添加以下代码：
   ```Java
   <provider
android:name="android.support.v4.content.FileProvider"
android:authorities="${applicationId}.fileprovider"
android:exported="false"
android:grantUriPermissions="true">
<meta-data
android:name="android.support.FILE_PROVIDER_PATHS"
android:resource="@xml/file_paths" />
</provider>
```

在res/xml目录下，新建一个XML文件ﬁle_paths,在该文件中添加如下代码：

```Java
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
<external-path name="external_path" path="." />
<external-files-path name="external_files_path" path="." />
</paths>
```
为了适配下载和安装相关功能，在工程中引用包  com.android.support:support-v4:24.2.0 使用24.2.0以及以上版本


## 6. 混淆配置

广告sdk内部混淆，若您项目需要进行混淆则需要在混淆文件中添加以下配置

```Java
-dontwarn com.tianpeng.tp_adsdk.**
-keep class com.tianpeng.tp_adsdk.**{*;}
-keep interface com.tianpeng.tp_adsdk.**{*;}
-keep class com.android.**{*;}
-keep class cn.async.admobhttp.**{	*;	}
-keep class com.jaredrummler.android.processes.**{*;}
-keep class com.jaredrummler.android.processes.models.**{*;}
-dontwarn org.apache.commons.**
-keep class org.apache.**{	*;	}
#广点通sdk
-keep class com.qq.e.**	{public protected *;}
-keep class android.support.v4.**{public *;}
-keep class android.support.v7.**{public *;}
-keep class MTT.ThirdAppInfoNew	{*;}
-keep class com.tencent.**	{*;}
#百度sdk
-keepclassmembers class * extends android.app.Activity	{
public void *(android.view.View);}
-keepclassmembers enum *	{
public static **[]	values();
public static ** valueOf(java.lang.String);
		}
-keep class com.baidu.mobads.*.**{*;}
#有道sdk
-ignorewarnings
-keep class com.youdao.sdk.**{*;}
#头条 穿山甲 sdk
-keep class com.bytedance.sdk.openadsdk.**	{	*;	}
-keep class com.androidquery.callback.**	{*;}
-keep class com.bytedance.sdk.openadsdk.service.TTDownloadProvider
```

## 7. 常见问题

get ad ﬁled  广告未放量，请联系商务</br>

platams is empty  广告渠道未接入，请联系商务
   
   
