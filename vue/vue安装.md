###  Vue安装

1、安装node.js

2、利用淘宝npm镜像安装相关的依赖
```dos
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

3、安装全局 vue-cli 脚手架，用于帮助搭建所需的模板框架
```dos
cnpm install -g vue-cli
```
检测是否安装成功： 输入 vue，若出现 vue 的相关说明则说明安装成功  

4、创建项目
```dos
vue init firstVue
```
firstVue 为项目名称  

5、安装依赖
进入到具体的项目
```dos
cd firstVue
```
安装项目
```dos
cnpm install
```
6、此时，打开项目文件夹，会多出一个 node_modules 文件夹，里边的内容是之前安装的依赖

7、测试环境是否搭建成功

进入项目文件夹，输入下面命令
```dos
cnpm run dev
```
或者在浏览器中输入 localhost:8080

