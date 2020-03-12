# otherBtns使用文档

- 此插件，为了优化表格操作按钮过多时影响美观。
- 原理是一个按钮组容器，多余的按钮通过点击“更多”按钮，下拉出更多按钮的列表
- 还添加了，点击“更多”按钮时的回调函数

## 一、普通用法

### html
``` html
	<div class="lay-other-btns">
    <span class="layui-btn-xs lay-btn-other" >更多 <i class="layui-icon layui-icon-down"></i> </span>
    <ul class="lay-btns-box">
        <li>
            {{#  if(d.status === 1){ }}
            <a class="layui-btn layui-bg-orange layui-btn-xs" lay-event="freeze">冻&nbsp; &nbsp; 结</a>
            {{#  } else { }}
            <a class="layui-btn layui-btn layui-btn-xs" lay-event="unfreeze">解&nbsp; &nbsp; 冻</a>
            {{#  } }}
        </li>
        <li>
            @if(shiro.hasPermission("/itc/device/record")){
            <a class="layui-btn layui-btn-xs" title="{{d.name}}&#45;&#45;车辆绑定记录" tab-href="/itc/user/user_car/+{{d.id}}" >绑&nbsp; &nbsp; 定</a>
            @}
        </li>
        <li>
            @if(shiro.hasPermission("/itc/black/add")){
            <a class="layui-btn {{d.blk_id==1?'layui-bg-black':'layui-btn-disabled'}}  layui-btn-xs" lay-event="add">黑名单</a>
            @}
        </li>
        <!--
        <li>
            <a class="layui-btn layui-btn-xs" data-params='{"id":"001"}'>白名单</a>
        </li>
        -->
    </ul>
</div>
```

### 在layui表格渲染结束的done方法中执行init()方法
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
            otherBtns.init();
        }
    });
```

结束，其他无需处理什么，按钮组内的按钮和其他按钮用法一样

## 二、回调用法

### html

``` html
	<div class="lay-other-btns">
    <span class="layui-btn-xs lay-btn-other" >更多 <i class="layui-icon layui-icon-down"></i> </span>
    <ul class="lay-btns-box">
        <li>
            {{#  if(d.status === 1){ }}
            <a class="layui-btn layui-bg-orange layui-btn-xs" lay-event="freeze">冻&nbsp; &nbsp; 结</a>
            {{#  } else { }}
            <a class="layui-btn layui-btn layui-btn-xs" lay-event="unfreeze">解&nbsp; &nbsp; 冻</a>
            {{#  } }}
        </li>
        <li>
            @if(shiro.hasPermission("/itc/device/record")){
            <a class="layui-btn layui-btn-xs" title="{{d.name}}&#45;&#45;车辆绑定记录" tab-href="/itc/user/user_car/+{{d.id}}" >绑&nbsp; &nbsp; 定</a>
            @}
        </li>
        <li>
            @if(shiro.hasPermission("/itc/black/add")){
            <a class="layui-btn {{d.blk_id==1?'layui-bg-black':'layui-btn-disabled'}}  layui-btn-xs" lay-event="add">黑名单</a>
            @}
        </li>
        <li>
            <a class="layui-btn layui-btn-xs" data-params='{"id":"001"}'>白名单</a>
        </li>
    </ul>
</div>
```
- 异步操作的按钮，需添加```data-params```属性, 没有```data-params```不会执行异步函数
- ```data-params```属性可以不设置值，没有值回调函数会按照无参数处理
- ```data-params```属性设置值的时候，必须是json数据格式的文本，普通文本会报JSON.parse失败

### 在init()方法中设置回调函数

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
        otherBtns.init({
          callback: function (params) {
            var btnStatus = "show";
            $.ajax({
              url:"/test",
              type:"get",
              async:false,
              data:params,
              //请求成功
              success:function(result){
                //这里处理按钮的显示方式
                btnStatus = "show";
                //show，正常显示
                //hide，隐藏按钮
                //disabled，禁用按钮
              },
              //请求失败
              error:function (xhr,status,error) {
              },
              //请求完成
              complete:function (xhr,status) {
              }
            });
            return btnStatus;
          }
        });
      }
    });
```

```btnStatus```是按钮显示状态，有三个可选值，show(正常显示)、hide(隐藏按钮)、disabled(禁用按钮)