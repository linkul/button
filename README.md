handlebars介绍
=======

Handlebars是JavaScript一个语义模板库，通过对view和data的分离来快速构建Web模板。它采用”Logic-less template“（无逻辑模版）的思路，在加载时被预编译，而不是到了客户端执行到代码时再去编译，这样可以保证模板加载和运行的速度

基本语法
====================

使用方法是加两个花括号{{value}}, handlebars模板会自动匹配相应的数值

例如:

handlebars模板

```
 <table class="table table-bordered">
    <thead>
        <tr>
            <th>id</th>
            <th>项目名称</th>
            <th>创建时间</th>
        </tr>
    </thead>
    <tbody>
    {{#this}}
        <tr>
            <td>{{id}}</td>
            <td>{{name}}</td>
            <td>{{createAt}}</td>
        </tr>
    {{/this}}
    </tbody>
</table>
```

json

```
[{
    "name":"kelude2",
    "id":"1",
    "createAt":"2014/02/17"
},
{
    "name":"kelude3",
    "id":"2",
    "createAt":"2014/02/18"
}]  
```

js handelbars预编译

```
;(function($){
    var template = Handlebars.compile(source); //模板文件
    $("body").append(template(data)) //返回预编译后的dom  ，data  json数据 
}(jQuery)) 

```

[在线模板调试](http://g.assets.daily.taobao.net/platform/kelude-static/2-kui/tool/tools.html)


block
====================
有时候当你需要对某条表达式进行更深入的操作时，就需要Blocks来支持，在Handlebars中，你可以在表达式后面跟随一个#号来表示Blocks，然后通过{{/表达式}}来结束Blocks。如果当前的表达式是一个数组，则Handlebars会“自动展开数组”，并将Blocks的上下文设为数组中的项目，让我们来看看下面的例子，例如：{{@index}}

```
<table class="table table-bordered">
    <thead>
        <tr>
            <th>id</th>
            <th>项目名称</th>
            <th>创建时间</th>
        </tr>
    </thead>
    <tbody>
    {{#this}}
        <tr>
            <td>{{id}}</td>
            <td>{{name}}</td>
            <td>{{createAt}}</td>
        </tr>
    {{/this}}
    </tbody>
</table>
```

```
[{
    "name":"kelude2",
    "id":"1",
    "createAt":"2014/02/17"
},
{
    "name":"kelude3",
    "id":"2",
    "createAt":"2014/02/18"
}]  
```
[在线模板调试](http://g.assets.daily.taobao.net/platform/kelude-static/2-kui/tool/tools.html)

内置block helper
====================

* each

你可以使用内置的{{#each}}{{/each}} helper遍历列表块内容。

当对象为数组的时候用*this*来引用遍历的元素，用*@index* 来引用索引

当对象是Object对象的时候可以用*@key*来引用对象的key值，*this* 来引用对象value值

可以配合{{else}}来做未空判断，输出默认元素

例子：

handlebars

```
{{#each tester}}
    <tr>
        <td>{{@index}}</td>
        <td>{{this}}</td>
    </tr>
{{else}}
    <tr>
        <td colspan="2">测试人员未配置</td>
    </tr>
{{/each}}

{{#each developer}}
    <tr>
        <td>{{@index}}</td>
        <td>{{@key}}</td>
        <td>{{this}}</td>
    </tr>
{{else}}
    <tr>
        <td colspan="2">开发人员未配置</td>
    </tr>
{{/each}}
```

json

```
{
  "tester": [
    "习木",
    "2月21号",
    "2014年"
  ],
  "developer":{
    "name":"习木",
    "birthday":"2月21号",
    "year":"2014年"
  }
}
```
[在线模板调试](http://g.assets.daily.taobao.net/platform/kelude-static/2-kui/tool/each.html)

* if/unless else

{{#if param}} {{/if}}helper 你可以指定条件渲染dom，如果它的参数返回false，undefined, null, “” 或者 [] ,Handlebar将不会渲染DOM，如果存在{{else}}则执行{{else}}后面的渲染;

unless是反向的if语法也就是当判断的值为false时他会渲染DOM

```
{{#if tester}}
    ....
{{else}}
    ....
{{/if}}

{{#unless tester}}
    ....
{{else}}
    ....
{{/unless}}
```

[在线模板调试](http://g.assets.daily.taobao.net/platform/kelude-static/2-kui/tool/if.html)

* with 

Handlebars模板会在编译的阶段的时候进行context传递和赋值。使用with的方法，我们可以将context转移到数据的一个section里面（如果你的数据包含section）。这个方法在操作复杂的template时候非常有用。

handlebars

```
{{#with developer}}
    <tr>
        <td>{{@index}}</td>
        <td>{{@key}}</td>
        <td>{{this}}</td>
    </tr>
{{else}}
    <tr>
        <td colspan="2">开发人员未配置</td>
    </tr>
{{/with}}
```

json

```
{
  "tester": [
    "习木",
    "2月21号",
    "2014年"
  ],
  "developer":{
    "name":"习木",
    "birthday":"2月21号",
    "year":"2014年"
  }
}
```

[在线模板调试](http://g.assets.daily.taobao.net/platform/kelude-static/2-kui/tool/with.html)

自定义block helper
====================

可以从任何上下文可以访问在一个模板，你可以使用”Handlebars.registerHelper”方法来注册一个helper。有两种类型的帮手，你可以使Function helper 和block helper。

Function helper 基本上都是定时功能，一旦注册，可以在你的模板中的任何地方。Handlebars写入到模板函数的返回值

block helper 在本质上相似if each with 内置块助手，允许改变内容里的值它的辅助和辅助函数的名称作为registerHelper的参数

```
[{
    "name":"kelude2",
    "id":"1",
    "createAt":"2014/02/17"
},
{
    "name":"kelude3",
    "id":"2",
    "createAt":"2014/02/18"
},
"sub":[{
        "name":"kelude3",
        "id":"2",
        "createAt":"2014/02/18"
    },
    {
        "name":"kelude3",
        "id":"2",
        "createAt":"2014/02/18"
    },
    {
        "name":"kelude3",
        "id":"2",
        "createAt":"2014/02/18"
    }]
] 
```

handlebars模板

```
{{#this}}
    <tr>
        <td>{{id}}</td>
        <td>{{name}}</td>
        <td>{{createAt}}</td>
    </tr>
{{/this}}

{{#sub}}
    <tr>
        <td>{{id}}</td>
        <td>{{projectname name}}</td>
        <td>{{createAt}}</td>
    </tr>
{{/sub}}

{{#list sub}}
    <tr>
        <td>{{id}}</td>
        <td>{{projectname name}}</td>
        <td>{{createAt}}</td>
    </tr>
{{/list}}

```

Function helper

```
Handlebars.registerHelper("projectname", function(projectname) {
    return '<a href="#">'+projectname+'</a>'
})

```

Block helper

```
Handlebars.registerHelper("list", function(items, options) {
  var out = "<ul>";
  for(var i=0, l=items.length; i<l; i++) {
    out = out + "<li>" + options.fn(items[i]) + "</li>";
  }
  return out + "</ul>";
})

```
[在线模板调试](http://g.assets.daily.taobao.net/platform/kelude-static/2-kui/tool/custom.html)

其他知识点
====================

* **{{this}}** 数据块的当前上下文
* **{{../name}}** json块向上引用
* **{{this/name}}**，**{{this.name}}**，**{{./name}}** json块向下应用
* **Handlebars.SafeString**  输出安全的html，html标签<>会被转义
* **Handlebars.Utils.escapeExpression()** 转义特殊字符
