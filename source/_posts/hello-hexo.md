---
title: Hello Hexo
date: 2017-10-19 10:10:31
categories: Hello World
tags:
  - Hello World
  - Hexo
---

Hexo 入门

<!-- more -->

# 安装 [Hexo][hexo]
首先需要安装
- [Node.js][node]
- [Git][git]

然后执行
```
    $ npm install -g hexo-cli
```
初始化项目
```
    $ hexo init <folder>
    $ cd <folder>
    $ npm install
```

# 安装 [NexT][next]主题 
在终端窗口下，定位到 Hexo 站点目录下。使用 Git checkout 代码：
```
    $ cd your-hexo-site
    $ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
配置项目下 _config.yml 
```
    theme: next
```

在配置主题时遇到的一些问题：

- 配置 [Algolia][algolia] 时：
  需要执行 
```
    $ set HEXO_ALGOLIA_INDEXING_KEY = apiKey
    $ hexo algolia
```
  来更新 Index。
  需要修改 API Key 的 ACL （全部勾选） 

- 设置文章版权信息，配置主题下 _config.yml
 ```
     post_copyright:
       enable: true
 ```
- 设置代码高亮
``` 
   ``` javascript
```
# 部署到 GitHub

安装 hexo-deployer-git

```
    $ npm install hexo-deployer-git --save
```

修改 _config.yml
 
```
   deploy:
      type: git
      repo: https://github.com/punkvv/punkvv.github.io.git
      branch: master
      message:
```

执行命令
```
   hexo d
```

部署时遇到的一些问题：

- CNAME、README.md 文件部署时被删除，需要把文件放到项目 source 目录下
- 配置 README.md 文件不被渲染，项目下 _config.yml
```
   skip_render:  README.md
```

# 常用指令

## 新建文章
```
    $ hexo new [layout] <title>
```
## 生成静态文件
```
    $ hexo g
```
选项|描述 
--- | ---
-d  | 文件生成后立即部署网站
-w  | 监视文件变动

## 启动服务器
```
    $ hexo s
```
选项|描述 
--- | ---
-p  | 重设端口
-s  | 只使用静态文件
-l  | 启动日记记录，使用覆盖记录格式

## 部署网站
```
   $ hexo d
```
选项|描述 
--- | ---
-g  | 部署之前预先生成静态文件

## 清除缓存文件
```
   $ hexo clean
```

参考 [Hexo][hexo]、[NexT][next]

[hexo]: https://hexo.io
[next]: http://theme-next.iissnan.com/
[node]: https://nodejs.org/
[git]: https://git-scm.com/
[algolia]: https://www.algolia.com/ 