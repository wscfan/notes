# Vue3 + TypeScript 项目搭建

## 环境准备

首先确保你的开发环境满足以下要求：

```bash
# 检查 Node.js 版本 (需要 14.0 或更高版本)
node -v

# 检查 npm 版本
npm -v

# 安装或更新 Vue CLI
npm install -g @vue/cli
```

## 创建项目

使用 Vue CLI 创建项目：

```bash
# 创建项目
npm create vue@latest

# 按提示选择需要的功能：
# ✔ Project name: … vue3-ts-project
# ✔ Add TypeScript? … Yes
# ✔ Add JSX Support? … Yes
# ✔ Add Vue Router? … Yes
# ✔ Add Pinia? … Yes
# ✔ Add ESLint? … Yes
# ✔ Add Prettier? … Yes

# 进入项目目录
cd vue3-ts-project

# 安装依赖
npm install
```

## 项目配置

### vite.config.ts 配置

```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src'),
    },
  },
  server: {
    port: 3000,
    proxy: {
      '/api': {
        target: 'http://your-api-server.com',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
    },
  },
})
```

## 目录结构规范

```
src/
├── assets/          # 静态资源
├── components/      # 公共组件
├── composables/     # 组合式函数
├── config/          # 配置文件
├── layouts/         # 布局组件
├── router/          # 路由配置
├── stores/          # Pinia 状态管理
├── styles/          # 全局样式
├── types/           # TypeScript 类型定义
├── utils/           # 工具函数
└── views/           # 页面组件
```

## 引入必要依赖

```bash
# UI 框架
npm install element-plus

# HTTP 请求
npm install axios

# 工具库
npm install lodash-es

# 类型定义
npm install @types/lodash-es -D
```

## TypeScript 配置

### tsconfig.json

```json
{
  "compilerOptions": {
    "target": "esnext",
    "module": "esnext",
    "strict": true,
    "jsx": "preserve",
    "moduleResolution": "node",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "forceConsistentCasingInFileNames": true,
    "useDefineForClassFields": true,
    "sourceMap": true,
    "baseUrl": ".",
    "types": [
      "webpack-env",
      "jest",
      "element-plus/global"
    ],
    "paths": {
      "@/*": [
        "src/*"
      ]
    },
    "lib": [
      "esnext",
      "dom",
      "dom.iterable",
      "scripthost"
    ]
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "tests/**/*.ts",
    "tests/**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

## 代码规范配置

### .eslintrc.js

```javascript
module.exports = {
  root: true,
  env: {
    node: true
  },
  extends: [
    'plugin:vue/vue3-essential',
    '@vue/typescript/recommended',
    '@vue/prettier'
  ],
  parserOptions: {
    ecmaVersion: 2020
  },
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off'
  }
}
```

## 路由配置

### src/router/index.ts

```typescript
import { createRouter, createWebHistory, RouteRecordRaw } from 'vue-router'

const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'Home',
    component: () => import('@/views/Home.vue'),
    meta: {
      title: '首页',
      requiresAuth: true
    }
  },
  {
    path: '/login',
    name: 'Login',
    component: () => import('@/views/Login.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

// 路由守卫
router.beforeEach((to, from, next) => {
  const isAuthenticated = localStorage.getItem('token')
  
  if (to.meta.requiresAuth && !isAuthenticated) {
    next('/login')
  } else {
    next()
  }
})

export default router
```

## 状态管理

### src/stores/user.ts

```typescript
import { defineStore } from 'pinia'

interface UserState {
  token: string
  userInfo: {
    id: number
    username: string
  } | null
}

export const useUserStore = defineStore('user', {
  state: (): UserState => ({
    token: localStorage.getItem('token') || '',
    userInfo: null
  }),
  
  getters: {
    isLoggedIn: (state) => !!state.token
  },
  
  actions: {
    async login(username: string, password: string) {
      try {
        // 模拟API请求
        const response = await fetch('/api/login', {
          method: 'POST',
          body: JSON.stringify({ username, password })
        })
        const data = await response.json()
        
        this.token = data.token
        this.userInfo = data.user
        localStorage.setItem('token', data.token)
      } catch (error) {
        console.error('Login failed:', error)
        throw error
      }
    },
    
    logout() {
      this.token = ''
      this.userInfo = null
      localStorage.removeItem('token')
    }
  }
})
```

## 示例代码

### 组件示例 (src/components/UserProfile.vue)

```vue
<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useUserStore } from '@/stores/user'

interface User {
  id: number
  name: string
  email: string
}

const userStore = useUserStore()
const user = ref<User | null>(null)

onMounted(async () => {
  try {
    // 获取用户信息
    const response = await fetch('/api/user')
    user.value = await response.json()
  } catch (error) {
    console.error('Failed to fetch user:', error)
  }
})
</script>

<template>
  <div class="user-profile">
    <h2>用户信息</h2>
    <div v-if="user" class="user-info">
      <p>ID: {{ user.id }}</p>
      <p>姓名: {{ user.name }}</p>
      <p>邮箱: {{ user.email }}</p>
    </div>
    <div v-else>
      加载中...
    </div>
  </div>
</template>

<style scoped>
.user-profile {
  padding: 20px;
}

.user-info {
  margin-top: 20px;
}
</style>
```

## 最佳实践

### 1. API 请求封装

```typescript
// src/utils/request.ts
import axios from 'axios'
import type { AxiosInstance, AxiosRequestConfig, AxiosResponse } from 'axios'

const instance: AxiosInstance = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
  timeout: 5000,
  headers: {
    'Content-Type': 'application/json'
  }
})

// 请求拦截器
instance.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token')
    if (token) {
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  },
  (error) => {
    return Promise.reject(error)
  }
)

// 响应拦截器
instance.interceptors.response.use(
  (response) => {
    return response.data
  },
  (error) => {
    if (error.response?.status === 401) {
      // 处理未授权
      localStorage.removeItem('token')
      window.location.href = '/login'
    }
    return Promise.reject(error)
  }
)

export default instance
```

### 2. 环境变量配置

```typescript
// .env
VITE_API_BASE_URL=http://localhost:3000

// .env.production
VITE_API_BASE_URL=https://api.yourdomain.com
```

### 3. 类型定义示例

```typescript
// src/types/user.d.ts
export interface User {
  id: number
  username: string
  email: string
  avatar?: string
  roles: string[]
}

export interface LoginParams {
  username: string
  password: string
  remember?: boolean
}

export interface LoginResponse {
  token: string
  user: User
}
```

### 4. 组合式函数示例

```typescript
// src/composables/useAuth.ts
import { ref } from 'vue'
import type { User, LoginParams } from '@/types/user'
import request from '@/utils/request'

export function useAuth() {
  const user = ref<User | null>(null)
  const loading = ref(false)
  const error = ref<string | null>(null)

  async function login(params: LoginParams) {
    try {
      loading.value = true
      error.value = null
      const response = await request.post('/auth/login', params)
      user.value = response.user
      localStorage.setItem('token', response.token)
      return response
    } catch (err) {
      error.value = err.message
      throw err
    } finally {
      loading.value = false
    }
  }

  function logout() {
    user.value = null
    localStorage.removeItem('token')
  }

  return {
    user,
    loading,
    error,
    login,
    logout
  }
}
```
