# Canvas 绘图基础教程

## 什么是 Canvas？
Canvas 是 HTML5 提供的一个用于图形绘制的元素，它提供了一个即时渲染的位图表面。与 SVG 不同，Canvas 是基于像素的，适合处理密集型图形操作和动画。

## 基础设置

### 1. HTML 结构
```html
<canvas id="myCanvas" width="800" height="600"></canvas>
```

### 2. JavaScript 基本设置
```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');
```

## 基础绘图操作

### 1. 矩形
```javascript
// 填充矩形
ctx.fillStyle = 'red';
ctx.fillRect(x, y, width, height);

// 描边矩形
ctx.strokeStyle = 'blue';
ctx.strokeRect(x, y, width, height);

// 清除矩形区域
ctx.clearRect(x, y, width, height);
```

### 2. 路径绘制
```javascript
ctx.beginPath();           // 开始路径
ctx.moveTo(x1, y1);       // 移动到起点
ctx.lineTo(x2, y2);       // 画线到指定点
ctx.closePath();          // 闭合路径
ctx.stroke();             // 描边
ctx.fill();              // 填充
```

### 3. 圆形和圆弧
```javascript
// 绘制圆形
ctx.beginPath();
ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);
ctx.fill();

// 绘制圆弧
ctx.arcTo(x1, y1, x2, y2, radius);
```

### 4. 贝塞尔曲线
```javascript
// 二次贝塞尔曲线
ctx.quadraticCurveTo(cpx, cpy, x, y);

// 三次贝塞尔曲线
ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y);
```

## 样式和颜色

### 1. 颜色、样式和阴影
```javascript
// 填充颜色
ctx.fillStyle = 'color';          // 可以是颜色名、HEX、RGB、RGBA
ctx.strokeStyle = 'color';        // 描边颜色

// 透明度
ctx.globalAlpha = 0.5;           // 全局透明度

// 线条样式
ctx.lineWidth = 5;               // 线条宽度
ctx.lineCap = 'round';          // 线条端点样式：butt, round, square
ctx.lineJoin = 'round';         // 线条连接样式：round, bevel, miter

// 阴影
ctx.shadowColor = 'rgba(0,0,0,0.5)';
ctx.shadowBlur = 5;
ctx.shadowOffsetX = 5;
ctx.shadowOffsetY = 5;
```

### 2. 渐变
```javascript
// 线性渐变
const gradient = ctx.createLinearGradient(x1, y1, x2, y2);
gradient.addColorStop(0, 'white');
gradient.addColorStop(1, 'black');
ctx.fillStyle = gradient;

// 径向渐变
const radialGradient = ctx.createRadialGradient(x1, y1, r1, x2, y2, r2);
```

## 变换

### 1. 基本变换
```javascript
ctx.translate(x, y);    // 平移
ctx.rotate(angle);      // 旋转
ctx.scale(x, y);        // 缩放

// 保存和恢复状态
ctx.save();            // 保存当前状态
ctx.restore();         // 恢复之前的状态
```

### 2. 变换矩阵
```javascript
ctx.transform(a, b, c, d, e, f);    // 变换矩阵
ctx.setTransform(a, b, c, d, e, f); // 重置并设置变换矩阵
```

## 图片操作

### 1. 绘制图片
```javascript
const img = new Image();
img.src = 'path/to/image.jpg';
img.onload = () => {
    ctx.drawImage(img, x, y);
    // 或者指定尺寸
    ctx.drawImage(img, x, y, width, height);
    // 裁剪并绘制
    ctx.drawImage(img, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
};
```

### 2. 像素操作
```javascript
// 获取像素数据
const imageData = ctx.getImageData(x, y, width, height);

// 修改像素数据
imageData.data[0] = 255;  // R
imageData.data[1] = 0;    // G
imageData.data[2] = 0;    // B
imageData.data[3] = 255;  // A

// 将修改后的像素数据放回画布
ctx.putImageData(imageData, x, y);
```

## 文本渲染

### 1. 文本样式
```javascript
ctx.font = '48px serif';
ctx.textAlign = 'center';     // start, end, left, right, center
ctx.textBaseline = 'middle';  // top, hanging, middle, alphabetic, ideographic, bottom

// 文本阴影
ctx.shadowColor = 'black';
ctx.shadowBlur = 5;
ctx.shadowOffsetX = 5;
ctx.shadowOffsetY = 5;
```

### 2. 绘制文本
```javascript
ctx.fillText('Hello Canvas', x, y);    // 填充文本
ctx.strokeText('Hello Canvas', x, y);   // 描边文本

// 测量文本宽度
const width = ctx.measureText('Hello Canvas').width;
```

## 动画基础

### 1. 基本动画循环
```javascript
function animate() {
    // 清除画布
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    // 绘制动画帧
    draw();
    
    // 请求下一帧
    requestAnimationFrame(animate);
}

// 启动动画
animate();
```

### 2. 性能优化建议
- 使用 `requestAnimationFrame` 而不是 `setInterval`
- 避免频繁的状态保存和恢复
- 预渲染复杂的图形到离屏 canvas
- 避免浮点数坐标以获得更清晰的渲染
- 适当使用 `clearRect` 而不是覆盖整个画布

## 调试技巧
1. 使用 console.log 输出坐标和状态
2. 临时绘制辅助线和边界框
3. 使用 chrome 开发者工具的 canvas 面板
4. 保存画布快照用于比较

## 最佳实践
1. 始终显式设置画布尺寸
2. 考虑设备像素比（DPR）以支持高分辨率显示
3. 合理使用 save() 和 restore()
4. 批量进行相似的绘制操作
5. 使用 requestAnimationFrame 进行动画
6. 适当时机清理不再需要的资源