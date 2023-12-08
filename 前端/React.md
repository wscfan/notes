# React笔记

## 常用模块

### styled-components

处理react中样式的模块

**安装：**

```bash
npm i styled-components -S
```

**用法：**

```js
import styled from 'styled-components'

export const SelfLink = styled.div`
    width: 100px;
    height: 50px;
    background: skyblue;
    border: 3px solid #ccc;
`;
```

### react-transiton-group

react动画库

**安装：**

```bash
npm i react-transition-group -S
```

### redux-thunk

用于异步操作的中间件

**用法：**

```js
const store = createStore(
  reducer,
  applyMiddleware(thunk, logger)
);
```

### redux-logger

用户日志记录的中间件

