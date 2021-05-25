---
title: 前端面试刷题2 
date: 2021-05-22 15:11:56 
tags: 前端
---

1. `bind` 方法的实现；

```javascript
const obj = {};
console.log(obj); // {}
console.log(typeof Function.prototype.bind); // function
console.log(typeof Function.prototype.bind()); // function
console.log(Function.prototype.bind.name); // bind
console.log(Function.prototype.bind().name); // bound
/******************************************
 * bind 函数调用之后返回 bound 函数
 ******************************************/
const obj1 = {name: 'spring'};

function original(a, b) {
    console.log(this.name);
    console.log(a, b);
    return false;
}

const bound = original.bind(obj1, 'first', 2);
console.log(bound.name); // bound original
console.log(bound()); // spring, ['first', 2], false
console.log(bound.length); // 0
console.log(original.length); // 2

/*****************************************
 * 自定义 bind 函数
 *****************************************/
Function.prototype.bindFn = function (thisArg) {
    if (typeof this !== "function") {
        throw new TypeError(this + ` must be a function`);
    }
    // 保存 this 指针
    const self = this;
    // 除去 thisArg 其余转换成 args
    const args = [].slice.call(arguments, 1);
    const bound = function () {
        // 将参数转换成 boundArgs
        const boundArgs = [].slice.call(arguments);
        return self.apply(thisArg, args.concat(boundArgs));
    }
    return bound;
}
```

2. 用 new 关键字实例化 bound 函数的结果是什么？

+ `new` 关键字都做了什么 ？
    1. 创建一个全新的对象
    2. 这个对象会被执行 `[[Prototype]]`，也就是 `__proto__`链接
    3. 生成的新对象会绑定到函数调用的`this`
    4. 通过 `new` 创建的每一个对象将最终被 `[[Prototype]]`链接到这个函数的`prototype`对象上
    5. 如果一个函数没有返回对象 `Function, Object, Array, Date, RegExp, Error`, 那么通过 `new` 调用的函数将会自动返回这个新的对象。

```javascript
const obj = {
    name: 'spring',
};

function original(a, b) {
    console.log('this', this);
    console.log('typed this is', typeof this);
    this.name = b;
    console.log('this.name is', this.name);
    console.log('typed this is', this);
    console.log([a, b]);
}

const bound = original.bind(obj, 1);
const instance = new bound(2);
// this {name:"spring"}, 
// typed this is object, 
// this.name is 2, 
// typed this is { name: 2 }, 
// [1,2]

Function.prototype.bindFn = function (thisArg) {
    if (typeof this !== "function") {
        throw new TypeError(this + " must be a function");
    }
    const self = this;
    const args = [].slice.call(arguments, 1);
    return function bound() {
        const boundArgs = [].slice.call(arguments);
        const finalArgs = args.concat(boundArgs);
        if (this instanceof bound) {
            if (self.prototype) {
                // ES5 中 bound.prototype = Object.create(self.prototype);
                function Empty() {
                };
                Empty.prototype = self.prototype;
                bound.prototype = new Empty();
            }
            const result = self.apply(this, finalArgs)
            const isObject = typeof result === 'object' && result !== null;
            const isFunction = typeof result === 'function';
            if (isObject || isFunction) {
                return result;
            }
            return this;
        } else {
            return self.apply(thisArg, finalArgs);
        }
    }
    return bound;
}
```



