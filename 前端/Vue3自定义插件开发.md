# Vue3 自定义插件开发

## 插件简介

Vue3 插件是一种能为 Vue 应用添加全局功能的工具。插件可以包含：
- 全局组件注册
- 全局指令添加
- 全局方法注入
- 全局 mixin 混入
- 向 Vue 应用实例注入属性

## 插件的基本结构

Vue3 插件应该暴露一个 `install` 方法，该方法将在插件安装时被调用：

```typescript
interface Plugin {
  install: (app: App, options?: any) => void
}
```

## 开发一个简单插件

让我们通过一个实例来了解如何开发一个基础插件。这个插件将提供一个全局提示框功能。

### 1. 创建插件文件

```typescript
// plugins/toast/index.ts
import { App } from 'vue'
import Toast from './Toast.vue'

export default {
  install: (app: App, options = {}) => {
    // 创建一个全局提示框组件
    const toast = {
      show(message: string) {
        // 创建提示框逻辑
      }
    }
    
    // 注册全局组件
    app.component('Toast', Toast)
    
    // 注入全局属性
    app.config.globalProperties.$toast = toast
    
    // 通过 provide/inject 提供依赖
    app.provide('toast', toast)
  }
}
```

### 2. 创建组件文件

```vue
<!-- plugins/toast/Toast.vue -->
<template>
  <transition name="toast-fade">
    <div v-if="visible" class="toast-wrapper">
      {{ message }}
    </div>
  </transition>
</template>

<script>
import { ref, defineComponent } from 'vue'

export default defineComponent({
  name: 'Toast',
  props: {
    message: {
      type: String,
      required: true
    }
  },
  setup() {
    const visible = ref(false)
    
    return {
      visible
    }
  }
})
</script>

<style scoped>
.toast-wrapper {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  padding: 10px 20px;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  border-radius: 4px;
}

.toast-fade-enter-active,
.toast-fade-leave-active {
  transition: opacity 0.3s ease;
}

.toast-fade-enter-from,
.toast-fade-leave-to {
  opacity: 0;
}
</style>
```

### 3. 使用插件

```typescript
// main.ts
import { createApp } from 'vue'
import App from './App.vue'
import ToastPlugin from './plugins/toast'

const app = createApp(App)

// 安装插件
app.use(ToastPlugin, {
  duration: 3000 // 配置选项
})

app.mount('#app')
```

### 4. 在组件中使用

```vue
<template>
  <button @click="showToast">显示提示</button>
</template>

<script>
import { getCurrentInstance } from 'vue'

export default {
  setup() {
    const { proxy } = getCurrentInstance()
    
    const showToast = () => {
      proxy.$toast.show('这是一条提示消息！')
    }
    
    return {
      showToast
    }
  }
}
</script>
```

## 插件的高级特性

### 1. TypeScript 支持

为了更好的类型支持，我们可以扩展 Vue 的类型定义：

```typescript
// types/index.d.ts
import { Toast } from './toast'

declare module '@vue/runtime-core' {
  export interface ComponentCustomProperties {
    $toast: Toast
  }
}
```

### 2. 插件选项处理

```typescript
interface ToastOptions {
  duration?: number
  position?: 'top' | 'bottom'
  theme?: 'light' | 'dark'
}

export default {
  install: (app: App, options: ToastOptions = {}) => {
    const defaultOptions: ToastOptions = {
      duration: 3000,
      position: 'top',
      theme: 'dark'
    }
    
    const mergedOptions = { ...defaultOptions, ...options }
    
    // 使用合并后的选项
  }
}
```

### 3. 生命周期钩子

插件可以在安装时注册全局生命周期钩子：

```typescript
app.mixin({
  mounted() {
    console.log('Component mounted')
  }
})
```
