---
urlname: law-of-yahoo-web-development
title: 雅虎开发的 14 条军规
date: 2018-07-30 16:44:17
categories: 技术研究
tags: [前端, HTTP]
---

对于前端的性能优化，早期有雅虎的十四条军规作为思路导向，目的是为了从开发源头上减少后期性能优化的复杂度

<!--more-->

1. 尽可能减少 HTTP 请求数
2. 使用内容分发网络 CDN
3. 添加 Expire/Cache-Control 头：Add an Expires Header
4. 启动 Gzip 压缩
5. 将 CSS 放在页面最上面
6. 将 script 放在页面最下面
7. 避免在 CSS 中使用 Expressions
8. 把 javascript 和 CSS 都放到外部文件中
9. 减少 DNS 查询
10. 压缩 javascript 和 CSS
11. 避免重定向
12. 移除重复的脚本
13. 配置实体标签（ETags）
14. 使 AJAX 缓存

# 参考资料

[Yahoo前端优化十四条军规](http://developer.51cto.com/art/201207/347525_all.htm)