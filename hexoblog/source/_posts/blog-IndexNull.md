---
title: Hexo 博客升级
date: 2021-01-11 21:30:00
tags: [hexo,nodejs]
categories: Blog
---
### Hexo 博客生成后 Index 文件为空

<!-- more -->
### 简介
出差时在一台新系统中装环境，执行生成博客命令 hexo generate 后检查生成目录 public 中的 index.html 文件都为空。
调试后发现是新装的 [Node.js](https://nodejs.org/zh-cn/) 版本与 [Hexo](https://hexo.io/) 版本不匹配，旧版 Nodejs 版本为 v12.14.0，新装版本为 v14.15.4，Hexo 版本为 v3.9。

### 解决办法
#### 降级 Nodejs
降级 Nodejs 版本至 v12

#### 升级 Hexo
升级 Hexo 版本至与 Nodejs v14.15 匹配的 Hexo v5.3.0

##### 修改配置文件版本
修改 Hexo 目录中 package.json 文件
``` json
{
  /* ··· */
  "hexo": {
    "version": "5.3.0"
  },
  "dependencies": {
    "hexo": "^5.3.0",
    /* ··· */
  }
}
```

##### 执行命令升级版本
``` shell
npm update
```

##### 安装 swig
如果直接生成博客则会出现 “ { % extends ‘_layout.swig‘ % } { % import ‘_macro/post.swig‘ as post_template % }“ 问题。
原因是 hexo 在 5.0 之后把 swig 给删除了需要自己手动安装。
``` shell
npm i hexo-renderer-swig
```

##### 修改 external_link
生成博客提示异常信息
``` log
INFO  Validating config
WARN  Deprecated config detected: "external_link" with a Boolean value is deprecated. See https://hexo.io/docs/configuration for more details.
```
修改文件 _config.yml 中的节点
``` yml
external_link:
  enable: true|false
```
改为
``` yml
external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude: ''
```

##### Next 主题分页显示异常
参考最新的 [pagination.swig](https://github.com/theme-next/hexo-theme-next/blob/master/layout/_partials/pagination.swig)
修改 Next 主题的配置文件 ../themes/next/layout/_partials/pagination.swig
``` swig
{%- if page.prev or page.next %}
  <nav class="pagination">
    {{
      paginator({
        prev_text: '<i class="fa fa-angle-left" aria-label="' + __('accessibility.prev_page') + '"></i>',
        next_text: '<i class="fa fa-angle-right" aria-label="' + __('accessibility.next_page') + '"></i>',
        mid_size : 1,
        escape   : false
      })
    }}
  </nav>
{%- endif %}
```

##### Hexo 无法解析特殊字符导致生成失败
标题中的中括号，例如：
``` Markdown
title: 网页使用 <iframe> 嵌入部分其他网页
```

###### 解决方法
文章中的特殊字符，例如：
``` Markdown
{{ }}、{% %}
```

推荐避免使用类似字符或使用[HTML转义字符对照表](https://tool.oschina.net/commons?type=2)进行转义。

##### NexT5 中文超链接乱码导致不能跳转
博客文章左侧超链接点击无效无法跳转，控制台报错：Cannot read property 'top' of undefined。

###### 解决方法
由于 href 链接中文 url 未经转码导致中文链接无法跳转，使用 decodeURI() 方法进行重新编码。
``` js
// 对获取到的url进行重编码
targetSelector = decodeURI(this.getAttribute('href'));
```

###### 完整段落代码
``` js
// TOC item animation navigate & prevent #item selector in adress bar.
$('.post-toc a').on('click', function(e) {
  e.preventDefault();
  var targetSelector = NexT.utils.escapeSelector(this.getAttribute('href'));
  // 对获取到的url进行重编码
  targetSelector = decodeURI(this.getAttribute('href'));
  var offset = $(targetSelector).offset().top;

  hasVelocity
    ? html.velocity('stop').velocity('scroll', {
      offset  : offset + 'px',
      mobileHA: false
    })
    : $('html, body').stop().animate({
      scrollTop: offset
    }, 500);
});
```