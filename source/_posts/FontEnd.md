---
title: 前端面试刷题
date: 2021-05-17 12:12:10
tags: 
- 前端
- react
---
1. 写一个LRU 缓存函数。
```java
class LRUCache {
    class DLinkedNode {
        int key;
        int value;
        DLinkedNode pre;
        DLinkedNode next;
        public DLinkedNode(){}
        public DLinkedNode(int key, int value){
            this.key = key;
            this.value = value;
        }
    }   
    private Map<Integer, DLinkedNode> cache = new Map();
    private int size;
    private int capacity;
    private DLinkedNode head, tail;
    public LRUCache(int capacity){
        this.size = 0;
        this.capacity =capacity;
        
        // 使用伪头部和伪尾部节点
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.pre = head;
    }
   
    public int get(int key){ 
        DLinkedNode node = cache.get(key);
        if(node == null){
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        moveToHead(node);
        return node.value;
    }
   
    public void put(int key, int value){
        DLinkedNode node = cache.get(key);
        if(node == null){
            DLinkNode newNode = new DLinkNode(key, value);
            cache.put(key, newNode);
            ++ size;
            if(size > capacity) {
                DLinkedNode tail = removeTail();
                cache.remove(tail.key);
                moveToHead(node);
            }
        }
    }
    
    private void addToHead(DLinkedNode node){
        node.pre = head;
        node.next = head.next;
        head.next.pre = node;
        head.next = node;
    }
    
    private void removeNode(DLinkedNode node){
        node.pre.next = node.next;
        node.next.pre = node.pre;
    }
    
    private void moveToHead(DLinkedNode node){
        removeNode(node);
        addToHead(node);
    }
    
    private DLinkedNode removeTail(){
        DLinkedNode res = tail.pre;
        removeNode(res);
        return res;
    }
}
   

```
2. 写个防抖和节流函数。

```typescript
// 防抖函数
type Callback = (...args:any[])=> void;
function debounce(cb: Callback, delay: number){
  let timer;
  return function (){
    const _this = this;
    const _args = args;
    if(!timer){
      setTimeout(cb.call(_this, ..._args), delay);
    }else{
      clearTimeout(timer);
      timer = setTimeout(cb.call(_this, ..._args), delay);
    }
  }
}

// 节流函数
function throttle(cb: Callback, delay: number){
  let timer;
  return function (){
    const _this = this;
    const _args = arguments;
    if(timer){
      return;
    }else {
      timer = setTimeout(function () {
        cb.apply(_this, _args)
        timer = null;
      },delay)
    }
  }
}

```
3. http2 的相关特性  
>+ 二进制分帧
>+ 多路复用
>+ 服务器推送
>+ 头部压缩
4. viewport 与 移动端布局方案
>- layout viewport 布局视图
>- visual viewport 可视视窗
>- ideal viewport 理想视窗
>- [参考资料](https://juejin.cn/post/6844903926450356231)
5. 实现一个 compose 函数
```javascript
// lodash.flow
function flow(...funcs) {
    const length = funcs.length
    let index = length
    while (index--) {
        if (typeof funcs[index] !== 'function') {
            throw new TypeError('Expected a function')
        }
    }
    return function(...args) {
        let index = 0
        let result = length ? funcs[index].apply(this, args) : args[0]
        while (++index < length) {
            result = funcs[index].call(this, result)
        }
        return result
    }
}
```
6. webpack plugin 的原理是什么
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

7. react ssr 的使用场景
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


