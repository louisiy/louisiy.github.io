---
title: Hexo主题开发
date: 2023-02-08 21:08:04
tags: hexo
---

## 主题开发

由于之前采用的white主题的开发者已将开源主题归档跑路不再维护，并且相较于本人在其他地方所看到的个人博客还有很多可以修改的可能，故决定在此基础之上进行修改，最后做出另一款自用的主题

在正式修改开发前需要先学习hexo主题的制作知识，以下综述总结于网络

### 预备

针对Hexo主题的开发需要了解

- HTML/CSS/JavaScript

- 模板引擎语法，如EJS/Jade/Swig
- CSS预处理器，如SASS/LESS/Stylus
- YML语法
- Hexo文档
  - Hexo | 变量
  - Hexo | 辅助函数


#### 主题基本结构

一般来说hexo的主题需要有以下页面：

- 首页 `index`
- 存档页 `archive`
- 标签文章列表页 `tag`
- 分类文章列表页 `category`
- 文章详情页 `post`
- 页面详情页 `page`

这些文件是Hexo在生成HTML文件时要用到的，全部放在`layout`文件夹中。这些页面内重复的组件代码，如页头页脚的部分，可以单独提取出来进行复用

此外，还有JS/CSS/图片/favicon.ico等文件直接放入source文件夹里，不需要页面引用

#### 主题文件夹结构

```
├─languages			//多语言文件夹
├─layout			//主题布局模板
│  ├─layout.ejs
│  └─_partial		//各页面共享的模板部分
├─scripts			//hexo脚本插件目录，可以编写一些辅助函数脚本
├─source			//资源文件目录，包括页面样式，js脚本等
│  ├─css
│  │ └─index.styl
│  └─js
└─_config.yml		//主题配置文件
```

#### 应用主题

修改**站点配置文件**中的主题配置，使用主题：

```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: theme-example
```

### 实操

#### HTML框架

```
├─layout
│  ├─layout.ejs		//通用的布局文件模板
│  ├─index.ejs		//继承layout.ejs布局模板
│  └─_partial
│    ├─head.ejs
│    ├─header.ejs
│    └─footer.ejs
```

`_config.yml`:

```yaml
menu:
  home: /
  categories: /categories
  tags: /tags
  archives: /archives
```

`layout/layout.ejs`:

```ejs
<!DOCTYPE html>
<html>
<%- partial('_partial/head') %>				//引入head.ejs
<body>
    <div class="container">
    <%- partial('_partial/header') %>		//引入header.ejs
    <%- body %>							   //新增的ejs文件将内容填充在这里
    <%- partial('_partial/footer') %>  		//引入footer.ejs
    </div>
</body>
</html> 
```

> partial()函数的作用是可以引入其他模板文件，详情参考hexo文档

`layout/_partial/head.ejs`:

```ejs
<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8">
    <meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">
    <title><%= config.title %></title>		//config全局变量，包含站点配置
</head>
```

`layout/_partial/header.ejs`：

```ejs
<header class="header">
    <div class="title">
        <a href="<%= url_for() %>" class="logo"><%= config.title %></a>
    </div>
    <nav class="navbar">
        <ul class="menu">
            <% for (name in theme.menu) { %>
            //theme.menu获取theme_config中导航菜单的设置
            <li class="menu-item">
                <a href="<%- url_for(theme.menu[name]) %>" class="menu-item-link"><%= name %></a>
            </li>
            <% } %>
        </ul>
    </nav>
</header>
```

`layout/_partial/footer.ejs`：

```ejs
<footer>
    <p>Theme is <a href="/" target="_blank">Theme-example</a> by <a href="<%= config.url %>" target="_blank"><%= config.author %></a></p>
    <p>Powered by <a href="https://hexo.io/" target="_blank" rel="nofollow">hexo</a> &copy; <%- date(Date.now(), 'YYYY') %></p>
</footer>
```

`index.ejs` ：

```ejs
//变量page会根据不同的页面拥有不同的属性，page变量的posts属性可以拿到文章数据的集合
//这个posts属性需要在layout的文件夹里新建post.ejs的文件才能获得对象的属性
<section class="posts">
    <% page.posts.each(function (post) { %>		//显示文章列表
    <article class="post">
        <div class="post-title">
            <a class="post-title-link" href="<%- url_for(post.path) %>"><%= post.title %></a>
        </div>
        <div class="post-content">
            <%- post.content %>  //显示文章的全部内容
        </div>
        或者：
        <div class="post-content">
    		   <%- post.excerpt %>
    //excerpt属性可获取文章的摘录部分，即文章中<!--more-->标记前的内容
    //如果未标记，那么post.excerpt会是空的
		  </div>
        <div class="post-meta">
            <span class="post-time"><%- date(post.date, "YYYY-MM-DD") %></span>
        	//
        </div>
    </article>
    <% }) %>
</section>
```

