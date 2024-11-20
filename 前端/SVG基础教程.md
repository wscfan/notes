# SVG 绘图基础教程

## 什么是 SVG？
SVG（Scalable Vector Graphics）是一种基于 XML 的矢量图形格式。它的特点是：
- 可缩放而不失真
- 文件体积小
- 可以通过代码控制和修改
- 支持动画和交互

## 基本结构
一个基本的 SVG 文档结构如下：
```xml
<svg xmlns="http://www.w3.org/2000/svg" width="800" height="600">
    <!-- SVG 内容 -->
</svg>
```

属性说明：

- xmlns: XML Namespace的缩写，意为XML命名空间。它是XML（标准通用标记语言的子集）中用于解决命名冲突的一种机制。

## 基础图形

### 1. 矩形 (rect)
```xml
<rect x="10" y="10" width="100" height="50" fill="red" />
```
属性说明：
- x, y: 矩形左上角的坐标
- width, height: 矩形的宽度和高度
- fill: 填充颜色

### 2. 圆形 (circle)
```xml
<circle cx="100" cy="100" r="50" fill="blue" />
```
属性说明：
- cx, cy: 圆心坐标
- r: 半径

### 3. 椭圆 (ellipse)
```xml
<ellipse cx="100" cy="100" rx="100" ry="50" fill="green" />
```
属性说明：
- cx, cy: 椭圆中心坐标
- rx, ry: x轴和y轴的半径

### 4. 线段 (line)
```xml
<line x1="0" y1="0" x2="100" y2="100" stroke="black" />
```
属性说明：
- x1, y1: 起点坐标
- x2, y2: 终点坐标
- stroke: 线条颜色

### 5. 多边形 (polygon)
```xml
<polygon points="50,0 100,50 50,100 0,50" fill="purple" />
```

![image-20241120153149656](https://gitee.com/wscfan/images/raw/master/202411201531724.png) 
属性说明：

- points: 点坐标序列

### 6. 路径 (path)
```xml
<path d="M10 10 L90 90 L90 10 Z" fill="orange" />
```
![image-20241120153455263](https://gitee.com/wscfan/images/raw/master/202411201534305.png) 

常用命令：
- M: 移动到 (Move to)
- L: 画线到 (Line to)
- H: 水平线
- V: 垂直线
- C: 贝塞尔曲线
- Z: 闭合路径

## 样式属性

### 常用样式
1. 填充
```xml
fill="color"           <!-- 填充颜色 -->
fill-opacity="0.5"     <!-- 填充透明度 -->
```

2. 描边
```xml
stroke="color"         <!-- 描边颜色 -->
stroke-width="2"       <!-- 描边宽度 -->
stroke-opacity="0.8"   <!-- 描边透明度 -->
```

3. 虚线
```xml
stroke-dasharray="5,5" <!-- 虚线样式 -->
```

## 变换

### 常用变换函数
```xml
<rect transform="translate(50,50)" />     <!-- 平移 -->
<rect transform="rotate(45)" />           <!-- 旋转 -->
<rect transform="scale(2)" />             <!-- 缩放 -->
```

## 示例：绘制一个房子
```xml
<svg width="200" height="200">
    <!-- 房子主体 -->
    <rect x="50" y="80" width="100" height="100" fill="#CC8866"/>
    <!-- 屋顶 -->
    <polygon points="50,80 150,80 100,30" fill="#884422"/>
    <!-- 门 -->
    <rect x="85" y="130" width="30" height="50" fill="#442211"/>
    <!-- 窗户 -->
    <rect x="65" y="100" width="25" height="25" fill="#FFFFFF"/>
    <rect x="110" y="100" width="25" height="25" fill="#FFFFFF"/>
</svg>
```

## 最佳实践
1. 始终设置 viewBox 属性以确保正确缩放
2. 使用语义化的 class 和 id 命名
3. 保持代码整洁，适当使用注释
4. 考虑浏览器兼容性
5. 对于复杂图形，建议使用 groups (`<g>`) 进行分组

## 调试技巧
1. 使用浏览器开发者工具查看 SVG
2. 临时添加 stroke 属性来查看图形边界
3. 使用在线 SVG 编辑器进行测试

## 进阶主题
- SVG 滤镜效果
- SVG 动画
- SVG 渐变
- SVG 裁剪和蒙版
- SVG 文本操作