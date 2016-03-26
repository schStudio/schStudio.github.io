---
layout: post
title:  "Jekyll学习笔记"
date:   2016-03-25 00:56:00
categories: jekyll
---


* content
{:toc}

### 什么是Jekyll
**官方定义**

		Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过 Markdown （或者 Textile） 以及 Liquid 转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 GitHub Page 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。

---

### 基本用法

#### build(编译)

	$ jekyll build
	# => 当前文件夹中的内容将会生成到 ./site 文件夹中。

	$ jekyll build --destination <destination>
	# => 当前文件夹中的内容将会生成到目标文件夹<destination>中。

	$ jekyll build --source <source> --destination <destination>
    # => 指定源文件夹<source>中的内容将会生成到目标文件夹<destination>中。

	$ jekyll build --watch
    # => 当前文件夹中的内容将会生成到 ./site 文件夹中，
	#    查看改变，并且自动再生成。
        
#### serve(运行)

	$ jekyll serve
	# => 一个开发服务器将会运行在 http://localhost:4000/

	$ jekyll serve --detach
	# => 功能和`jekyll serve`命令相同，但是会脱离终端在后台运行。
	#    如果你想关闭服务器，可以使用`kill -9 1234`命令，"1234" 是进程号（PID）。
	#    如果你找不到进程号，那么就用`ps aux | grep jekyll`命令来查看，然后关闭服务器。

	$ jekyll serve --watch
	# => 和`jekyll serve`相同，但是会查看变更并且自动再生成。

---

### 目录结构
* `_config.yml`
* `_drafts`
* `_includes`
* `_layouts`
* `_posts`
* `_data`
* `_site`

---

### 配置

#### 全局配置
<table>
	<tr>
      	<td>说明</td>
		<td>配置</td>
	</tr>
	<tr>
		<td>源文件地址</td>
		<td>source: DIR</td>
	</tr>
	<tr>
		<td>目标文件地址</td>
		<td>destination: DIR</td>
	</tr>
	<tr>
		<td>禁用插件</td>
		<td>safe: bool</td>
	</tr>
	<tr>
		<td>编译时排除项</td>
		<td>exclude: [DIR, FILE, …]</td>
	</tr>
	<tr>
		<td>编译时包含项</td>
		<td>include: [DIR, FILE, …]</td>
	</tr>
	<tr>
		<td>时区</td>
		<td>timezone: TIMEZONE</td>
	</tr>
	<tr>
		<td>文件编码</td>
		<td>encoding: ENCODING</td>
	</tr>
</table>
    
#### 编译选项
<table>
    <tr>
    	<td>说明</td>
    	<td>配置</td>
    </tr>
    <tr>
    	<td>允许文件修改时自动重新生成网站。</td>
    	<td>-w, --watch</td>
    </tr>
    <tr>
    	<td>手动设置配置文件并替代_config.yml</td>
    	<td>--config FILE1[,FILE2,...]</td>
    </tr>
    <tr>
    	<td>处理草稿</td>
    	<td>--drafts</td>
    </tr>
    <tr>
    	<td>用将来的日期发布文章</td>
    	<td>--future</td>
    </tr>
    <tr>
    	<td>相关文章生成索引</td>
    	<td>--lsi</td>
    </tr>
    <tr>
    	<td>限制文章的数量</td>
    	<td>--limit_posts NUM</td>
    </tr>
</table>
    
#### 服务选项
<table>
    <tr>
    	<td>说明</td>
    	<td>配置</td>
    </tr>
    <tr>
    	<td>监听所给的端口</td>
    	<td>--port PORT</td>
    </tr>
    <tr>
    	<td>监听所给的主机名</td>
    	<td>--host HOSTNAME</td>
    </tr>
    <tr>
    	<td>网站的根路径</td>
    	<td>--baseurl URL</td>
    </tr>
    <tr>
    	<td>从终端命令行中分离出来</td>
   		<td>-B, --detach</td>
    </tr>
</table>

