# Mithril
[Mithril](https://mithril.js.org/index.html)中文文档翻译、学习.

## 介绍

### 什么是Mithril？
> Mithril是一个用于构建单页面应用程序的现代客户端JavaScript框架。它很小(< 10kb gzip)，速度快，并且提供开箱即用的路由和网络实用程序。
>
|                文件大小               |   性能    |
|:-------------------------------------------:|:----------------:|
|               Mithril (9.5kb)               | Mithril (6.4ms)  |
|   Vue + Vue-Router + Vuex + fetch (40kb)    |   Vue (9.8ms)    |
| React + React-Router + Redux + fetch (64kb) |  React (12.1ms)  |
|               Angular (135kb)               | Angular (11.5ms) |

> Mithril被Vimeo和Nike等公司使用，也被Lichess等开源平台使用。   
> 
> 如果您是一个有经验的开发人员，并且想知道Mithril与其他框架的比较，请参阅`框架比较`页面.    
> Mithril支持IE11, Firefox ESR，以及最后两个版本的Firefox, Edge, Safari和Chrome。不需要polyfills.   
> 寻找v1文档?请点击 :[Click here](https://mithril.js.org/archive/v1.1.7/index.html).

### 开始学习
> 尝试使用Mithril的一个简单方法是从CDN中包含它，并遵循本教程。它将覆盖大部分API列表(包括路由和网络)，而且你学习这些只需要10分钟。    
> 让我们创建一个HTML文件:`demo1.html`(以下所有示例代码在code文件夹下)

>   
```
<body>
    <script src="https://unpkg.com/mithril/mithril.js"></script>
    <script>
    var root = document.body

    // your code goes here!
    </script>
</body>
```
> 为了让事情变得更简单，你可以把这支已经装载了mithril最新版本的笔分叉。 
> (原文：To make things simpler you can fork this pen which already has the latest version of mithril loaded.) 

> 
```
// js
var root = document.body
m.render(root,'Hello World!')

// result
Hello World!
```
> Mithril也已经加载到这个页面了，所以如果你愿意，你可以马上在开发控制台中查看m对象.

### Hello World
> 为了更好的学习，现在就让我们将下面代码copy到你新建的html文件中:`demo2.html`。  

```
var root = document.body
m.render(root, "Hello world")
```

> 然后，我们接着改变render的文本，查看页面的显示。  

```
m.render(root, "My first app")
```

> 如你所见，你使用相同的代码创建和更新HTML。 Mithril会自动找出更新文本的最有效方法，而不是从头开始盲目地重新创建它。    

### DOM元素
> 让我们将文本包装在h1标签中：`demo3.html`。  

```
m.render(root, m("h1", "My first app"))
```

> m()函数的作用是:可以描述任何你想要的HTML结构。所以如果你需要向h1元素中添加一个类，你可以这么写：  

 ```
m("h1", {class: "title"}, "My first app")
```
> 如果你想要多个元素:   

```
[
    m("h1", {class: "title"}, "My first app"),
    m("button", "A button"),
]
```

> 或者：    

 ```
m("main", [
    m("h1", {class: "title"}, "My first app"),
    m("button", "A button"),
])
```
> 注意:如果您喜欢`html`语法，`可以通过Babel插件使用它`  
>
```
// HTML syntax via Babel's JSX plugin
<main>
    <h1 class="title">My first app</h1>
    <button>A button</button>
</main>
```

### 组件
> Mithril组件只是一个使用`view`函数的对象。下面是一个组件代码示例:`demo4.html`： 
 
 ```
var Hello = {
    view: function(){
        return m('main',[
            m('h1',{class:'title'},'My first app'),
            m('button','A button')
        ])
    }
}
```
> 接着，我们使用`m.mount`激活这个组件:  
>
 ```
m.mount(root,Hello);
```
> m.mount函数类似于m.render函数，但不仅仅是渲染一些HTML，它激活了Mithril的自动重绘系统。为了理解这意味着什么，让我们添加一些事件: `demo5.html`：
>
 ```
var count = 0;
var Hello = {
    view :function() {
        return m("main", [
            m("h1", {class: "title"}, "My first app"),
            // changed the next line
            m("button", {onclick: function() {count++}}, count + " clicks"),
        ])
    }
}

m.mount(root, Hello)
```

> 我们在按钮上定义了一个onclick事件，该事件增加了变量计数（该变量在顶部声明）。上面的例子中，我们还在按钮标签中呈现了该变量的值。   
> 现在，您可以通过单击按钮来更新按钮的文本内容。 由于我们使用了m.mount，因此您无需手动调用m.render即可将count变量中的更改应用于HTML； Mithril已经为你做好了这些。   
> 在性能方面，Mithril在渲染更新方面是非常快的，因为它只涉及DOM中它绝对需要的部分。在我们上面的例子中，当你点击按钮时，里面的文本是DOM Mithril唯一更新的部分。   

### 路由
> 路由只是意味着在具有多个页面的应用程序中从一个页面转到另一个页面。    
> 让我们添加一个出现在单击计数器之前的启动页面。首先，我们为它创建一个组件：`demo6.html`:       
> 
 ```
var Splash = {
    view: function() {
        return m("a", {href: "#!/hello"}, "Enter!")
    }
}
```
> 如你所见，此组件只是呈现到#!/hello的链接。#!部分称为哈希值，它是在单页面应用程序中使用的一种常见约定，用于指示其后面的内容(/hello部分)是一个路由路径。    
> 现在我们将有多个页面，我们使用m.route替代m.mount:`demo7.html`:    
 ```
m.route(root, "/splash", {
    "/splash": Splash,
    "/hello": Hello,
})
```
> m.route函数仍然具有与m.mount相同的自动重绘功能，并且还启用了URL感知功能。 换句话说，它让Mithril在网页地址栏中看到＃时知道该怎么办！    
> 根目录后面的“/ splash”表示这是默认路由，即如​​果URL中的哈希值未指向已定义的路由之一（在我们的示例中为/splash和/hello），则Mithril会重定向到默认路由 。 因此，如果您在浏览器中打开页面，并且URL为https：// localhost，那么您将被重定向到https：// localhost /＃!/splash.   
> 同样，正如你所期望的那样，单击启动页面上的链接会将您带到我们之前创建的点击计数器屏幕。 请注意，现在您的URL将指向https：// localhost /＃!/hello。 您可以使用浏览器的后退和下一步按钮来回导航至初始页面。

### 网络(XHR)
> 基本上，XHR只是与服务器对话的一种方式。   
> 让我们更改点击计数器，使其将数据保存在服务器上。 对于服务器，我们将使用REM，这是一种针对本教程等基础示例应用程序设计的模拟REST API。  
> 首先，我们创建一个调用m.request的函数。 url指定代表请求的资源端点，`method`指定我们正在执行的操作的类型（通常是PUT方法upserts），`body`是我们要发送到端点的有效负载，`withCredentials`表示启用cookie（要求 以使REM API正常工作)。代码如下：`demo8.html`:  

```
var count = 0
var increment = function() {
    m.request({
        method:"PUT",
        url: "//rem-rest-api.herokuapp.com/api/tutorial/1",
        body: {count:count + 1 },
        withCredentials: true,
    })
    .then(function(data) {
        count = parseInt(data.count)
    })
}
```
> 调用`increment`函数会将对象{count：1}向/api/tutorial/1发送数据。该接口会返回一个对象，该对象具有发送给它的相同计数值。 请注意，count变量仅在请求完成后才更新，并且及时使用服务器的响应值进行更新。    
> 让我们替换组件中的事件处理程序以调用`increment`函数，而不是直接递增count变量: 

```
var Hello = {
    view: function() {
        return m("main", [
            m("h1", {class: "title"}, "My first app"),
            m("button", {onclick: increment}, count + " clicks"),
        ])
    }
}
```
> 单击按钮现在应该更新计数了。

> 我们介绍了如何创建和更新HTML，如何创建组件，单页应用程序的路由以及如何通过XHR与服务器交互.    
> 这应该足以让您开始为实际应用程序编写前端。 如果你对Mithril API的基础知识感到满意，请确保阅读简单的应用程序教程，该教程将引导您完成构建实际的应用程序。

License: MIT. © Leo Horie.

## 安装

### CDN
> 如果你是一个JavaScript新手或者你只想要在一个简单的页面中使用Mithril，你可以从CDN中使用：  
>

```
<script src="https://unpkg.com/mithril/mithril.js"></script>
```

### npm
> 

 ```
npm install mithril --save
```
> TypeScript类型定义的可以从DefinitelyTyped获得。安装方式：
>

 ```
npm install @types/mithril --save-dev
```
> 示例用法，文档问题或讨论TypeScript相关主题你可以访问：https://github.com/MithrilJS/mithril.d.ts