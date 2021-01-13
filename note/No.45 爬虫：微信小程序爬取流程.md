# No.45 爬虫：微信小程序爬取流程

## 导读

> 爬取微信小程序数据的整体流程。

### 1、准备工作

1. [下载微信小程序反编译工具](https://github.com/xuedingmiaojun/wxappUnpacker)

2. [下载微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)

3. 安装node.js

4. 一台已root手机或模拟器。

5. 微信抓包

- 安卓系统 7.0 以下版本，不管微信任意版本，都会信任系统提供的证书

- 安卓系统 7.0 以上版本，微信 7.0 以下版本，微信会信任系统提供的证书

- 安卓系统 7.0 以上版本，微信 7.0 以上版本，微信只信任它自己配置的证书列表

### 2、提取微信小程序wxapkg包

- 清空微信所有数据。

- 打开微信，登录微信账号。

- 打开待爬小程序，等到加载完毕正常显示后关闭。

```shell
从手机上取得wxapkg包
adb pull /data/data/com.tencent.mm/MicroMsg/8f86...(微信标识号)/appbrand/pkg/*.wxapkg
```

### 3、反编译

- 反编译

```shell
# 进入工具目录wxappUnpacker，安装node依赖
npm install esprima css-tree cssbeautify vm2 uglify-es js-beautify escodegen cheerio

# 把取得的.wxapkg文件筛选一下进行反编译
node wuWxapkg.js xxx.wxapkg
# 出现File done，表示反编译完毕，会生成一个同名的目录。打开这个目录就能看到了。
```

- 问题

```shell
# 问题一
Error: Cannot find module 'uglify-es'
    at Function.Module._resolveFilename (internal/modules/cjs/loader.js:581:15)
    at Function.Module._load (internal/modules/cjs/loader.js:507:25)
    at Module.require (internal/modules/cjs/loader.js:637:17)
    at require (internal/modules/cjs/helpers.js:22:18)
    at Object.<anonymous> (/Users/whidy/webs/wxappUnpacker/wuJs.js:3:16)
    at Module._compile (internal/modules/cjs/loader.js:689:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:700:10)
    at Module.load (internal/modules/cjs/loader.js:599:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:538:12)
    at Function.Module._load (internal/modules/cjs/loader.js:530:3)

# 原因：没装好依赖
npm run uglify-es
```

### 4、查看小程序源码

打开微信开发者工具，导入反编译出来的文件夹，就会自动加载小程序。

注意事项：打开详情->本地设置->勾选（不校验合法域名、web-view（业务域名）、TLS版本以及HTTPS证书）
