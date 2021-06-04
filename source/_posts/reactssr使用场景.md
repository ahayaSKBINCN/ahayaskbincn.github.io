---
title: react ssr 的使用场景
date: 2021-05-17 12:12:10
tags: 
- 前端
- React 
categories:
- 前端
- React
---
+ react ssr 的使用场景
>- <strong>客户端渲染（CSR）</strong>页面初始加载的 HTML 页面中无网页展示内容，需要加载执行JavaScript 文件中的 React 代码，
   > 通过 JavaScript 渲染生成页面，同时，JavaScript 代码会完成页面交互事件的绑定
   > ![Client Side Render](./csr.jpeg "Client Side Render")
>- <strong>服务端渲染（SSR）</strong>用户请求服务器，服务器上直接生成 HTML 内容并返回给浏览器。服务器端渲染来，页面的内容是由 Server 端生成的。
   > 一般来说，服务器端渲染的页面交互能力有限，如果要实现复杂交互，还是要通过引入 JavaScript 文件来辅助实现。
   > 服务器端渲染这个概念，适用于任何后端语言。
   > ![SSR](./ssr.png "Server Side Render")
> - <strong>同构</strong> 同构这个概念存在于 Vue，React 这些新型的前端框架中，同构实际上是客户端渲染和服务器端渲染的一个整合。
    > 我们把页面的展示内容和交互写在一起，让代码执行两次。在服务器端执行一次，用于实现服务器端渲染，在客户端再执行一次，用于接管页面交互，详细流程可参考下图
    > ![同构](./ssr%20同构.jpeg "同构")
> - <strong>同构的优点（为什么会出现同构技术）</strong>
    >   + CSR 项目的 TTFP（Time To First Page）时间比较长，参考之前的图例，
          > 在 CSR 的页面渲染流程中，首先要加载 HTML 文件，之后要下载页面所需的 JavaScript 文件，
          > 然后 JavaScript 文件渲染生成页面。在这个渲染过程中至少涉及到两个 HTTP 请求周期，所以会有一定的耗时，
          > 这也是为什么大家在低网速下访问普通的 React 或者 Vue 应用时，初始页面会有出现白屏的原因。
>   + CSR 项目的 SEO 能力极弱，在搜索引擎中基本上不可能有好的排名。
      > 因为目前大多数搜索引擎主要识别的内容还是 HTML，对 JavaScript 文件内容的识别都还比较弱。
      > 如果一个项目的流量入口来自于搜索引擎，这个时候你使用 CSR 进行开发，就非常不合适了。  
      [相关资料](https://zhuanlan.zhihu.com/p/47044039)








