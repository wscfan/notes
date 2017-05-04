# Vue.js笔记
[toc]

## 一、Vue.js介绍
### 1、Vue.js核心思想
1. 数据驱动
2. 组件化

## 二、Vue-cli（Vue的脚手架工具）
### 1、 Vue-cli安装
Prerequisites: Node.js (>=4.x, 6.x preferred), npm version 3+ and Git
```dos
npm install -g vue-cli
```
> 使用淘宝NPM镜像安装
> `npm install -g cnpm --registry=https://registry.npm.taobao.org`
> `cnpm install -g vue-cli`

### 2、新建项目
```dos
vue init <template-name> <project-name>
```
Example:
```dos
vue init webpack sell
```
进入到新建的工程目录下:
```dos
npm install
```
执行脚本：
```dos
npm run dev
```
