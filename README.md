
# 重要说明

由于苹果iOS 13系统版本安全升级，微信官方SDK在1.8.6版本进行了适配并且支持*Universal Links*方式跳转，以及分享时的合法性校验。

插件3.0.0版本接入了最新的微信SDK，在使用之前你需要配置Universal Links服务，并且在安装插件的时候注意传入`universallink`变量，否则将无法正常使用。

如果你不想使用新版本功能，可以回退3.0.0版本之前。

# 关于

微信sdk的cordova插件

# 功能

微信登录，微信分享，微信投票



# 安装

1. ```cordova plugin add cordova-plugin-wechat-share  --variable wechatappid=YOUR_WECHAT_APPID```, or using [plugman](https://npmjs.org/package/plugman), [phonegap](https://npmjs.org/package/phonegap), [ionic](http://ionicframework.com/)

2. ```cordova build ios``` or ```cordova build android```

3. (iOS only) if your cordova version <5.1.1,check the URL Type using XCode

# 用法

## 检查微信是否安装
```Javascript
Wechat.isInstalled(function (installed) {
    alert("Wechat installed: " + (installed ? "Yes" : "No"));
}, function (reason) {
    alert("Failed: " + reason);
});
```

## 微信认证
```Javascript
var scope = "snsapi_userinfo",
    state = "_" + (+new Date());
Wechat.auth(scope, state, function (response) {
    // you may use response.code to get the access token.
    alert(JSON.stringify(response));
}, function (reason) {
    alert("Failed: " + reason);
});
```

## 分享文本
```Javascript
Wechat.share({
    text: "This is just a plain string",
    scene: Wechat.Scene.TIMELINE   // share to Timeline
}, function () {
    alert("Success");
}, function (reason) {
    alert("Failed: " + reason);
});
```

## 分享媒体（例如链接，照片，音乐，视频等）
```Javascript
Wechat.share({
    message: {
        title: "Hi, there",
        description: "This is description.",
        thumb: "www/img/thumbnail.png",
        mediaTagName: "TEST-TAG-001",
        messageExt: "这是第三方带的测试字段",
        messageAction: "<action>dotalist</action>",
        media: "YOUR_MEDIA_OBJECT_HERE"
    },
    scene: Wechat.Scene.TIMELINE   // share to Timeline
}, function () {
    alert("Success");
}, function (reason) {
    alert("Failed: " + reason);
});
```

### 分享网页
```Javascript
Wechat.share({
    message: {
        ...
        media: {
            type: Wechat.Type.WEBPAGE,
            webpageUrl: "http://tech.qq.com/zt2012/tmtdecode/252.htm"
        }
    },
    scene: Wechat.Scene.TIMELINE   // share to Timeline
}, function () {
    alert("Success");
}, function (reason) {
    alert("Failed: " + reason);
});
```

### 分享到小程序
```Javascript
Wechat.share({
    message: {
        ...
        media: {
            type: Wechat.Type.MINI,
            webpageUrl: "http://www.jason-z.com", // 兼容低版本的网页链接
            userName: "wxxxxxxxx", // 小程序原始id
            path: "user/info", // 小程序的页面路径
            hdImageData: "http://wwww.xxx.com/xx.jpg", // 程序新版本的预览图二进制数据 不超过128kb
            withShareTicket: true, // 是否使用带shareTicket的分享
            miniprogramType: Wechat.Mini.RELEASE //正式版:0，测试版:1，体验版:2
        }
    },
    scene: Wechat.Scene.TIMELINE   // share to Timeline
}, function () {
    alert("Success");
}, function (reason) {
    alert("Failed: " + reason);
});
```

 