# electron-app-starter

## 背景知识

* [Quick Start](./docs/quick-start.md)
* [Electron基础 - 解决无法使用jQuery/RequireJS/Meteor/AngularJS 的问题](./docs/issue01.md)

## 先决条件 —— 安装electron
npm install -g electron-prebuilt  

## 运行
electron .

## 发布
发布32位exe应用程序：npm run package32
发布64位exe应用程序：npm run package




## 使用Electron Hybird架构

使用Electron Hybird架构嵌入本Web App的时候，需要对项目下/src/index.html文件进行少许改进。要在页面head节内增加：  

<!--这里用来适配Electron : begin  -->
    <script>
        window.nodeRequire = require;
        delete window.require;
        delete window.exports;
        delete window.module;
    </script>
<!--这里用来适配Electron : end  -->

## 嵌入自己不能更改的页面（因为原因始Electron默认启用了Node.js的require模块等问题无法解决）

使用[./index_with_iframe.html](./index_with_iframe.html)页面，在iframe的src里面植入链接，并将main.js中的启动页面设置成该页面，如此可以解决。


