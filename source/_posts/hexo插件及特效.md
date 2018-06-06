---
title: hexo插件以及特效
# date: 2018-06-05 23:56:24
tags: [hexo]
---

# hexo插件以及特效

- 添加动态背景
[https://github.com/hustcc/canvas-nest.js](https://github.com/hustcc/canvas-nest.js)
- 添加`Fork me on GitHub`
[https://blog.github.com/2008-12-19-github-ribbons/](https://blog.github.com/2008-12-19-github-ribbons/)
- 添加RSS
```bash
npm install hexo-generator-feed --save
```
然后在hexo目录下修改`_config.yml`文件
```bash
# Extensions
## Plugins: http://hexo.io/plugins/
Plugins: hexo-generate-feed
```
<!--more-->
- 点击出现桃心效果
将[这个网址](http://7u2ss1.com1.z0.glb.clouddn.com/love.js)里的代码copy一下， 在`/themes/next/source/js/src`目录下新建一个`love.js`文件并将代码粘贴进去， 然后在`/themes/next/layout/_layout.swig`里添加代码
```js
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/love.js"></script>
```
