# Grid布局基础教程

## 什么是Grid布局？

CSS Grid是一种现代的二维布局系统，允许你轻松创建复杂的网页布局。它提供了强大的网格管理能力，使得页面排版变得简单而灵活。

## 基本概念

### Grid容器和Grid项目

- **Grid容器**：应用`display: grid`的父元素
- **Grid项目**：Grid容器内的直接子元素

### 核心术语

1. **Grid线**：构成网格的水平和垂直分隔线
2. **Grid单元格**：网格中的基本单位
3. **Grid轨道**：两条Grid线之间的空间

## 创建基本Grid布局

### 定义列和行

```css
.grid-container {
  display: grid;
  grid-template-columns: 100px 200px 100px; /* 三列 */
  grid-template-rows: 50px 100px; /* 两行 */
  gap: 10px; /* 网格间距 */
}
```

### 列宽定义方式

#### 固定宽度
```css
grid-template-columns: 100px 200px 300px;
```

#### 弹性单位
```css
grid-template-columns: 1fr 2fr 1fr; /* 按比例分配空间 */
```

#### 重复单位
```css
grid-template-columns: repeat(3, 1fr); /* 创建3个等宽列 */
```

## 网格项目定位

### 显式定位

```css
.grid-item {
  grid-column: 2 / 4; /* 跨越第2到第4条列线 */
  grid-row: 1 / 3;    /* 跨越第1到第3条行线 */
}
```

### 命名网格线

```css
.grid-container {
  grid-template-columns: [start] 1fr [middle] 2fr [end];
  grid-template-rows: [top] 100px [bottom] 200px;
}

.grid-item {
  grid-column: start / end;
  grid-row: top / bottom;
}
```

## 响应式Grid布局

### 自动填充

```css
.responsive-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 15px;
}
```

## 实用网格属性

### 间距控制

```css
.grid-container {
  gap: 10px;               /* 全局间距 */
  column-gap: 20px;        /* 列间距 */
  row-gap: 15px;           /* 行间距 */
}
```

### 对齐属性

```css
.grid-container {
  justify-items: center;   /* 水平居中 */
  align-items: stretch;    /* 垂直拉伸 */
}
```

## 常见布局模式

### 居中布局

```css
.center-layout {
  display: grid;
  place-items: center;     /* 同时居中 */
  height: 100vh;
}
```

### 侧边栏布局

```css
.sidebar-layout {
  display: grid;
  grid-template-columns: 250px 1fr;
}
```

## 兼容性注意

- 现代浏览器全面支持Grid布局
- IE11及以下版本需要额外的polyfill
- 建议使用渐进增强策略

## 学习建议

1. 从简单布局开始
2. 多实践，尝试各种组合
3. 使用浏览器开发者工具调试
4. 关注Grid布局的最新特性和技巧

## 总结

Grid布局提供了前所未有的布局灵活性。通过掌握其基本概念和属性，你可以创建复杂且响应迅速的网页设计。

## 推荐资源

- MDN Web Docs
- CSS-Tricks Grid指南
- Grid by Example网站

> 提示：持续学习和实践是掌握Grid布局的关键！