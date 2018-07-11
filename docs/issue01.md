## Electron基础 - 解决无法使用jQuery/RequireJS/Meteor/AngularJS 的问题


jQuery等新版本的框架，在Electron中使用普通的引入的办法会引发异常，原因始Electron默认启用了Node.js的require模块，而这些框架为了支持commondJS标准，当Window中存在require时，会启用模块引入的方式。以下是几种解决办法：
1. 去掉框架中的模块引入判断代码，比如jQuery中的第一行代码中的：
```
!function(a,b)
{
    "object"==typeof module&&"object"==typeof module.exports?       module.exports=a.document?b(a,!0):function(a)
    {
        if(!a.document)
        throw new Error("jQuery requires a window with a document");
        return b(a)}:b(a)
    }
}

```


改成：


```
!function(a,b){b(a)}
```
以上方法只适用于jQuery框架，其它框架需要自行研究其模块结构，删除相应代码即可。

2. 使用Electron官方论坛提供的方法，需要改变require的写法，此方法各个框架通用:
```
<head>
<script>
window.nodeRequire = require;
delete window.require;
delete window.exports;
delete window.module;
</script>
<script type="text/javascript" src="jquery.js"></script>
</head>
```

如果你是刚开始开发Electron桌面端程序，建议使用第二种方法，注意引入习惯即可。如果已经开发了一定量的，不推荐使用第二种用法。如果你不想使用Node.js模块，大可以去掉require模块化引入，直接使用以下方法禁用Node.js的require模块化引入，即可正常使用任何框架

```
// In the main process.
let win = new BrowserWindow({
  webPreferences: {
    nodeIntegration: false
  }
});
Electron
```