#### CSS样式

Hexo提供 `hexo-renderer-stylus` 插件，只需要将样式文件放到 `source/css` 文件夹中。Hexo 在生成页面的时候会将 `source` 中的所有文件复制到生成的 `public` 文件中，并且在此之前会编译 `styl` 为 `css` 文件

在 `css` 文件夹中创建 `style.styl`，编写一些基础的样式，并把所有样式 `import` 到这个文件。所以最终编译之后只会有 `style.css` 一个文件。创建 `_partial/header.styl` 与 `_partial/post.styl` 存放页面导航以及文章的样式，并且在 `style.styl` 中 `import` 这两个文件

`_partial/header.styl`：

```ejs
.header {
    margin-top: 2em
    display: flex
    align-items: baseline
    justify-content: space-between

    .blog-title .logo {
        color: #AAA;
        font-size: 2em;
        font-family: "Comic Sans MS",cursive,LiSu,sans-serif;
        text-decoration: none;
    }

    .menu {
        margin: 0;
        padding: 0;

        .menu-item {
            display: inline-block;
            margin-right: 10px;
        }

        .menu-item-link {
            color: #AAA;
            text-decoration: none;

            &:hover {
                color: #368CCB;
            }
        }
    }
}
```

`index.styl`:

```ejs
.post {
    margin: 1em auto;
    padding: 30px 50px;
    background-color: #fff;
    border: 1px solid #ddd;
    box-shadow: 0 0 2px #ddd;
}

.posts  {
    .post:first-child {
        margin-top: 0;
    }

    .post-title {
        font-size: 1.5em;

        .post-title-link {
            color: #368CCB;
            text-decoration: none;
        }
    }

    .post-content {
        a {
            color: #368CCB;
            text-decoration: none;
        }
    }

    .post-meta {
        color: #BABABA;
    }
}
```

`style.styl`:

```ejs
body {
    background-color: #F2F2F2;
    font-size: 1.25rem;
    line-height: 1.5;
}

.container {
    max-width: 960px;
    margin: 0 auto;
}

@import "_partial/header";
@import "_partial/index";
```

#### 添加分页

在站点的 `source/_post/` 目录下存放的是我们的文章，现在我们把原本的 `hello-world.md` 复制黏贴 10+ 次，再查看站点首页。会发现，首页只显示了 10 篇文章。

首页显示的文章数量我们可以通过站点配置文件中的 `per_page` 字段来修改，但是我们不可能把所有文章都放在一页，所以我们现在来添加文章列表的分页。

新建 `_partial/paginator.ejs`

```ejs
<% if (page.total > 1){ %>
    <nav class="page-nav">
    <%- paginator({
        prev_text: "&laquo; Prev",
        next_text: "Next &raquo;"
    }) %>
    </nav>
<% } %>
```

在 `index.ejs` 中添加这个文件的内容：

```ejs
...
</section>
<%- partial('_partial/paginator') %>
```

这里我们使用到了另外的一个辅助函数 `paginator`，它能够帮助我们插入分页链接，这里我们只是实现了一个最基本的分页，具体的样式可以自行添加，或者根据文档使用其他配置自定义分页

#### 添加文章详情页

文章详情页对应的布局文件是 `post.ejs`，新建 `post.ejs`:

```ejs
<article class="post">
    <div class="post-title">
        <h2 class="title"><%= page.title %></h2>
    </div>
    <div class="post-meta">
        <span class="post-time"><%- date(page.date, "YYYY-MM-DD") %></span>
    </div>
    <div class="post-content">
        <%- page.content %>
    </div>
</article>
```

#### 添加归档页

创建归档页使用的模板文件 `archive.ejs`:

```ejs
<section class="archive">
    <ul class="post-archive">
    <% page.posts.each(function (post) { %>
        <li class="post-item">
            <span class="post-date"><%= date(post.date, "YYYY-MM-DD") %></span>
            <a class="post-title" href="<%- url_for(post.path) %>"><%= post.title %></a>
        </li>
    <% }) %>
    </ul>
</section>
<%- partial('_partial/paginator') %>
```

