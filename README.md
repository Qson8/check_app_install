# check_app_install

#### 逻辑
最近在写的应用需要实现打开微信的功能，对应原生来说这个很容易实现。
思路就是：
    * iOS需要找到对应App的URL Scheme，配置在项目中
    * 检测是否能打开这个App
    * 打开App     

#### 实现

##### | 配置URL Scheme
iOS需要在项目中配置指定的URL Scheme，可以在[点击这里](http://blog.520lee.com/2019/12/07/iOS：%20URL%20Scheme%20一览/)查看你需要打开应用的URL Scheme，

提前设置好打开APP的列表，也就是白名单，并配置到工程的 info.plist中去。
LSApplicationQueriesSchemes ，加入对应的 URL Scheme，如图
![1](media/15757108482340/1.jpg)

```
<key>LSApplicationQueriesSchemes</key>
	<array>
		<string>wechat</string>
	</array>
```


##### | 编码实现

检测是否能打开这个App，并打开app业务实现，我们可以使用url_launcher这个插件，可以到这里搜索https://pub.dartlang.org/

* 在项目.yaml文件依赖内添加库依赖，vscode保存会自动获得packages,或终端命令：`flutter packages get`.


* 实现调转的逻辑，也可以写在你封装好的公共类里，提供接口出来。可以拷贝下面的代码，在需要调用的地方调用这个方法即可。

```
/*
  * 检测是否安装了微信
  * 
  * 1. 配置url Scheme
  * 2. 用Application 的canopenUrl 的方法判断 (YES代表已安装)
  */
  _launchURL() async {
    // 1.url Scheme
    const url = 'wechat://';
    
    // 2. 判断当前手机是否安装某app. 能否正常跳转
    if (await canLaunch(url)) {
      // 2.1 正常跳转
      await launch(url);
    } else {
      // 2.2 不能跳转
      throw 'Could not launch $url';
    }
  }
```

相关源码可以到github下载：https://github.com/Qson8/check_app_install
