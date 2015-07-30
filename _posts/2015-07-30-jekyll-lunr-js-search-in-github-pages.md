---
layout: post
category: "web"
title: "jekyll-lunr-js-search使用的安装与使用"
tags: ["jekyll,lunr,search,github-pages"]
summary: "我只是一条摘要"

---

花了几天时间，终于把站内静态搜索的插件`jekyll-lunr-js-search`搞定了，主要参考了推酷上的一篇文章[Jekyll站内搜索：jekyll-lunr-js-search使用说明](http://www.tuicool.com/articles/6vA36f6)和github上的README。  
{% include github-widget.html git-repo="slashdotdash/jekyll-lunr-js-search" %}

这里把主要的步骤记一下，方便其他人。

## 安装
参照github上的步骤，使用ruby gems安装：

1. gem install jekyll-lunr-js-search
2. 修改Jekyll根目录下的`_config.yml`文件，添加一项配置`gems: ['jekyll-lunr-js-search']`
3. 在`_config.yml`中我掉进了一个坑：
    有个配置项`safe`以前填的是`true`,这种情况下插件不会自动在`_site`目录下生成所需的文件，也就是不起作用了；
    必须把`safe`改为`false`（或不配置）才可以
4. 给搜索输入框找个家  

    ```js
    <script src="/js/search.min.js" type="text/javascript" charset="utf-8"></script>

    <div id="search">
      <form action="/search.html" method="get">
        <input type="text" id="search-query" name="q" placeholder="Search" autocomplete="off">
      </form>
    </div>

    <script type="text/javascript">
      $(function() {
        $('#search-query').lunrSearch({
          indexUrl: '/js/index.json',   // Url for the .json file containing search index data
          results : '#search-results',  // selector for containing search results element
          entries : '.entries',         // selector for search entries containing element (contained within results above)
          template: '#search-results-template'  // selector for Mustache.js template
        });
      });
    </script>
    ```
5. 搜索结果页

    ```html
    ---
    layout: default
    title: "搜索：Search"
    ---

    <section id="search-results" style="display: none;">
      <p>Search results</p>
      <div class="entries">
      </div>
    </section>

    {% raw %}
    <script id="search-results-template" type="text/mustache">
      {{#entries}}
        <article>
          <h3>
            {{#date}}<small><time datetime="{{pubdate}}" pubdate>{{displaydate}}</time></small>{{/date}}
            <a href="{{url}}">{{title}}</a>
          </h3>
        </article>
      {{/entries}}
    </script>
    {% endraw %}
    ```

## 安装完成
安装完成之后在本地执行`jekyll serve`测试搜索，一切正常（只是搜不出中文），但是push到github上以后却不能用了。  
这是因为github都是以safe模式执行jekyll的，相当于把`_config.yml`中的`safe`项配置为`true`，也相当于在本地执行`jekyll serve --safe`命令。  
解决方法在推酷的那篇文章里有3种，我使用的方法是：
    把本地生成的`_site/js`目录里的`index.json`和`search.min.js`都拷贝到jekyll项目的根目录下的`js`文件夹中，
    再提交到github；
    缺点是每次更新文章后都要先把根目录下的`js/index.json`删掉，再执行`jekyll serve`，
    把最新生成的`index.json`再拷贝一次并提交到github。

## 花絮
在使用chrome调试时，提示`search.js.map Not Found`，这个问题不影响js的执行，
但是当时搜索搜不出结果，所以查了它的原因和解决办法：
[jquery.min.map 404 (Not Found)出错的原因及解决办法](http://nonfu.me/p/8113.html)

    从 jQuery 1.9.0 版本后在原始代码里会有 @ sourceMappingURL=jquery.min.map

    什么是Source map

    简单说，Source map就是一个信息文件，里面存储着位置信息。也就是说，转换后的代码的每一个位置，所对应的转换前的位置。

    有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码。这无疑给开发这带来了很大方便。

    导致  jquery.min.map 404 原因

    更新后 Chrome 自行开启了 Enable source maps 的选项但你又沒有放 Source map 导致找不到档案。

    解决办法

    解決方式1.

    将 Developer Tools ->设置 Enable source maps 关闭

    解決方式2.

    下载同一版本的 source maps跟jquery.js同目录

    source maps 会跟 jquery 同位置
