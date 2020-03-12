# jQuery插件封装

jQuery封装插件时有两种方法，$.extend()和$.fn。$.extend()是全局调用，$.fn是通过元素调用。

## 一、$.extend()

### 普通用法

#### 定义
``` javascript
	(function($){
	  $.extend({
	    request: function() {
        $.ajax({
          url:"demo_test.json",
          success:function(result){
            $("#div1").html(result);
          }
        });
      }
	  })
	})(jQuery);
```

#### 调用
``` javascript
	$.request();
```

### 带参用法

#### 定义
``` javascript
	(function($){
	  $.extend({
	    request: function(options) {
        $.ajax({
          type:options.type,
          url:options.url,
          data: options.data,
          headers: options.headers,
          success:function(res){
            options.success(res);
          },
          error:function(err){
            options.error(err);
          }
        });
      }
	  })
	})(jQuery);
```

#### 设置参数默认值

``` javascript
	(function($){
	  $.extend({
	    request: function(options) {
        var defaults = {
          type: 'post',
          url : '',
          data: {},
          headers: {
            "Content-Type": "application/json"
          },
        };
        var settings = $.extend(defaults, options);
        $.ajax({
          type:settings.type,
          url:settings.url,
          data: settings.data,
          headers: settings.headers,
          success:function(res){
            settings.success(res);
          },
          error:function(err){
            settings.error(err);
          }
        });
      }
	  })
	})(jQuery);
```

#### 调用
``` javascript
	$.request({
    type: 'post',
    url: 'score/get',
    data: {
      "id": id,
    },
    success: function(req){
      $("#div1").html(result);
    },
    error: function(xhr, type){
      alert("服务器错误");
    }
  });
```

## 一、$.fn

### 普通用法

#### 定义
``` javascript
	(function($){
	  $.fn.showRed = function() {
      this.css("color","red");
    }
	})(jQuery);
```

#### 调用
``` javascript
	$("div").showRed()
```

### 带参用法

#### 定义
``` javascript
	(function($){
	  $.fn.showRed = function(options) {
      this.css("color",options.color);
    }
	})(jQuery);
```

#### 调用
``` javascript
	$("div").showRed({color:"red"})
```

### 设置参数默认值

#### 定义
``` javascript
	(function($){
	  $.fn.showRed = function(options) {
      var defaults = {
        color: "red"
      }
      var settings = $.extend(defaults, options);
      this.css("color",options.color);
    }
	})(jQuery);
```

#### 调用
``` javascript
	$("div").showRed({color:"#f0f"})
```