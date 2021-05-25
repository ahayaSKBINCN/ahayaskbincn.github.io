---
layout: keep
title: webpack 的构建流程
date: 2021-05-25 11:35:57
tags: webpack
---
## 1. 基本架构
![基本架构](./FontEndV3/webpack.jpeg "基本架构")
1. 通过 `Yargs` 解析 `config` 和 `shell` 中的配置项。
2. `webpack` 初始化过程, 首先通过 `options` 生成 `compiler`,
然后初始化 `webpack` 内置插件及 `options` 配置。
3. `run` 代表编译的开始，会构建 `compilation` 对象，用于存储这一次编译过程的所有数据。
4. `make` 执行真正的编译构建过程，从入口文件开始，构建模块，直到所有模块创建结束。
5. `seal` 生成 `chunks`，对 `chunks` 进行一系列的优化操作，并生成要输出的代码。
6. `seal` 结束后，`Compilation` 实例的所有工作到此也全部结束，意味着一次构建过程已经结束。
7. `emit` 被触发之后，`webpack` 会遍历 `compilation.assets`, 生成所有文件，然后触发任务点 `done`，结束构建流程。
