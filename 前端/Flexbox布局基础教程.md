# Flexbox布局基础教程

## 1. 什么是Flexbox？

Flexbox（弹性盒子布局）是CSS3引入的一种强大布局模式，旨在提供一种更加高效和灵活的方式来分配空间、对齐内容，并为不同尺寸的元素提供动态布局能力。

### 1.1 Flexbox的核心优势
- 简化复杂布局
- 响应式设计更容易实现
- 元素可以动态调整大小和位置
- 减少使用浮动和定位的复杂性

## 2. Flexbox的基本概念

### 2.1 Flex容器和Flex项目
- **Flex容器**：使用`display: flex`声明的父元素
- **Flex项目**：Flex容器中的直接子元素

```css
.container {
    display: flex;  /* 创建一个flex容器 */
}
```

### 2.2 主轴(Main Axis)和交叉轴(Cross Axis)
- **主轴**：由`flex-direction`决定的主要排列方向
- **交叉轴**：垂直于主轴的方向

## 3. Flex容器属性详解

### 3.1 flex-direction：控制主轴方向
```css
.container {
    flex-direction: row;           /* 默认，从左到右 */
    flex-direction: row-reverse;   /* 从右到左 */
    flex-direction: column;        /* 从上到下 */
    flex-direction: column-reverse;/* 从下到上 */
}
```

### 3.2 justify-content：主轴对齐方式
```css
.container {
    justify-content: flex-start;    /* 起始端对齐 */
    justify-content: flex-end;      /* 结束端对齐 */
    justify-content: center;        /* 居中对齐 */
    justify-content: space-between; /* 两端对齐 */
    justify-content: space-around;  /* 均匀分布 */
}
```

### 3.3 align-items：交叉轴对齐方式
```css
.container {
    align-items: stretch;     /* 默认，拉伸填充 */
    align-items: flex-start;  /* 起始端对齐 */
    align-items: flex-end;    /* 结束端对齐 */
    align-items: center;      /* 居中对齐 */
    align-items: baseline;    /* 文字基线对齐 */
}
```

### 3.4 flex-wrap：控制换行
```css
.container {
    flex-wrap: nowrap;       /* 默认，不换行 */
    flex-wrap: wrap;         /* 换行 */
    flex-wrap: wrap-reverse; /* 反向换行 */
}
```

## 4. Flex项目属性详解

### 4.1 flex-grow：放大比例
```css
.item {
    flex-grow: 0;  /* 默认，不放大 */
    flex-grow: 1;  /* 等比例放大 */
}
```

### 4.2 flex-shrink：缩小比例
```css
.item {
    flex-shrink: 1;  /* 默认，等比例缩小 */
    flex-shrink: 0;  /* 不缩小 */
}
```

### 4.3 flex-basis：初始尺寸
```css
.item {
    flex-basis: auto;   /* 默认，根据内容决定 */
    flex-basis: 200px;  /* 固定宽度 */
    flex-basis: 50%;    /* 百分比 */
}
```

### 4.4 align-self：单个项目对齐
```css
.item {
    align-self: auto;        /* 继承容器align-items */
    align-self: flex-start;  /* 顶部对齐 */
    align-self: flex-end;    /* 底部对齐 */
    align-self: center;      /* 居中 */
}
```

## 5. 实用布局示例

### 5.1 垂直居中
```css
.center-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
```

### 5.2 响应式导航
```css
.nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
```

### 5.3 卡片布局
```css
.card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}

.card {
    flex: 1 1 250px;  /* 可增长、可缩小，最小250px */
}
```

## 6. 最佳实践

1. 使用`gap`属性替代`margin`创建间距
2. 利用`flex-grow`和`flex-shrink`创建灵活布局
3. 为不同屏幕尺寸准备媒体查询
4. 考虑浏览器兼容性

## 7. 兼容性建议

- 现代浏览器全面支持Flexbox
- 使用Autoprefixer处理前缀兼容
- 为旧浏览器准备备用布局

## 8. 学习建议

1. 多实践，通过编码加深理解
2. 使用在线可视化工具
3. 阅读MDN等权威文档
4. 关注最新CSS布局技术

## 结语

Flexbox 是现代web布局的利器，掌握它将大大提升你的前端开发技能。持续练习，灵活运用！