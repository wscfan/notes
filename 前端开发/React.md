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