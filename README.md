# QcloudApi-VOD
- php sdk for www.qcloud.com vod service
- 针对腾讯云点播服务(后期可能支持其他腾讯云产品接口服务，有需要可以 [收藏](https://github.com/ShaoZeMing/QcloudApi-VOD/stargazers) || [关注](https://github.com/ShaoZeMing/QcloudApi-VOD/subscription))，基于多个官方SDK进行封装，目前支持：

>    1. 点播服务：
>        - 分类添加，修改，查找，删除。
>        - 视频API上传
>        - 视频信息获取
>        - ·····（待更新）

* 使用本包前，建议阅读官方文档<https://www.qcloud.com/document/product/266/1965>。*
知其然而知其所以然才是修仙成佛之根本。当然了，你真不想看，也阔以！我对很多方法也做了简化的修改，使其看着一目了然。如果这样你还是一脸懵逼？也没关系，后期我也会添加简单使用说明手册，保证智商受限的童鞋也能快速使用。什么？这还不行！（惊讶）那个.....，这。这位看官，别激动，放下你手里的板砖，我们有话好好说，我再给你一个demo....(哭着跑。。。)

如有不当之处，请各位看官批评指正。可以在我的GitHub上<https://github.com/ShaoZeMing/QcloudApi-VOD/issues>讨论

# 文件结构描述

在此简要介绍以下几个文件：

```
    - src/QcloudApi/Common             核心公共类文件目录
    - src/QcloudApi/Module             核心各个云服务模块类目录
    - src/QcloudApi.php                核心加载中心类文件。
    - src/Wrapper/                     使用操作类目录
    - src/Wrapper/CommonVod.php        点播api公共类
    - src/Wrapper/VideoUpload.php      点播视频上传类
    - src/Wrapper/VideoApi.php         点播视频api操作类
    - src/.......(更新中。。。。)
```

# 安装

## 方法 1：(推荐)
执行命令

   `composer require shaozeming/qcloudapi-vod "dev-master"`

直接运行composer自动安装代码。

## 方法 2：
在项目根目录的下composer.json文件中添加代码 `shaozeming/qcloudapi-vod": "dev-master`
```
     "require": {
            "php": ">=5.6.4",
            "laravel/framework": "5.3.*",
            "predis/predis": "^1.1",
            "zizaco/entrust": "5.2.x-dev",
            "shaozeming/qcloudapi-vod": "dev-master"  //添加这一行
        },
```
添加在 require 中。然后执行命令：`composer update`。

## 方法 3：
直接在github<https://github.com/ShaoZeMing/QcloudApi-VOD>,下载源代码。减压到你需要的项目中。

在需要使用的API的代码文件中添加：

`require_once $xx_path.'/vendor/autoload.php'`

注：`$xx_path`指的是基于api文件减压目录路径
该核心类会自动基于composer.json 加载相应的类文件。

# demo示例

## Api上传视频

```
$config = array(
    'SecretId' => 'AKIDG*****************',   //你的腾讯云SecretId
    'SecretKey' => '7O9y2tz***************',      //你的腾讯云SecretKey
);
$vod = new \shaozeming\api_vod\VideoUpload($config);  //要输入的参数
$vod->videoUpload($file);  //上传视频

```
## 添加分类

```
$config = array(
    'SecretId' => 'AKIDG*****************',   //你的腾讯云SecretId
    'SecretKey' => '7O9y2tz***************',      //你的腾讯云SecretKey
);
$vod = new \shaozeming\api_vod\VideoApi($config);  //要输入的参数
$vod->createClass("默认分类1");  //添加分类

```
## 获取层级分类

```
 $data=$vod->getLevelAllClass();

```

## 获取所有分类

```
 $data=$vod->getAllClass();

```

## 修改分类名称

```
 $vod->saveClassName('1608',"我是修改1608后的分类名称");

```

## 获取视频播放Url

```
 $data=$vod->getVideoPlayUrls('14651978969258625432');

```
.....更多方法请看下面方法预览

由于后端上传视频有是基于你服务器后再进行云上传，个人觉得这种方式不推荐。如果真有上传接口需求，可使用web_js 方法上传，可访问web_upload_demo.html进行修改操作。

## videoApi类方法预览

** 功能 | 方法名称 | 参数**

```
创建视频分类      	          CreateClass              $className,[$parentId]
获取用户所有分类层级	      getLevelAllClass         无
获取视频分类列表   	          getAllClass              无
修改分类名	                  saveClassName            $classId,$className
修改视频分类       	          saveVideoClass           $fileId, $classId
删除视频分类      	          deleteClass              $classId
获取视频信息列表              getVideosInfo            $package   //参考官方文档以键值对数组传入
获取视频播放信息列表	      getVideoPlayInfo         $package   //参考官方文档以键值对数组传入
获取视频详细信息	          getVideoPlayUrls         $fileId    //视频ID
修改视频信息      	          saveVideoInfo            $package   //参考官方文档以键值对数组传入
对视频文件转码     	          convertVideoFile         $fileId
删除视频文件      	          deleteVideo              $fileId
```

## videoUpload上传类方法预览

** 功能 | 方法名称 | 参数**

```
视频上传                videoUpload          $fileName, [$notifyUrl], [$isScreenshot], [$isWatermark], [$dataSize]
URL拉取上传视频         videoUrlUpload       $pullUrl, $fileName, [$classId], [$notifyUrl]
官方上传视频自定义方法  videoUploadInfo      $package       //参考官方文档以键值对数组传入

```

### 注：在包中有demo.php，video_upload_demo.php,web_js_upload.html文件，都是demo测试文件，可修改对应的SecretId，SecretKey直接在运行环境中查看示例。在点击删除视频时，要确保该文件时刻拷贝或可以删除，否则后果自负。

如果有问题，欢迎访问我的[博客](http://blog.4d4k.com)或[github](https://github.com/ShaoZeMing/QcloudApi-VOD)进行批评指正。