# JavaScript 获取真实时间

## 基本方法

### 1. Date 对象

最基本的获取时间方法是使用 `Date` 对象：

```javascript
const now = new Date();
console.log(now.toString());  // 输出完整时间字符串
console.log(now.toLocaleString());  // 根据本地时间格式输出
```

### 2. 时间戳

获取毫秒级时间戳：

```javascript
// 方法一
const timestamp1 = Date.now();

// 方法二
const timestamp2 = new Date().getTime();

// 方法三
const timestamp3 = +new Date();
```

## 高精度时间

### 1. Performance API

如果需要高精度计时，可以使用 `Performance` API：

```javascript
const startTime = performance.now();
// 执行一些操作
const endTime = performance.now();
const duration = endTime - startTime;  // 精确到微秒级别
```

### 2. 服务器时间同步

为确保时间准确性，建议与服务器时间同步：

```javascript
async function getServerTime() {
    try {
        const response = await fetch('/api/time');
        const serverTime = await response.json();
        return new Date(serverTime);
    } catch (error) {
        console.error('获取服务器时间失败:', error);
        return new Date();
    }
}
```

## 格式化时间

### 1. 使用 Intl.DateTimeFormat

```javascript
const date = new Date();
const formatter = new Intl.DateTimeFormat('zh-CN', {
    year: 'numeric',
    month: '2-digit',
    day: '2-digit',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
    hour12: false
});
console.log(formatter.format(date));  // 2024-11-19 14:30:45
```

### 2. 自定义格式化函数

```javascript
function formatDate(date) {
    const year = date.getFullYear();
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const day = String(date.getDate()).padStart(2, '0');
    const hours = String(date.getHours()).padStart(2, '0');
    const minutes = String(date.getMinutes()).padStart(2, '0');
    const seconds = String(date.getSeconds()).padStart(2, '0');
    
    return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
}
```

## 最佳实践

1. **时区处理**
```javascript
// 获取用户时区
const userTimeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;

// 转换时间到特定时区
const date = new Date();
const options = {
    timeZone: 'Asia/Shanghai',
    hour12: false,
    year: 'numeric',
    month: '2-digit',
    day: '2-digit',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit'
};
console.log(new Intl.DateTimeFormat('zh-CN', options).format(date));
```

2. **性能考虑**
```javascript
// 缓存 DateTimeFormat 实例
const formatter = new Intl.DateTimeFormat('zh-CN', {
    hour12: false,
    year: 'numeric',
    month: '2-digit',
    day: '2-digit',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit'
});

// 重复使用该实例
function getFormattedTime() {
    return formatter.format(new Date());
}
```

## 注意事项

1. `Date.now()` 返回的是 UTC 时间戳，与本地时区无关
2. `new Date()` 创建的是本地时间对象
3. 服务器时间同步时需考虑网络延迟
4. 处理时区时要特别注意夏令时的影响
5. 高精度计时场景建议使用 `Performance` API

## 总结

在实际应用中，建议：
- 存储时使用时间戳
- 显示时使用格式化后的时间字符串
- 需要高精度计时时使用 `Performance` API
- 关键业务依赖时间时与服务器时间同步