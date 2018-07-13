# UMShare_tools
# 友盟基础的功能分享统计和登录
- 注意这个新版本和以前版本有很多不同
适用范围
UMeng Analytics iOS SDK适用于iOS 7.0及以上操作系统。

- ** pod 'UMCCommon'**
- ** pod 'UMCAnalytics'**
- ** pod 'UMCSecurityPlugins'**
  
#关于三方的分享和登录的集成需要添加固定的sdk 
- 例如
- ** pod 'UMCShare/Social/WeChat'**
- ** pod 'UMCShare/Social/QQ'**
   

-------------------

## 主动调用的头文件
```
#import <UMCommon/UMCommon.h>
#import <UMAnalytics/MobClick.h>
#import <UMShare/UMShare.h>
// 需要调用的三方的分享和登录
#import <UMSocialWechatHandler.h>
#import <UMSocialQQHandler.h>
```
## AppDelegate 里面需要初始化友盟SDK
```
	/**
	    统计场景类型，默认为普通应用统计：E_UM_NORMAL 
	@param 游戏统计必须设置为：E_UM_GAME.
    */
    [MobClick setScenarioType:E_UM_NORMAL];
    /**
	    开启CrashReport收集, 默认YES(开启状态).
	@param 设置为NO,可关闭友盟CrashReport收集功能.
    */
    [MobClick setCrashReportEnabled:YES];//mi
    /** 初始化友盟所有组件产品
	 @param appKey 开发者在友盟官网申请的appkey.
	 @param channel 渠道标识，可设置nil表示"App Store". 
	 */
    [UMConfigure initWithAppkey:UmengAppkey channel:nil];
    /** 设置是否在console输出sdk的log信息.
	 @param bFlag 默认NO(不输出log); 设置为YES, 输出可供调试参考的log信息. 发布产品时必须设置为NO.
	 */
    [UMConfigure setLogEnabled:NO];
    /** 设置是否对日志信息进行加密, 默认NO(不加密).
	 @param value 设置为YES, umeng SDK 会将日志信息做加密处理
	 */
    [UMConfigure setEncryptEnabled:YES];
```
## 分享调用的方法
```
	//创建分享消息对象
    UMSocialMessageObject *messageObject = [UMSocialMessageObject messageObject];
    //创建网页分享对象
    UMShareWebpageObject *shareObject = [UMShareWebpageObject shareObjectWithTitle:@"标题" descr:nil thumImage:@"图片"];
    //设置网页地址
    shareObject.webpageUrl = @"分享地址URL";
    //分享消息对象设置分享内容对象
    messageObject.shareObject = shareObject;
    
    if (是微博) {
        //创建图片内容对象
        UMShareImageObject *shareObjectWB = [[UMShareImageObject alloc] init];
        shareObjectWB.shareImage = _model.imgUrl;
        shareObjectWB.title = _model.title;
        messageObject.shareObject = shareObjectWB;
    }
    
    //调用分享接口
    [[UMSocialManager defaultManager] shareToPlatform:umType messageObject:messageObject currentViewController:nil completion:^(id result, NSError *error) {
            if (error) {
            NSString *result = @"";
            if ((int)error.code == 2008) {
                result = @"应用未安装";
                
            }else if((int)error.code == 2009) {
                result = [NSString stringWithFormat:@"取消分享"];
            }else {
                result = [NSString stringWithFormat:@"分享失败"];
            }
            // 这里是一个三方只是显示提示
            [QYprogressHUD showInfoWithStatus:result];
            
        }else{
           
           //分享成功之后的操作可以在这里操作
        }
    }];
```

## [新版本更改的不多，其他的可以参考之前的博客看看][1]

---------

[1]: https://blog.csdn.net/sinat_23907467/article/details/53374898
