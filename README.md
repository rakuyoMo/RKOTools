# RKOTools

自己平时写的一个小工具库，上传到 **GitHub** 中且支持 `CocoaPods` ，方便自己使用。不断更新完善中。

**注意：** `1.4.0` 之前的版本集成了自己写的一些控件，从 `1.4.0` 版本开始，这些控件不再集成到 `RKOTools` 库中了，将会单独提供并支持 `CocoaPods` 。该页面则做目录之用，将会列出所有控件。

<p align="center">
<a href=""><img src="https://img.shields.io/badge/pod-v1.11.1-brightgreen.svg"></a>
<a href=""><img src="https://img.shields.io/badge/ObjectiveC-compatible-orange.svg"></a>
<a href=""><img src="https://img.shields.io/badge/platform-iOS%209.0%2B-ff69b5152950834.svg"></a>
<a href="https://github.com/rakuyoMo/RKOTools/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-MIT-green.svg?style=flat"></a>
</p>

## 目录

1. [RKOTools](#rkotools-1)
    1. [RKOBaseCell](#rkobasecell)
    2. [NetWorkTool](#networktool)
    3. [CloseKeyBoard](#closekeyboard)
    4. [CollecionLog](#collecionlog)
    5. [TopViewController](#topviewcontroller)
    6. [UIView+StoryBoard](#uiviewstoryboard)
    7. [ImageWithColor](#imagewithcolor)
    8. [HexString](#hexstring)
    9. [Rotate](#rotate)
    10. [UIViewTools](#uiviewtools)
    11. [RKOBaseModel](#rkobasemodel)
    12. [~~DebugDescription~~](#debugdescription)
    13. [~~FastFrame~~](#fastframe)
2. [RKOControl](#rkocontrol)
3. [BLOG](#blog)

## RKOTools

这里是一些平时使用的一些**工具类**。

- 版本号命名规则：
    1. 开头的版本号代表**组织结构**的版本。不修改组织结构的话该编码不变。
    2. 中间的版本号代表库中**可用的组件**数量。
    3. 末尾的版本号代表当前数量下的**修复**版本。

### 集成：

```shell
 pod 'RKOTools', '~> 1.11.1'
```

---------------------------------------------------------------------

### RKOBaseCell

从 `xib` 或者自定义 `Cell` 中快速获取 `Cell` 的一个小工具。接口如下所示：

```objc
/**
 快速获取 cell

 @param tableView 当前的tableView
 @return 一个普通的cell
 */
+ (instancetype)cell:(UITableView *)tableView;

/**
 从xib中获取cell

 @param tableView 当前的tableView
 @return 从xib中获取到的cell
 */
+ (instancetype)xibCell:(UITableView *)tableView;

/**
 获取一个空白的cell

 @param tableView 当前的tableView
 @return 一个空白的cell
 */
+ (id)blankCell:(UITableView *)tableView;
```

---------------------------------------------------------------------

### NetWorkTool

自定义封装的 `AFNetworking` 。初学乍道还不是很完善。

<br>接口提供如下几个**宏定义**：

```objc
// 您可以按照需求随意修改下面的宏的值，也可以在您的.pch文件中重新定义下面的宏。使您的代码逻辑看起来更加易读。
// 请注意不要重名。

// 内存缓存大小。
#define MEMCAPA 5

// 磁盘缓存大小。
#define DISKCAPA MEMCAPA * 2

// 超时时长，默认15秒。
#define TIMEOUT 15

// 缓存的文件夹名，请在您的ViewController.m中对该对象赋值。如：NSString * const diskPath = @"WebCache";
UIKIT_EXTERN NSString * const diskPath;
// 定义baseURL，请在您的ViewController.m中对该对象赋值。如：NSString * const baseURL = @"http://httpbin.org/";
UIKIT_EXTERN NSString * const baseURL;
```

- 注意：在 `RKONetWorkTool.h` 文件中，为了避免添加到CocoaPods不通过，对 `diskPath` 和 `baseURL` 进行了定义。在您使用的时候建议进行删除，将定义写在您的代码中。

<br> `RKONetWorkTool` 提供7个方法，声明如下：

**POST方法：**
```objc
/**
 POST 网络请求
 
 @param url 请求地址（不需要包含baseURL）
 @param param 请求参数 (没有参数, 传 nil 即可)
 @param success 成功回调
 @param failure 请求错误回调
 */
+ (void)POSTWithPath:(NSString *)url
               param:(id)param
             success:(void (^)(id dataObject))success
             failure:(void (^)(NSError *error))failure;
```

**GET方法：**

```objc
/**
 GET 网络请求 获取JSON （自动JSON序列化）
 
 @param url 请求地址（不需要包含baseURL）
 @param param 请求参数 (没有参数, 传 nil 即可)
 @param success 成功回调
 @param failure 请求错误回调
 */
+ (void)GETJSONWithPath:(NSString *)url
                  param:(id)param
                success:(void (^)(id dataObject))success
                failure:(void (^)(NSError *error))failure;

/**
 GET 网络请求 获取HTTP （内部不进行JSON序列化）
 
 @param url 请求地址（不需要包含baseURL）
 @param param 请求参数 (没有参数, 传 nil 即可)
 @param success 成功回调
 @param failure 请求错误回调
 */
+ (void)GETHTTPWithPath:(NSString *)url
                  param:(id)param
                success:(void (^)(id dataObject))success
                failure:(void (^)(NSError *error))failure;
```

**download方法**

```objc
/**
 download 网络请求 自定义文件路径
 
 @param url 请求地址（不需要包含baseURL）
 @param filePath 文件下载路径（如果传nil则存入Caches）
 @param progress 下载进度回调，第一个参数为未处理的进度，第二个参数为带%的进度字符串。（block在主线程中执行）
 @param success 成功回调
 @param failure 请求错误回调
 */
+ (void)downloadWithPath:(NSString *)url
                filePath:(NSString *)filePath
                progress:(void (^)(double_t fractionCompleted, NSString *progressStr))progress
                 success:(void (^)(id dataObject))success
                 failure:(void (^)(NSError *error))failure;

/**
 download 网络请求 默认保存到Caches
 
 @param url 请求地址（不需要包含baseURL）
 @param progress 下载进度回调，第一个参数为未处理的进度，第二个参数为带%的进度字符串。（block在主线程中执行）
 @param success 成功回调
 @param failure 请求错误回调
 */
+ (void)downloadWithPath:(NSString *)url
                progress:(void (^)(double_t fractionCompleted, NSString *progressStr))progress
                 success:(void (^)(id dataObject))success
                 failure:(void (^)(NSError *error))failure;

```

**upload方法：**

```objc
/**
 upload 网络请求 （同步）
 
 @param url 请求地址（不需要包含baseURL）
 @param fileName 要上传的文件名
 @param progress 下载进度回调，第一个参数为未处理的进度，第二个参数为带%的进度字符串。（block在主线程中执行）
 @param success 成功回调
 @param failure 请求错误回调
 */
+ (void)uploadSynWithPath:(NSString *)url
                 fileName:(NSString *)fileName
                 progress:(void (^)(double_t fractionCompleted, NSString *progressStr))progress
                  success:(void (^)(id dataObject))success
                  failure:(void (^)(NSError *error))failure;

/**
 upload 网络请求 （异步）
 
 @param url 请求地址 (必须包含baseURL在内的使用全地址)
 @param fileName 要上传的文件名
 @param assocName 与指定数据进行关联的名称
 @param progress 下载进度回调，第一个参数为未处理的进度，第二个参数为带%的进度字符串。（block在主线程中执行）
 @param success 成功回调
 @param failure 请求错误回调
 */
+ (void)uploadAsynWithPath:(NSString *)url
                  fileName:(NSString *)fileName
            associatedName:(NSString *)assocName
                  progress:(void (^)(double_t, NSString *))progress
                   success:(void (^)(id))success
                   failure:(void (^)(NSError *))failure;
```

另外还额外提供了一个**单例方法**：

```objc
/**
 单例方法 内配置了baseURL与超时时长，超时时长默认15s。
 
 @return RKONetWorkTool类对象。
 */
+ (instancetype)sharedManager;
```

提供一个协议 `RKONetWorkToolDelegate` ，协议声明如下：

```objc
/**
 RKONetWorkToolDelegate 代理协议，用以提供设置无网络时候设置Alert的方法。
 
 之所以使用代理而不是直接把Alert写在本类中，是为了开放接口，方便使用者定义自己的Alert，而不用进到本类中修改代码。
 */
@protocol RKONetWorkToolDelegate <NSObject>

- (void)networkStaAlertWithNetWorkTool:(RKONetWorkTool *)netWorkTool;

@end
```

遵守该协议并实现 `networkStaAlertWithNetWorkTool:` 方法，即可设置在无网络时的弹窗提示。

---------------------------------------------------------------------

### CloseKeyBoard

`UITableView` 的分类，点击空白处关闭键盘的小工具。在需要的地方引入头文件即可
<br><br>转载自简书：<br>
[iOS利用响应链机制点击tableview空白处关闭键盘](http://www.jianshu.com/p/9717b792599c)**评论中**的**鱼鱼鱼四只鱼**提供的代码。

---------------------------------------------------------------------

### CollecionLog

`NSDictionary` 、 `NSArray` 和 `NSSet` 的分类，拼接字符串，解决字典、数组和集合中**输出中文**的时候是 `unicode` 编码的问题。

- **注意**：该类于 `2018/03/07` 有**更新**。

---------------------------------------------------------------------

### TopViewController

`UIViewController` 的分类，用来获取当前界面真在显示的 `ViewController` ，接口部分如下所示：

```objc
@interface UIViewController (RKOTopViewController)

+ (UIViewController *)topViewController;

@end
```

在需要的地方导入头文件，调用 `topViewController` 方法即可。

---------------------------------------------------------------------

### UIView+StoryBoard

原先 `CALayer+Additions` 的代替品。

`UIView` 的分类。使用~~`IB_DESIGNABLE`及~~ `IBInspectable` ，可以在 `StoryBoard` 中快速设置视图**圆角**、**边框**以及**阴影**。

~~并且提供一个空白的`UIView`子类`RKOBaseStoryBoardView`，如果您在`StoryBoard`中的空白`UIView`需要**所见即所得**，那么可以选择关联到该类上。~~

具体来说我们提供的属性如下：

```objc
/** 圆角。 */
@property (nonatomic,assign) IBInspectable CGFloat cornerRadius;

/** 边框宽度和颜色。 */
@property (nonatomic,assign) IBInspectable CGFloat borderWidth;
@property (nonatomic,strong) IBInspectable UIColor *borderColor;

/** 阴影。 */
@property (nonatomic,assign) IBInspectable float shadowOpacity;
@property (nonatomic,strong) IBInspectable UIColor *shadowColor;
@property (nonatomic,assign) IBInspectable CGFloat shadowRadius;
@property (nonatomic,assign) IBInspectable CGSize shadowOffset;
```

在`StoryBoard`中如下所示：

![](https://github.com/rakuyoMo/RKOTools/raw/master/READMEImage/WX20170916-211920@2x.png)

~~- 注意：如果您有如下警告，请在`podfile`文件中添加`use_frameworks!`语句~~

```shell
# IB Designables: Failed to render and update auto layout status for WANBaseVi....
```

- **注意：** 在 `Xcode 9` 以及 `iOS 11` 中， `IB_DESIGNABLE` 宏在 `Objective-C` 中依然存在bug，且暂时无法修复，故该控件还是放弃使用了这个宏。其衍生基类暂时保存。

---------------------------------------------------------------------

### ImageWithColor

`UIImage` 的分类方法，可以根据某一颜色生成一张图片，方便设置 `UIButton` 的背景图。

使用时调用下面的方法，传入颜色即可：

```objc
+ (UIImage *)imageWithColor:(UIColor *)color;
```

---------------------------------------------------------------------

### HexString

`UIColor` 的分类，用于**将十六进制字符串转为数字**。当我们存储颜色值的时候可能会存颜色的十六进制数字，有的时候会以字符串去存，这个时候就用到了这个方法了。

使用时调用下面的方法，传入字符串即可：

```objc
+ (NSInteger)colorWithHexString:(NSString *)hexString;
```

---------------------------------------------------------------------

### Rotate

`UIImage` 的分类。用于**将图片旋转指定的角度/弧度**。

若需按 **角度** 旋转，可调用该方法：

```objc
- (UIImage *)imageRotatedByRadians:(CGFloat)radians;
```

如若按弧度旋转，则调用该方法：

```objc
- (UIImage *)imageRotatedByDegrees:(CGFloat)degrees;
```

---------------------------------------------------------------------

### UIViewTools

`UIView` 的分类。该分类用于集中管理一些 `UIView` 的扩展，但是难以单独分离出来命名的。未来某些组件**可能从该分类中剥离**出来单独成一个分类。

目前该分类提供以下功能：

1. 寻找第一响应者的视图。


#### 寻找第一响应者

```objc
- (UIView *)findFirstResponder;
```

---------------------------------------------------------------------

### RKOBaseModel

模型基类，集成了 `YYModel`，借由 `YYModel` 实现了 `Coding`、`Copying`、`hash`、`equal` 方法。同时重写了 `description` 方法，用来代替原先的 `DebugDescription` 分类，提供**打印模型时，同时打印属性值**的功能。

使用时只需让您的模型类继承自 `RKOBaseModel` 即可。

---------------------------------------------------------------------

### DebugDescription

**因其会导致 po 数组、字典时出现 bug，故从库中删除。现已用 RKOBaseModel 取代。**

~~`NSObject` 的分类。方便我们在使用 `po` 打印模型时，输出其下元素的具体内容而非单一地址。~~

~~重写了 `debugDescription` 方法。无需调用，集成到项目中就可以直接使用。~~

~~参照 [iOS模型打印](http://www.cocoachina.com/ios/20170728/20055.html) 编写。~~

---------------------------------------------------------------------

### FastFrame

**因其与** [Masonry](https://github.com/SnapKit/Masonry) **冲突，故从库中删除**。如果您有需要，可 [点击这里](https://github.com/rakuyoMo/RKOTools/commit/79426847a70577038c93f1d1a95bd887577f13d7) ，查看相关代码与记录在 `README` 文件中的 API 说明。

---------------------------------------------------------------------

## RKOControl

安利一下自己写的另外一个控件库。原本和 `RKOTools` 在一个库下，后来觉得有点冗余所以分成了两个库。

下面几个都是封装的一些**小控件**。具体介绍不再在该页面提供，请点击对应的 **Github** 页面查看。

---------------------------------------------------------------------

### RKONetworkAlert

<p align="left">
<a href=""><img src="https://img.shields.io/badge/pod-v1.0.0-brightgreen.svg"></a>
<a href=""><img src="https://img.shields.io/badge/ObjectiveC-compatible-orange.svg"></a>
<a href=""><img src="https://img.shields.io/badge/platform-iOS%207.0%2B-ff69b5152950834.svg"></a>
<a href="https://github.com/rakuyoMo/RKOTools/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-MIT-green.svg?style=flat"></a>
</p>

地址：[RKONetworkAlert](https://github.com/rakuyoMo/RKOControls/tree/master/RKONetworkAlertManager)

#### 集成：

```shell
 pod 'RKONetworkAlert', '~> 1.0.0'
```

---------------------------------------------------------------------

### RKOTextView

<p align="left">
<a href=""><img src="https://img.shields.io/badge/pod-v1.1.1-brightgreen.svg"></a>
<a href=""><img src="https://img.shields.io/badge/ObjectiveC-compatible-orange.svg"></a>
<a href=""><img src="https://img.shields.io/badge/platform-iOS%209.0%2B-ff69b5152950834.svg"></a>
<a href="https://github.com/rakuyoMo/RKOTools/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-MIT-green.svg?style=flat"></a>
</p>

地址：[RKOTextView](https://github.com/rakuyoMo/RKOControls/tree/master/RKOTextViewManager)

#### 集成：

```shell
 pod 'RKOTextView', '~> 1.1.1'
```

---------------------------------------------------------------------

### RKOTopAlert

<p align="left">
<a href=""><img src="https://img.shields.io/badge/pod-v1.1.0-brightgreen.svg"></a>
<a href=""><img src="https://img.shields.io/badge/ObjectiveC-compatible-orange.svg"></a>
<a href=""><img src="https://img.shields.io/badge/platform-iOS%209.0%2B-ff69b5152950834.svg"></a>
<a href="https://github.com/rakuyoMo/RKOTools/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-MIT-green.svg?style=flat"></a>
</p>

地址：[RKOTopAlert](https://github.com/rakuyoMo/RKOControls/tree/master/RKOTopAlertManager)

#### 集成：

```shell
 pod 'RKOTopAlert', '~> 1.1.0'
```

---------------------------------------------------------------------

### RKOTabBar

封装的一个 `TabBar` ，但是效果并是很好....想了想还是不放在这里了，几乎用不到，每次都要删除怪麻烦的。

---------------------------------------------------------------------

## BLOG

本人课余时间利用 `HEXO` 在 GitHub 上搭建的博客。未来部分工具会有对应的 blog 文章对应。在这里也把 blog 的地址贴出来吧：<br><br>
<a href="https://rakuyomo.github.io" target="_blank">喵喵喵</a>