#### 默认选项

    source:      .
    destination: ./_site
    plugins:     ./_plugins
    layouts:     ./_layouts
    include:     ['.htaccess']	(注*:该文件定义了针对目录的权限规则)
    exclude:     []
    keep_files:  ['.git','.svn']
    gems:        []
    timezone:    nil
    encoding:    nil

    future:      true
    show_drafts: nil
    limit_posts: 0
    highlighter: pygments

    relative_permalinks: true

    permalink:     date
    paginate_path: 'page:num'
    paginate:      nil

    markdown:      maruku
    markdown_ext:  markdown,mkd,mkdn,md
    textile_ext:   textile

    excerpt_separator: "\n\n"

    safe:        false
    watch:       false    # deprecated
    server:      false    # deprecated
    host:        0.0.0.0
    port:        4000
    baseurl:     /
    url:         http://localhost:4000
    lsi:         false

    maruku:
        use_tex:    false
        use_divs:   false
        png_engine: blahtex
        png_dir:    images/latex
        png_url:    /images/latex
        fenced_code_blocks: true

    rdiscount:
    	extensions: []

    redcarpet:
    	extensions: []

    kramdown:
        auto_ids: true
        footnote_nr: 1
        entity_output: as_char
        toc_levels: 1..6
        smart_quotes: lsquo,rsquo,ldquo,rdquo
        use_coderay: false

    coderay:
        coderay_wrap: div
        coderay_line_numbers: inline
        coderay_line_numbers_start: 1
        coderay_tab_width: 4
        coderay_bold_every: 10
        coderay_css: style

    redcloth:
    hard_breaks: true

---

### 头信息

#### 结构

    # =>头信息写在两行三虚线之间
    ---
    layout: post
    title: Blogging Like a Hacker
    ---

#### 预定义全局变量
<table>
    <tr>
        <td>变量名</td>
        <td>说明</td>
    </tr>
    <tr>
        <td>layout</td>
        <td>布局文件, 从_layout中选择</td>
    </tr>
    <tr>
        <td>permalink</td>
        <td>文章命名格式要求</td>
    </tr>
    <tr>
        <td>published</td>
        <td>文章具体内容是否展示</td>
    </tr>
    <tr>
        <td>category/categories</td>
        <td>文章分类</td>
    </tr>
    <tr>
        <td>tags</td>
        <td>文章标签</td>
    </tr>
</table>
    
#### 自定义变量

    {% raw %}
    <!-- 头信息中未被定义的变量将在此页面中定义并使用 -->
    <!DOCTYPE HTML>
    <!DOCTYPE HTML>
    <html>
    <head>
    	<title>{{ page.title }}</title>
    </head>
    <body>
    	...
    {% endraw %}

#### 预定义局部变量

    {% raw %}
    <!-- 页面中已经定义好了局部变量 -->
    <!DOCTYPE HTML>
    <!DOCTYPE HTML>
    <html>
    <head>
    	<title>{{ page.date }}</title>
    </head>
    <body>
    	...
    {% endraw %}

---

### 写文章

#### 文章命名



#### 引用图片和其他资源

首先在网站目录下创建自己的资源文件夹, 例如assets\或者downloads\,
之后在页面中使用site.访问, 例子如下:

    ![有帮助的截图]({{ site.url }}/assets/screenshot.jpg)
    … 你可以直接 [下载 PDF]({{ site.url }}/assets/mydoc.pdf).

#### 文章目录

列举网站包含的所有文章代码如下:
    {% raw %}
    <ul>
        {% for post in site.posts %}
        <li>
        	<a href="{{ post.url }}">{{ post.title }}</a>
        </li>
        {% endfor %}
    </ul>
    {% endraw %}

#### 文章摘要

Jekyll 会自动取每篇文章从开头到第一次出现`excerpt_separator`的地方作为文章的摘要， 并将此内容保存到变量`post.excerpt`中

    {% raw %}
    <ul>
        {% for post in site.posts %}
        <li>
        	<a href="{{ post.url }}">{{ post.title }}</a>
        	{{ post.excerpt }}
        </li>
        {% endfor %}
    </ul>
    {% endraw %}
    
#### 代码片段高亮

Jekyll 自带语法高亮功能，它是由 Pygments 来实现的。在文章中插入一段高亮代码非常 容易，只需使用下面的 Liquid 标记：
    
    {% raw %}
    {% highlight ruby %}
    def show
    	@widget = Widget(params[:id])
    	respond_to do |format|
    		format.html # show.html.erb
    		format.json { render json: @widget }
    	end
    end
    {% endhighlight %}
    {% endraw %}
    
#### 显示行数

Jekyll 显示行数功能也是很简单, 代码如下所示:

	{% raw %}
    {% highlight ruby linenos %}
    def show
    	@widget = Widget(params[:id])
    	respond_to do |format|
    		format.html # show.html.erb
    		format.json { render json: @widget }
    	end
    end
    {% endhighlight %}
    {% endraw %}
---

### 写草稿

#### 草稿目录

	|-- _drafts/
	|   |-- a-draft-post.md
        
#### 草稿生成

