---
layout: article
title: 关于webpack中proxy的使用
date: 2017-12-18 23:00:18
tags: webpack
---

#### Webpack解决前端跨域问题

> 编写项目的时候，遇到了跨域问题

> 通过设置webpack.config.js中dev-server proxy的使用来解决跨域问题

> 通过webpack-dev-server中proxy来实现

```javascript
	devServer: {
    contentBase: './dist',
    port: "5000",
    hot: true,
    proxy: {
      '/api/v1/*': {
        target: 'http://[::1]:3000',
        secure: false,
        changeOrigin: true
      }
    }
  }
```

> 通过查看上面的代码很容易看出

> 所有本地dev服务请求/api/v1/的接口

> 即被webpack实现代理地址

> 比如现在请求一个接口 /api/v1/login

> webpack 代理为本地3000端口的/api/v1/login

> 具体地址是 htpp://localhost:3000/api/v1/login

##### proxy实现原理

> 通过webpack内置的插件http-middleware-proxy来实现代理

> 所以所有对于这个插件的配置信息都可以用于webpack中proxy的使用

##### 参数说明

+ 路径重写 option.pathRewrite 

> 支持对象或者函数返回，重写目标url地址

```
    // 重写路由
    pathRewrite: {'^/old/api' : '/new/api'}
    
    // 去掉路由
    pathRewrite: {'^/remove/api' : ''}
    
    // 添加基础路由
    pathRewrite: {'^/' : '/basepath/'}
    
    // 正常 路由 正则匹配 实现 重写
    pathRewrite: function (path, req) { return path.replace('/api', '/base/api') }
```


