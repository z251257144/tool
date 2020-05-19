# Gitbook安装使用



### 安装 Node.js

GitBook 是一个基于 Node.js 的命令行工具，下载安装 [Node.js](https://links.jianshu.com/go?to=https%3A%2F%2Fnodejs.org%2Fen)，安装完成之后，你可以使用下面的命令来检验是否安装成功。

```sh
$ node -v
v12.14.0
```



### 安装 [GitBook](https://www.gitbook.com/)

输入下面的命令来安装 [GitBook](https://www.gitbook.com/)。

```sh
$ npm install gitbook-cli -g
```

安装完成之后，你可以使用下面的命令来检验是否安装成功。

```sh
$ gitbook -V
CLI version: 2.3.2
GitBook version: 3.2.3
```



### 使用

GitBook 准备工作做好之后，我们进入一个你要写书的目录，输入如下命令。

```sh
$ gitbook init
warn: no summary file in this book
info: create README.md
info: create SUMMARY.md
info: initialization is finished
```



可以看到他会创建 README.md 和 SUMMARY.md 这两个文件，README.md 应该不陌生，就是说明文档，而 SUMMARY.md 其实就是书的章节目录，其默认内容如下所示：

```css
# Summary

* [Introduction](README.md)
```



接下来，我们输入 `$ gitbook serve` 命令，然后在浏览器地址栏中输入 `http://localhost:4000` 便可预览书籍。

效果如下所示：

![](image\1.jpg)



运行该命令后会在书籍的文件夹中生成一个 `_book` 文件夹, 里面的内容即为生成的 html 文件，我们可以使用下面命令来生成网页而不开启服务器。

```undefined
gitbook build
```

下面我们来详细介绍下 GitBook 目录结构及相关文件。

## 目录结构

GitBook 基本的目录结构如下所示：

```ruby
.
├── README.md
├── SUMMARY.md
├── chapter-1/
|   ├── README.md
|   └── something.md
└── chapter-2/
    ├── README.md
    └── something.md
```



### SUMMARY.md

这个文件主要决定 GitBook 的章节目录，它通过 Markdown 中的列表语法来表示文件的父子关系，下面是一个简单的示例：



```csharp
# Summary

* [Introduction](README.md)
* [Part I](part1/README.md)
    * [Writing is nice](part1/writing.md)
    * [GitBook is nice](part1/gitbook.md)
* [Part II](part2/README.md)
    * [We love feedback](part2/feedback_please.md)
    * [Better tools for authors](part2/better_tools.md)
```

这个配置对应的目录结构如下所示:

![img](https:////upload-images.jianshu.io/upload_images/1944467-de97699c5919469e?imageMogr2/auto-orient/strip|imageView2/2/w/604/format/webp)



### 编辑器

可以用[Typora](https://www.typora.io/) 等自己喜欢的来编辑。



## 发布

使用github、travis、github pages自动构建发布gitbook页面，首先保证注册了[github](https://github.com/)账号、[travis](https://www.travis-ci.org/)账号，在github建立文档仓库，travis开启同步。

1、创建工程，并创建`gh-pages`分支。

![](image\2.jpg)

2、开启工程阅读功能

![](image\3.jpg)

3、在travis开启同步

![](image\4.jpg)

![](image\5.jpg)

![](image\6.jpg)

此处需要用到Github Token用于同步代码，可从Github个人设置中获取。

![](image\7.jpg)



设置文档工程部署配置，在github仓库新建`.travis.yml`文件：

```ruby
language: node_js # 构建所需的语言环境
node_js:
  - "v11.14.0"  # 对应的版本
branches:
  only:
  - master    # 构建的分支
cache:
  directories:
  - node_modules # 依赖缓存的目录
before_install:
- export TZ='Asia/Shanghai'  # 设置时区
install:
 - npm install -g gitbook-cli # 安装编译工具
script:
- gitbook build
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GITHUB_TOKEN # github 上的token环境变量
  local-dir: ./_book/ ## 根据情况自定义到静态文件输出目录
  target-branch: gh-pages
  verbose: true
  on:
    branch: master
   
```

![](image\8.jpg)



现在将文件提交至Github，等待travis同步部署。部署效果如下。

![部署效果图](image\10.jpg)



打开文档链接：https://z251257144.github.io/tool/， 运行效果如下：

![](image\9.jpg)



## 结束

自此Gitbook安装、使用、发布流程已经全部写完，方便我们编写、阅读文章。