为了预览你拥有草稿的网站，运行 `jekyll serve` 或者带有 `--drafts` 配置选项的 `jekyll build`。此两种方法皆会将 `Time.now` 的值赋予草稿文章，作为其发布日期，所以你将看到草稿文章作为最新文章被生成。

---

### 创建页面

#### 主页

`index.html`放在网站根目录下即可

#### 其他页面

将 HTML 文件放在哪里取决于你想让它们如何工作。有两种方式可以创建页面：
    
1.	命名 HTML 文件：将命名好的为页面准备的 HTML 文件放在站点的根目录下。

        .
        |-- _config.yml
        |-- _includes/
        |-- _layouts/
        |-- _posts/
        |-- _site/
        |-- about.html    # => http://example.com/about.html
        |-- index.html    # => http://example.com/
        └── contact.html  # => http://example.com/contact.html

2.	命名文件夹：在站点的根目录下为每一个页面创建一个文件夹，并把 index.html 文件放在每 个文件夹里。

        .
        ├── _config.yml
        ├── _includes/
        ├── _layouts/
        ├── _posts/
        ├── _site/
        ├── about/
        |   └── index.html  # => http://example.com/about/
        ├── contact/
        |   └── index.html  # => http://example.com/contact/
        └── index.html      # => http://example.com/


---

### 常用变量

#### 全局变量

<table>
    <tr>
        <td>变量</td>
        <td>说明</td>
    </tr>
    <tr>
        <td>site</td>
        <td>来自_config.yml文件</td>
    </tr>
    <tr>
        <td>page</td>
        <td>通过 YAML 头文件自定义的信息</td>
    </tr>
    <tr>
        <td>content</td>
        <td> Post 或者 Page 渲染生成的内容</td>
    </tr>
    <tr>
        <td>paginator</td>
        <td>paginate 配置开启后可使用,分页信息</td>
    </tr>
</table>
    
#### 全站变量

<table>
    <tr>
        <td>变量</td>
        <td>说明</td>
    </tr>
    <tr>
        <td>site.time</td>
        <td>网站启动当前时间</td>
    </tr>
    <tr>
        <td>site.pages</td>
        <td>Pages 清单</td>
    </tr>
    <tr>
        <td>site.posts</td>
        <td> Posts 清单</td>
    </tr>
    <tr>
        <td>site.related_posts</td>
        <td>相关的 Post</td>
    </tr>
    <tr>
        <td>site.categories.CATEGORY</td>
        <td>CATEGORY 类别下的帖子</td>
    </tr>
    <tr>
        <td>site.tags.TAG</td>
        <td>TAG 标签下的帖子</td>
    </tr>
    <tr>
        <td>site.[CONFIGURATION_DATA]</td>
        <td>_config.yml 设置的变量</td>
    </tr>
</table>
    
#### 页面变量

<table>
    <tr>
        <td>变量</td>
        <td>说明</td>
    </tr>
    <tr>
        <td>page.content</td>
        <td>页面内容的源码</td>
    </tr>
    <tr>
        <td>page.title</td>
        <td>页面的标题</td>
    </tr>
    <tr>
        <td>page.excerpt</td>
        <td>页面摘要的源码</td>
    </tr>
    <tr>
        <td>page.url</td>
        <td>相对路径</td>
    </tr>
    <tr>
        <td>page.date</td>
        <td>帖子的日期</td>
    </tr>
    <tr>
        <td>page.id</td>
        <td>帖子的唯一标识码</td>
    </tr>
    <tr>
        <td>page.categories</td>
        <td>这个帖子所属的 Categories</td>
    </tr>
    <tr>
        <td>page.tags</td>
        <td>这个 Post 所属的所有 tags</td>
    </tr>
    <tr>
        <td>page.path</td>
        <td>Post 或者 Page 的源文件地址</td>
    </tr>
</table>
    
#### 分页器

<table>
    <tr>
        <td>变量</td>
        <td>说明</td>
    </tr>
    <tr>
        <td>paginator.per_page</td>
        <td>每一页Posts的数量</td>
    </tr>
    <tr>
        <td>paginator.posts</td>
        <td>这一页可用的Posts</td>
    </tr>
    <tr>
        <td>paginator.total_posts</td>
        <td>Posts 的总数</td>
    </tr>
    <tr>
        <td>paginator.total_pages</td>
        <td>Pages 的总数</td>
    </tr>
    <tr>
        <td>paginator.page</td>
        <td>当前页号</td>
    </tr>
    <tr>
        <td>paginator.previous_page</td>
        <td>前一页的页号</td>
    </tr>
    <tr>
        <td>paginator.previous_page_path</td>
        <td>前一页的地址</td>
    </tr>
    <tr>
        <td>paginator.next_page</td>
        <td>下一页的页号</td>
    </tr>
    <tr>
        <td>paginator.next_page_path</td>
        <td>下一页的地址</td>
    </tr>
