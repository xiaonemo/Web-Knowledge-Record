# switchBtn使用文档

- 因switch插件无法设置回调函数，所以开发此插件
- 此插件是参照switch插件的样式重新开发的模块插件

## 一、普通用法

### html
``` html
  <!-- 打开状态 -->
    <input class="switchBtn" checked status="on" type="switch" data-params='{"id":"{{d.id}}","phone":"{{d.phone}}"}' on-text="冻结" off-text="解冻" />
  <!-- 关闭状态 -->
    <input class="switchBtn" status="off" type="switch" data-params='{"id":"{{d.id}}","phone":"{{d.phone}}"}' on-text="冻结" off-text="解冻" />
```

- 异步操作的按钮，需添加```data-params```属性, 没有```data-params```不会执行异步函数
- ```data-params```属性可以不设置值，没有值回调函数会按照无参数处理
- ```data-params```属性设置值的时候，必须是json数据格式的文本，普通文本会报JSON.parse失败

### 在layui表格渲染结束的done方法中执行init()方法，并在init()方法中设置回调函数
``` javascript
  // 渲染表格
  var tableResult = table.render({
    elem: '#' + ItcUser.tableId,
    url: LayUtil.ctxPath + '/itc/user/list',
    page: true,
    height: "full-158",
    cellMinWidth: 100,
    cols: ItcUser.initColumn(),
    done: function(res, curr, count){
      switchBtn.init({
        callback: function (params,checked) {
          var isSuccess;
          if (checked) {
            isSuccess = ItcUser.onUnfreeze(params);
          } else {
            isSuccess = ItcUser.onFreeze(params);
          }
          return isSuccess;
        }
      });
    }
  });
```