其实结构跟首页差不多，只是不显示文章内容而已。添加归档页的样式：

`css/_partial/archive.styl`:

```ejs
.archive {
    margin: 1em auto;
    padding: 30px 50px;
    background-color: #fff;
    border: 1px solid #ddd;
    box-shadow: 0 0 2px #ddd;

    .post-archive {
        list-style: none;
        padding: 0;

        .post-item {
            margin: 5px 0;

            .post-date {
                display: inline-block;
                margin-right: 10px;
                color: #BABABA;
            }

            .post-title {
                color: #368CCB;
                text-decoration: none;
            }
        }
    }
}
```

#### 添加分类页

分类页跟归档页也类似，相当于根据不同的分类进行归档。

分类页和标签页的模板编写比较特殊，本质上，分类页和标签页属于自定义页面，我们需要新建自定义页面模板`page.ejs`

```ejs
<% if (is_current(theme.menu.categories)) { %>
<%- partial('_partial/category') %>
<% } else if (is_current(theme.menu.tags)) { %>
<%- partial('_partial/tag') %>
<% } else { %>
<%- partial('_partial/custom') %>
<% } %>
```

这里，我们需要根据当前自定义页面的类型来决定渲染何种自定义页面模板。

新建`_partial/category.ejs`:

```ejs
<section class="archive">
    <ul class="post-archive">
    <% site.categories.each(function (category) { %>
        <span><%= category.name %></span>
        <% category.posts.forEach(function(post) { %>
        <li class="post-item">
            <span class="post-date"><%= date(post.date, "YYYY-MM-DD") %></span>
            <a class="post-title" href="<%- url_for(post.path) %>"><%= post.title %></a>
        </li>
        <% }) %>
    <% }) %>
    </ul>
</section>
```

`site.categories`包括了站点所有的分类信息，可以遍历获取分类信息，其中`category.posts`又包含了该分类的所有文章信息。

需要注意的是，要想在页面中展示分类页，需要先执行`hexo new page categories`生成分类页面，并添加`type`为`categories`：

```javascript
title: categories
date: 2019-02-25 18:19:55
type: "categories"
---
```

后续的标签页类似。

#### 添加标签页

标签页与分类页及其类似，只是根据标签进行归档。

新建`_partial/tag.ejs`:

```ejs
<section class="archive">
    <ul class="post-archive">
    <% site.tags.each(function (tag) { %>
        <span><%= tag.name %></span>
        <% tag.posts.forEach(function(post) { %>
        <li class="post-item">
            <span class="post-date"><%= date(post.date, "YYYY-MM-DD") %></span>
            <a class="post-title" href="<%- url_for(post.path) %>"><%= post.title %></a>
        </li>
        <% }) %>
    <% }) %>
    </ul>
</section>
```

#### 添加自定义页面

自定义页面与文章详情页类似

`_partial/custom.ejs`:

```ejs
<article class="post">
    <div class="post-title">
        <h2 class="title"><%= page.title %></h2>
    </div>
    <div class="post-meta">
        <span class="post-time"><%- date(page.date, "YYYY-MM-DD") %></span>
    </div>
    <div class="post-content">
        <%- page.content %>
    </div>
</article>
```

我们在主题配置文件中添加自定义页面的菜单：

```yaml
menu:
  home: /
  categories: /categories
  tags: /tags
  archives: /archivesz
  about: /about
```

执行`hexo new page about`进行手动生成页面，编辑文件内容即可

#### 多语言支持

Hexo 支持多语言显示，在主题的 `languages` 文件夹中，存放具体的多语言文件，可以是 YML 或者 JSON 文件。再在主配置文件 `_config.yml` 中使用下面的方法来指定具体的使用的配置文件名：

```yaml
language: zh-CN
# 或者多个配置文件
language:
 - zh-CN
 - en
```

像下面这样组织语言文件，`languages/en.yml`：

```yaml
archive_title: Archives
category_title: Category
tag_title: Tag
```

在模板里，当需要在页面中显示文字时，可以使用 Hexo 提供的帮助函数 `__()` / `_p()` 来读取具体的值，如：

```ejs
{% if is_archive() %}
  {% set pageTitle = _p('archive_title') %}
  {% endif %}

   page_title 
```

### Hexo插件

