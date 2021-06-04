---
title: 实现compose
date: 2021-06-04 19:27:49
tags: 
- 前端 
- javascript
categories:
- 前端
- javascript
---
+ 实现一个 compose 函数
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
