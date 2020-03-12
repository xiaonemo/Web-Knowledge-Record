# CSS知识整理

## CSS选择器的种类

### 内联选择器

``` html
<div style="color:red;">内联选择器</div>
```

### ID选择器

- ID选择器用"#"定义
- id属性不能以数字开头
- 一个html只能有一个id

``` html
<div id="name">ID选择器</div>
<style>
  #id {
    color: red;
  }
</style>
```

### class选择器

- ID选择器用"."定义
- class属性不能以数字开头

``` html
<div class="name">class选择器</div>
<style>
  .id {
    color: red;
  }
</style>
```

### 属性选择器

- [name] -> 普通匹配
- [name="value"] -> 属性和值匹配

``` html
<input type="text" value="input值" />
<input type="button" value="按钮" />
<style>
  [type] {
    color: red;
  }
  [type=text] {
    color: #333;
    height: 22px;
    line-height: 22px;
    padding-left: 5px;
  }
</style>
```

- [name*="value"] -> 模糊字符匹配
- [name~="value"] -> 模糊值匹配
- [name*="value"] 的匹配范围比 [name~="value"]广

``` html
<p> "~="匹配成功、"*="匹配成功 </p>
<input type="text" title="input name" value="input值" />
<input type="text" title="text name" value="input值" />

<p> "*="匹配成功 </p>
<input type="text" title="inputname" value="input值" />
<input type="text" title="textname" value="input值" />

<p> 匹配失败 </p>
<input type="text" title="other" value="input值" />

<style>
  [type] {
    color: red;
  }
  [type=text] {
    color: #333;
    height: 22px;
    line-height: 22px;
    padding-left: 5px;
  }
  [title*="name"] {
    color: #f0f;
    height: 22px;
    line-height: 22px;
    padding-left: 5px;
  }
  [title~="name"] {
    color: red;
    height: 22px;
    line-height: 22px;
    padding-left: 5px;
  } 
</style>
```

- [name|="value"] -> "-"分隔并匹配"开头"的字符串
- [name^="value"] -> 匹配"开头"的字符串
- [name$="value"] -> 匹配"结尾"的字符串
- [name^="value"] 的匹配范围比 [name!="value"]广

``` html
<p> "|="匹配成功、 "^="匹配成功 </p>
<input type="text" title="en-input" value="input值" />
<input type="text" title="en-text" value="input值" />

<p> "^="匹配成功 </p>
<input type="text" title="entext" value="input值" />

<p> "$="匹配成功 </p>
<input type="text" title="texten" value="input值" />
<input type="text" title="other-en" value="input值" />

<p> 匹配失败 </p>
<input type="text" title="other" value="input值" />

<style>
  [type] {
    color: red;
  }
  [type=text] {
    color: #333;
    height: 22px;
    line-height: 22px;
    padding-left: 5px;
  }
  [title^="en"] { color:#0f0; }
  [title$="en"] { color:#0ff; }
  [title|="en"] { color:blue; }
</style>
```

### 伪类伪元素选择器

":"表示伪类或者伪元素，下面列出几个常见的伪类

|	选择器	|	示例	|	示例说明	|
|--	|--	|--	|
|	:hover	|	a:hover	|	鼠标移上a链接的样式	|
|	:link	|	a:link	|	未访问a链接的样式	|
|	:visited	|	a:visited	|	已访问a链接的样式	|
|	:active	|	a:active	|	已选中a链接的样式	|
|	:checked	|	input:checked	|	选择所有选中的表单元素	|
|	:disabled	|	input:disabled	|	选择所有禁用的表单元素	|
|	:focus	|	input:focus	|	获取焦点的表单样式	|
|	:before	|	p:before	|	在每个p元素之前插入内容	|
|	:after	|	p:after	|	在每个p元素之后插入内容	|
|	:lang(language)	|	p:lang(en)	|	选择所有lang属性值为en的p元素	|
|	:first-letter	|	p:first-letter	|	选择每个p元素的第一个字母	|

### 元素选择器

- div -> 普通元素选择
- div:hover -> 元素伪类选择
- div[title] -> 元素属性选择

### 关系选择器

|	选择器	|	类型	|	说明	|
|--	|--	|--	|
|	div div	|	后代选择	|	div下所有为div的后代元素	|
|	div>div	|	子选择	|	div下所有为div的子元素	|
|	div,p	|	并列选择	|	所有的div和p元素	|
|	div+p	|	紧连选择	|	div元素后的第一个p元素	|
|	div~p	|	后选择	|	div元素后的所有的p元素	|

### 通配符选择器

通配符选择器最简单，用*表示所有元素
- "*" -> 所有元素
- "div *" -> div下的所有元素
- "div>*" -> div下的所有子元素
- "* div ->" 所有元素下的div，这个和直接用div表示一样

## 优先级

!important > 内联样式 > ID选择器 > 类选择器/属性选择器/伪类选择器 > 元素选择器/关系选择器/伪元素选择器 > 通配符选择器

## BFC

### 什么是BFC
块级格式化上下文 (Block Formatting Context)是Web页面中盒模型布局的CSS渲染模式，简称BFC。它的定位体系属于常规文档流。摘自W3C：

##### 浮动，绝对定位元素，inline-blocks, table-cells, table-captions,和overflow的值不为visible的元素，（除了这个值已经被传到了视口的时候）将创建一个新的块级格式化上下文。

上面的引述几乎总结了一个BFC是怎样形成的。让我们以另一种方式来重新定义以便能更好的去理解。一个BFC是一个HTML盒子并且至少满足下列条件中的任何一个：

- float的值不为none
- position的值不为static或者relative
- display的值为 table-cell, table-caption, inline-block, flex, 或者 inline-flex中的其中一个
- overflow的值不为visible

### 触发条件

- float的值不为none
- position的值不为static或者relative
- display的值为 table-cell, table-caption, inline-block, flex, 或者 inline-flex中的其中一个
- overflow的值不为visible

### 能解决的问题

- 垂直外边距重叠问题
- 清除浮动
- 自适应两列(或多列)布局