Hexo 有强大的插件系统，让我们能够轻松扩展功能而不用修改核心模块的源码。在 Hexo 中有两种形式的插件：

- 脚本（Scripts）
- 插件（Packages）

如果我们的代码很简单，我们可以编写脚本，只需要把 JavaScript 文件放到 `scripts` 文件夹，在启动时就会自动载入。简单来说，脚本文件可以相当于一些这样的的工具函数，当我们发现Hexo官方提供的函数不能满足我们的需求时，我们可以通过添加一个脚本来实现。

比如，我们现在有这样一个简单的需求，我们想给首页文章列表中的文章块添加一个背景颜色，背景颜色我们可以在文章md文件中定义，如果未定义，则随机选用一种颜色。

首先，我们先在文章md文件中顶部Front-matter添加一个`color`字段：

`_posts/hello-world-1.md`:

```javascript
title: Hello World 1
date: 2019-02-12 17:49:32
categories: 分类1
tags: 
    - 标签1
color: blue
---
```

复制

定义完成后，我们就可以在文章信息字段`post`或者`page`中获取到`color`。

然后，我们需要添加一个脚本函数来根据`color`字段来获取文章块的背景颜色，新增`scripts/getPostBgColor.js`:

```javascript
const arr = [ 'blue', 'purple', 'green', 'yellow', 'red', 'orange' ];

var getPostBgColor = function(color) {
  if (arr.indexOf(color) >= 0) {
    return `bg-${color}`;
  }
  return 'bg-' + randBgColor();
};

function randBgColor() {
  return arr[randomInt(0, 5)];
}

function randomInt(min, max) {
  return Math.round(Math.random() * (max - min)) + min;
}

hexo.extend.helper.register('getPostBgColor', getPostBgColor);
```

最后一行我们通过`hexo.extend.helper.register`全局注册一个脚本函数，注册完成后，我们就可以在模板文件中和Hexo官方提供的全局函数一样使用。

修改`layout/index.ejs`:

```ejs
...
<article class="post <%= getPostBgColor(post.color) %>">
...
```

添加背景颜色样式，编辑`css/index.styl`:

```stylus
...
.bg-blue {
    background-color: #6fa3ef;
}
.bg-purple {
    background-color: #bc99c4;
}
.bg-green {
    background-color: #46c47c;
}
.bg-yellow {
    background-color: #f9bb3c;
}
.bg-red {
    background-color: #e8583d;
}
.bg-orange {
    background-color: #f68e5f;
}
```

看下效果，Hello World 1这篇文章我们定义了`color：blue`，因此是蓝色，其他文章，我们未定义`color`，因此是随机颜色。

这样，我们就实现了随机背景色这个小需求，当然这只是一个非常简单的例子，如果有其他复杂的需求，我们可以通过编写更加复杂的脚本来实现

### Hexo的数据DB扩展查询

我们已经知道，Hexo已经为我们预先义了很多常用的变量供我们使用，具体可以在 Hexo | 变量 查询。但是如果系统提供的变量数据不能满足我们的要求，那我们该怎么办呢？其实我们可以通过扩展查询来获取到我们期望的数据。

其实Hexo所有的文章分类标签等等变量信息，在编译成本地静态文件之前，都是本地存储在一个`db.json`中的，相当于小型的本地[数据库](https://cloud.tencent.com/solution/database?from=10680)，Hexo在运行阶段，所有的数据相关操作其实都是在这个小型数据库上进行操作，其底层使用的查询引擎就是Warehouse。因此我们可以通过Warehouse的语法进行自定义扩展查询。

比如我们需要在页面的底部展示全站的最近6篇文章列表，由于Hexo首页只提供了第一页的数据，因此我们可以基于`site`变量进行扩展查询：

```ejs
site.posts.sort({date: -1}).limit(6)
```

`site.posts`表示所有的文章，`sort（{date: -1})`表示按创建时间倒序排列，`limit(6)`表示只取前6条数据，这样我们就可以拿到了全站的最近6文章信息，后续进行相应展示操作即可。

其他更多复杂的扩展查询都可以根据Warehouse语法文档进行按需扩展。

## 总结

其实说白了，Hexo就是把那些 Markdown 文件，按照我们编写的对应布局模板，填上对应的数据生成 HTML 页面，然后在编译的过程中将JS/CSS等文件引入HTML，然后生成每个页面的对应HMTL静态文件。

而Hexo主题的作用就是决定每个布局模板长什么样