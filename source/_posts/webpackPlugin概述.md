---
title: webpackPlugin概述
date: 2021-06-04 19:29:16
tags: 
- webpack plugin
- 前端
categories:
- 前端
- webpack
---

+ webpack plugin 的原理是什么
> - 在 webpack 中，专注于处理 webpack 在编译过程中的某个特定的任务的功能模块，可以称为插件。
> - `plugin` 和 `loader` 的区别：
    >  + loader 是一个转换器，将 A 文件进行编译成 B 文件，比如：将 A.less 转换为 A.css，单纯的文件转换过程。
         webpack 自身只支持 js 和 json 这两种格式的文件，对于其他文件需要通过 loader 将其转换为 commonJS 规范的文件后，webpack 才能解析到。
>   + plugin 是一个扩展器，它丰富了 webpack 本身，针对是 loader 结束后，
      webpack 打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听 webpack 打包过程中的某些节点
      执行广泛的任务。

- [参考资料](https://segmentfault.com/a/1190000038338386)
```javascript
class HelloPlugin {
  // 在构造函数中获取用户给该插件传入的配置
  constructor(options) {}
  // Webpack 会调用 HelloPlugin 实例的 apply 方法给插件实例传入 compiler 对象
  apply(compiler) {
    // 在emit阶段插入钩子函数，用于特定时机处理额外的逻辑；
    compiler.hooks.emit.tap('HelloPlugin', (compilation) => {
      // 在功能流程完成后可以调用 webpack 提供的回调函数；
    })
    // 如果事件是异步的，会带两个参数，第二个参数为回调函数，
    compiler.plugin('emit', function (compilation, callback) {
      // 处理完毕后执行 callback 以通知 Webpack
      // 如果不执行 callback，运行流程将会一直卡在这不往下执行
      callback()
    })
  }
}

module.exports = HelloPlugin
```
