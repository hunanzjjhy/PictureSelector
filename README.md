# PictureSelector  
   最近项目中用到多图选择上传的需求，考虑到android机型众多问题就自己花时间写了一个，测试了大概60款机型，出现过一些问题也都一一修复了，基本上稳定了特分享出来，界面UI也是商用级的开发者不用在做太多修改了，界面高度自定义，可以设置符合你项目主色调的风格，集成完成后就可以拿来用。
  
  
[![](https://jitpack.io/v/LuckSiege/PictureSelector.svg)](https://jitpack.io/#LuckSiege/PictureSelector)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg)](https://github.com/LuckSiege)
[![CSDN](https://img.shields.io/twitter/url/http/blog.csdn.net/luck_mw.svg?style=social)](http://blog.csdn.net/luck_mw)
[![I](https://img.shields.io/github/issues/LuckSiege/PictureSelector.svg)](https://github.com/LuckSiege/PictureSelector/issues)
[![Star](https://img.shields.io/github/stars/LuckSiege/PictureSelector.svg)](https://github.com/LuckSiege/PictureSelector)


******功能特点：******  
```
  1.适配android6.0+系统
  2.解决部分机型裁剪闪退问题
  3.解决图片过大oom闪退问题
  4.动态获取系统权限，避免闪退
  5.支持相片or视频的单选和多选
  6.支持裁剪比例设置，如常用的 1:1、3：4、3:2、16:9 默认为图片大小
  7.支持视频预览
  8.支持gif图片
  9.支持.webp格式图片
  10.支持一些常用场景设置：如:是否裁剪、是否预览图片、是否显示相机等
  11.新增自定义主题设置
  12.新增图片勾选样式设置
  13.新增图片裁剪宽高设置
  14.新增图片压缩处理
  15.新增录视频最大时间设置
  16.新增视频清晰度设置
  17.新增QQ选择风格，带数字效果
  18.新增自定义 文字颜色 背景色让风格和项目更搭配
  19.新增多图裁剪功能
  20.新增LuBan多图压缩
  21.新增单独拍照功能
  22.新增压缩大小设置
  23.新增Luban压缩档次设置
  24.新增圆形头像裁剪
```

******那些遇到拍照闪退问题的同学，请记得看清下面适配6.0的配置~******

重要的事情说三遍记得添加权限

```
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.CAMERA" />
    
```

******注：适配android6.0以上拍照问题，请在AndroidManifest.xml中添加标签******

```
<provider
   android:name="android.support.v4.content.FileProvider"
   android:authorities="${applicationId}.provider"
   android:exported="false"
   android:grantUriPermissions="true">
     <meta-data
         android:name="android.support.FILE_PROVIDER_PATHS"
         android:resource="@xml/file_paths" />
</provider>

```

******集成步骤******

方式一 compile引入

```
dependencies {
    compile 'com.github.LuckSiege.PictureSelector:picture_library:v1.5.7'
}

```

项目根目录build.gradle加入

```
allprojects {
   repositories {
      jcenter()
      maven { url 'https://jitpack.io' }
   }
}
```

方式二 maven引入 

step 1.
```
<repositories>
       <repository>
       <id>jitpack.io</id>
	<url>https://jitpack.io</url>
       </repository>
 </repositories>
```
step 2.
```
<dependency>
      <groupId>com.github.LuckSiege.PictureSelector</groupId>
      <artifactId>picture_library</artifactId>
      <version>v1.5.7</version>
</dependency>

```

******常见错误*******
```
 问题一：
 rxjava冲突：在app build.gradle下添加
 packagingOptions {
   exclude 'META-INF/rxjava.properties'
 }  
 
 问题二：
 java.lang.NullPointerException: 
 Attempt to invoke virtual method 'android.content.res.XmlResourceParser 
 android.content.pm.ProviderInfo.loadXmlMetaData(android.content.pm.PackageManager, java.lang.String)'
 on a null object reference
 
 application下添加如下节点:
 
 <provider
      android:name="android.support.v4.content.FileProvider"
      android:authorities="${applicationId}.provider"
      android:exported="false"
      android:grantUriPermissions="true">
       <meta-data
         android:name="android.support.FILE_PROVIDER_PATHS"
         android:resource="@xml/file_paths" />
</provider>

注意：如已添加其他sdk或项目中已使用过provider节点，
[请参考我的博客](http://blog.csdn.net/luck_mw/article/details/54970105)的解决方案

问题三：
PhotoView 库冲突，可以删除自己项目中引用的，Picture_library中已经引用过
 
```

******相册启动构造方法******
```
FunctionOptions options = new FunctionOptions.Builder()
        .setType() // 图片or视频 FunctionConfig.TYPE_IMAGE  TYPE_VIDEO
        .setCropMode() // 裁剪模式 默认、1:1、3:4、3:2、16:9
        .setCompress() //是否压缩
        .setEnablePixelCompress() //是否启用像素压缩
        .setEnableQualityCompress() //是否启质量压缩
        .setMaxSelectNum() // 可选择图片的数量
	.setMinSelectNum()// 图片或视频最低选择数量，默认代表无限制
        .setSelectMode() // 单选 or 多选 FunctionConfig.MODE_SINGLE FunctionConfig.MODE_MULTIPLE
	.setVideoS(0)// 查询多少秒内的视频 单位:秒
        .setShowCamera() //是否显示拍照选项 这里自动根据type 启动拍照或录视频
        .setEnablePreview() // 是否打开预览选项
        .setEnableCrop() // 是否打开剪切选项
	.setCircularCut()// 是否采用圆形裁剪
        .setPreviewVideo() // 是否预览视频(播放) mode or 多选有效
        .setCheckedBoxDrawable() // 选择图片样式
        .setRecordVideoDefinition() // 视频清晰度
        .setRecordVideoSecond() // 视频秒数
	.setCustomQQ_theme()// 可自定义QQ数字风格，不传就默认是蓝色风格
        .setGif()// 是否显示gif图片，默认不显示
        .setCropW() // cropW-->裁剪宽度 值不能小于100  如果值大于图片原始宽高 将返回原图大小
        .setCropH() // cropH-->裁剪高度 值不能小于100 如果值大于图片原始宽高 将返回原图大小
        .setMaxB() // 压缩最大值 例如:200kb  就设置202400，202400 / 1024 = 200kb左右
        .setPreviewColor() //预览字体颜色
        .setCompleteColor() //已完成字体颜色
	.setPreviewTopBgColor()//预览图片标题背景色
        .setPreviewBottomBgColor() //预览底部背景色
        .setBottomBgColor() //图片列表底部背景色
        .setGrade() // 压缩档次 默认三档
        .setCheckNumMode() //QQ选择风格
        .setCompressQuality() // 图片裁剪质量,默认无损
        .setImageSpanCount() // 每行个数
        .setSelectMedia() // 已选图片，传入在次进去可选中，不能传入网络图片
        .setCompressFlag() // 1 系统自带压缩 2 luban压缩
        .setCompressW() // 压缩宽 如果值大于图片原始宽高无效
        .setCompressH() // 压缩高 如果值大于图片原始宽高无效
        .setThemeStyle() // 设置主题样式
	.setPicture_title_color() // 设置标题字体颜色
        .setPicture_right_color() // 设置标题右边字体颜色
        .setLeftBackDrawable() // 设置返回键图标
        .setStatusBar() // 设置状态栏颜色，默认是和标题栏一致
        .setImmersive(false)// 是否改变状态栏字体颜色(黑色) 
	.setNumComplete(false) // 0/9 完成  样式
	.setClickVideo()// 点击声音
        .create();     
```
```
或在application进行初始化配置

public class App extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        // application 初始化
        FunctionOptions options = new FunctionOptions.Builder()
	.setType(FunctionConfig.TYPE_IMAGE);
        .setCompress(true);
        .setGrade(Luban.THIRD_GEAR);
	.create();
        PictureConfig.getInstance().init(options);
    }
}
```

******启动相册并拍照******       
```
 PictureConfig.getInstance().init(options).openPhoto(mContext, resultCallback);
 
 或默认配置
 PictureConfig.getInstance().openPhoto(mContext, resultCallback);
```

******单独启动拍照或视频 根据type自动识别******       
```
 PictureConfig.getInstance().init(options).startOpenCamera(mContext);
 
 或默认配置
 PictureConfig.getInstance().startOpenCamera(mContext);
```
******预览图片******       
```
 // 预览图片 可长按保存 也可自定义保存路径
 PictureConfig.getInstance().externalPicturePreview(MainActivity.this, "/custom_file", position, selectMedia);
 PictureConfig.getInstance().externalPicturePreview(mContext, position, selectMedia);
```
******预览视频****** 
```
PictureConfig.getInstance().externalPictureVideo(mContext, selectMedia.get(position).getPath());
```
******图片回调完成结果返回 注意:单独拍照不走此回调，往下看↵******
```
  private PictureConfig.OnSelectResultCallback resultCallback = new PictureConfig.OnSelectResultCallback() {
        @Override
        public void onSelectSuccess(List<LocalMedia> resultList) {
	    // 多选回调
	    selectMedia = resultList;
            Log.i("callBack_result", selectMedia.size() + "");
            LocalMedia media = resultList.get(0);
            if (media.isCut() && !media.isCompressed()) {
                // 裁剪过
                String path = media.getCutPath();
            } else if (media.isCompressed() || (media.isCut() && media.isCompressed())) {
                // 压缩过,或者裁剪同时压缩过,以最终压缩过图片为准
                String path = media.getCompressPath();
            } else {
                // 原图地址
                String path = media.getPath();
            }
            if (selectMedia != null) {
                adapter.setList(selectMedia);
                adapter.notifyDataSetChanged();
            }
        }
	
	 @Override
        public void onSelectSuccess(LocalMedia media) {
            // 单选回调
            selectMedia.add(media);
            if (selectMedia != null) {
                adapter.setList(selectMedia);
                adapter.notifyDataSetChanged();
            }
        }
    };
    
```

******单独拍照回调******
```
    /**
     * 単独拍照图片回调
     *
     * @param requestCode
     * @param resultCode
     * @param data
     */
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK) {
            if (requestCode == FunctionConfig.CAMERA_RESULT) {
                if (data != null) {
                    selectMedia = (List<LocalMedia>) data.getSerializableExtra(FunctionConfig.EXTRA_RESULT);
                    if (selectMedia != null) {
                        adapter.setList(selectMedia);
                        adapter.notifyDataSetChanged();
                    }
                }
            }
        }
    }
```

# 更新日志：

###### 版本 v1.5.7
###### 1.修复传入网络图片压缩失败问题
###### 2.修复传入网络图片裁剪无响应问题
###### 3.修复单独拍照在内存不足时导致activity被回收，回调失败问题
###### 4.单独拍照回调改成走onActivityResult();

# 历史版本：

###### 版本 v1.5.5
###### 1.修复QQ选择风格不同相册下选择数字下标不刷新问题
###### 2.修复拍照和截屏时图片列表图片错乱问题

###### 版本 v1.5.4
###### 移除多余代码，删除多余资源文件
###### 新增图片裁剪框是否可滑动设置

# 项目使用第三方库：
###### 1.裁剪使用ucrop库
###### 2.glide:3.7.0
###### 3.rxjava:2.0.5
###### 4.rxandroid:2.0.1
###### 5.photoView
###### 6.luban

# 混淆配置
```
-keep class com.luck.picture.lib.** { *; }

-dontwarn com.yalantis.ucrop**
-keep class com.yalantis.ucrop** { *; }
-keep interface com.yalantis.ucrop** { *; }
   
 #rxjava
-dontwarn sun.misc.**
-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
 long producerIndex;
 long consumerIndex;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
 rx.internal.util.atomic.LinkedQueueNode producerNode;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueConsumerNodeRef {
 rx.internal.util.atomic.LinkedQueueNode consumerNode;
}

#rxandroid
-dontwarn sun.misc.**
-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
   long producerIndex;
   long consumerIndex;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode producerNode;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueConsumerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode consumerNode;
}

#glide
-keep public class * implements com.bumptech.glide.module.GlideModule
-keep public enum com.bumptech.glide.load.resource.bitmap.ImageHeaderParser$** {
  **[] $VALUES;
  public *;
}

```

# 兼容性测试：
******腾讯优测-深度测试-通过率达到100%******

![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/test.png)

# 演示效果：

![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/1.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/2.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/3.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/4.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/white.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/blue.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/11.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/5.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/6.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/7.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/8.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/9.jpg)
![image](https://github.com/LuckSiege/PictureSelector/blob/master/image/10.jpg)

