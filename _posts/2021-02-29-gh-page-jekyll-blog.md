---
title: 如何使用github page+jekyll搭建个人博客
published: true
categories: project
---
## 前言

搭建个人博客,分享知识,记录生活.
静态网页的个人博客可以托管在github上,只需创建一个免费的账号.
再搭建静态网页生成器jekyll,使用markdown编辑博客,还可获取大量主题

## 搭建过程

### github个人界面

1. 创建一个免费的[github](https://github.com)账号:点击sign in,填写内容

    >
    >github简介:git仓库托管平台,git为一种分布式!版本控制系统
    >

    ![](assets/2021-02-29-gh-page-jekyll-blog/2021-01-29-085301.png)

2. 挑选一个喜欢的[jekyll主题](http://jekyllthemes.org/),fork它的仓库

    ![](assets/2021-02-29-gh-page-jekyll-blog/2021-01-29-090428.png)

    比如说主题black-pourl,点击它

    ![](assets/2021-02-29-gh-page-jekyll-blog/2021-01-29-090707.png)

    点击homepage去它的github首页

    ![](assets/2021-02-29-gh-page-jekyll-blog/2021-01-29-090748.png)

    点击右上角的fork,复制到自己的仓库

    ![](assets/2021-02-29-gh-page-jekyll-blog/2021-01-29-091049.png)

    切换到settings更改仓库的名称,把仓库名改为`username.github.io`,username使用你的用户名替换

    ![](assets/2021-02-29-gh-page-jekyll-blog/2021-01-29-091233.png)\

    再setting中切换发布源为master

    ![](assets/2021-02-29-gh-page-jekyll-blog/publishing-source-drop-down.png)

    然后使用浏览器访问username.github.io就可以看到自己的博客


### 更改域名

1. 注册一个域名,在[阿里云](https://cn.aliyun.com/index.htm),[腾讯云](https://cloud.tencent.com/),[华为云](https://www.huaweicloud.com/)都可以.

2. 解析域名到username.github.io(域名先要实名认证,审核一般需要一个小时左右)

    ![](assets/2021-02-29-gh-page-jekyll-blog/2021-01-29-092736.png)

3. 在github setting中设置custom domain

    ![](assets/2021-02-29-gh-page-jekyll-blog/2021-01-29-093122.png)

    github在仓库中创建了一个文件CNAME,内容为你的域名

### 本地部署

[安装WSL](https://juejin.cn/post/6844903845097635854)在windows中使用Ubuntu的命令行工具

1. 安装git(ubuntu)

    ```bash
    sudo apt-get install git-core
    ```

    [git教程](https://git-scm.com/book/en/v2)

2. 安装ruby

    ```bash
    sudo apt-get install ruby-full
    ```

3. 安装jekyll

    ```bash
    gem install jekyll
    gem install jekyll-seo-tag
    gem install jekyll-paginate
    gem install jekyll-sitemap
    ```

4. 使用git clone你的github仓库

    ![](assets/2021-02-29-gh-page-jekyll-blog/2021-01-29-094158.png)    

    ```bash
    git clone https://github.com/username/username.github.io.git
    ```

5. 本地部署

    ```bash
    cd username.github.io
    jekyll serve
    ```

    然后就可以在[localhost](http://127.0.0.1:4000/)访问

### 编写博客

1. 在`_post`创建文件,格式为`yyyy-mm-dd-name.md`

    首行添加文章信息,用于jekyll生成静态网页

    ```txt
    ---
    title: tittle of blog
    published: true
    ---

    content of your blog
    ```
    
    文章使用[md语法](https://www.markdownguide.org/basic-syntax/)编辑

    如果需要添加图片,把图片放在`assets`目录中,博客中使用相对路径引用

2. 本地测试

    ```bash
    jekyll serve
    ```

3. 部署到github仓库

    ```bash
    git add .
    git commit -m "commit message"
    git push -u origin master
    ```

### 扩展功能

1. [评论功能](https://blog.csdn.net/ljinddlj/article/details/52273652)

2. [文章分类功能](https://blog.webjeda.com/jekyll-categories/)

3. [流量统计](http://ibruce.info/2015/04/04/busuanzi/)

4. [icon库](https://www.iconfont.cn/)

5. [搜索栏](https://learn.cloudcannon.com/jekyll/jekyll-search-using-lunr-js/)

### 参考链接

1. [github官方文档](https://docs.github.com/en/github/working-with-github-pages)

2. [Halfrost的领域](https://halfrost.com/jekyll/)

3. [少数派](https://sspai.com/post/54608)

4. [cloudcannon](https://learn.cloudcannon.com/)

## 感想

搭建博客饶了很多弯路,看了很多教程,相互参考,github官方文档,绕来绕去简单几个步骤搞了一天,问题主要处在安装jekyll上,最后发现fork一个主题示例是最好的方法,fork成功之后我直接😱.还是要详细浏览文档之后再操作,多看几个教程.还有早睡早起.