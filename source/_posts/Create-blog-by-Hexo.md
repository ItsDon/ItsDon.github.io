---
title: 基于Hexo搭建Github博客
author: Don
date: 2018-3-30 17:23:23
tags:
    - Hexo
    - Github
categories:
    - Tech
---

### Hexo

安装 `hexo`之前需要先安装`git`和`node.js`,详细安装见[Hexo官网](https://hexo.io/)

```
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server

```

经过以上步骤的操作，就可以在浏览器中查看静态的blog模版，默系统认分配4000端口，完整地址：[http://localhost:4000/](http://localhost:4000/)

### 配置站点_config.yml文件

使用`git`将站点部署到远程仓库

``` 
deploy:
  type: git
  repo: git@github.com:ItsDon/ItsDon.github.io.git

```

然后执行以下命令
```
npm install hexo-deployer-git --save

```

安装完成后，就可以在`_posts`目录下添加markdown文件作为每一篇博客，通过以下语句来生成和部署我们的静态站点

清空缓存,即删除`public`目录
```
 hexo clean

```

重新生成`public`目录，该目录下的放的就是博客所有的页面及相关的`css` ，`js`等
```
hexo g

```

通过`git`将`.deploy_git`目录下的文件部署到远端仓库
```
hexo deploy

```
 <!-- more -->

### 更换theme

[hexo官网](https://hexo.io/themes/)提供了大量的theme样式，[NEXT](https://github.com/iissnan/hexo-theme-next)是目前比较流行的一款主题

#### step1
clone next 项目到`themes`目录下

```
 git clone https://github.com/iissnan/hexo-theme-next themes/next

```

#### step2
  在站点_config.yml中更换主题

  ```
  # Extensions
  ## Plugins: https://hexo.io/plugins/
  ## Themes: https://hexo.io/themes/
  theme: next

  ```