</table>

---

### 模板

#### 过滤器

`过滤器`是一个简便的工具, 用于对输出数据进行处理
    
详细说明请查看相关文档 [Liquid Filter](https://docs.shopify.com/themes/liquid/filters)

#### 标签

`标签`是一个表示程序逻辑的工具, 告诉模板处理逻辑
    
详细说明请看相关文档 [Liquid tag](https://docs.shopify.com/themes/liquid/tags)

---

### 永久链接

`永久链接`是用来设置文章的URL链接格式的

#### 模板变量

<table>
    <tr>
        <td>变量名</td>
        <td>说明</td>
    </tr>
    <tr>
        <td>year</td>
        <td>年份</td>
    </tr>
    <tr>
        <td>month</td>
        <td>月份，格式如 `01, 10`</td>
    </tr>
    <tr>
        <td>i_month</td>
        <td>月份，格式如 `1, 10`</td>
    </tr>
    <tr>
        <td>day</td>
        <td>日期，格式如 `01, 20`</td>
    </tr>
    <tr>
        <td>i_day</td>
        <td>日期，格式如 `1, 20`</td>
    </tr>
    <tr>
        <td>title</td>
        <td>标题</td>
    </tr>
    <tr>
        <td>categories</td>
        <td>目录</td>
    </tr>
</table>
    
#### 链接类型

<table>
    <tr>
        <td>链接类型</td>
        <td>URL 模板</td>
    </tr>
    <tr>
        <td>date</td>
        <td>/:categories/:year/:month/:day/:title.html</td>
    </tr>
    <tr>
        <td>pretty</td>
        <td>/:categories/:year/:month/:day/:title/</td>
    </tr>
    <tr>
        <td>none</td>
        <td>/:categories/:title.html</td>
    </tr>
</table>

#### 举例

比如文件名： `/2009-04-29-slap-chop.textile`
<table>
    <tr>
        <td>设置</td>
        <td>对应的 URL</td>
    </tr>
    <tr>
        <td>没有配置或 permalink: date</td>
        <td>/2009/04/29/slap-chop.html</td>
    </tr>
    <tr>
        <td>permalink: pretty</td>
        <td>/2009/04/29/slap-chop/index.html</td>
    </tr>
    <tr>
        <td>permalink: /:month-:day-:year/:title.html</td>
        <td>/04-29-2009/slap-chop.html</td>
    </tr>
    <tr>
        <td>permalink: /blog/:year/:month/:day/:title</td>
        <td>/blog/2009/04/29/slap-chop/index.html</td>
    </tr>
</table>

---

### 分页

#### 开启分页

开启分页功能很简单，只需要在 _config.yml里边加一行，并填写每页需要几行：

	paginate: 5
        
下边是对需要带有分页页面的配置：

	paginate_path: "blog/page:num"
        
blog/index.html将会读取这个设置，把他传给每个分页页面，然后从第 2 页开始输出到 blog/page:num ， :num 是页码。如果有 12 篇文章并且做如下配置 paginate: 5 ， Jekyll会将前 5 篇文章写入 blog/index.html ，把接下来的 5 篇文章写入 blog/page2/index.html，最后 2 篇写入 blog/page3/index.html。

#### 分页例子

    {% raw %}
    {% if paginator.total_pages > 1 %}
    <div class="pagination">
      {% if paginator.previous_page %}
        <a href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">&laquo; Prev</a>
      {% else %}
        <span>&laquo; Prev</span>
      {% endif %}

      {% for page in (1..paginator.total_pages) %}
        {% if page == paginator.page %}
          <em>{{ page }}</em>
        {% elsif page == 1 %}
          <a href="{{ '/index.html' | prepend: site.baseurl | replace: '//', '/' }}">{{ page }}</a>
        {% else %}
          <a href="{{ site.paginate_path | prepend: site.baseurl | replace: '//', '/' | replace: ':num', page }}">{{ page }}</a>
        {% endif %}
      {% endfor %}

      {% if paginator.next_page %}
        <a href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Next &raquo;</a>
      {% else %}
        <span>Next &raquo;</span>
      {% endif %}
    </div>
    {% endif %}
    {% endraw %}

---

### 插件

[=====> JUMP <=====](http://jekyll.bootcss.com/docs/plugins/)

---
