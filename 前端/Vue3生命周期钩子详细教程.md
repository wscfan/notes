# Vue3 生命周期钩子详解

## 简介
Vue3的生命周期钩子让我们能够在组件的不同阶段执行自定义代码。与Vue2相比，Vue3的生命周期钩子在Composition API中有了新的使用方式，但整体概念保持一致。

## 基础知识
Vue3中的生命周期钩子可以通过两种方式使用：
1. 在setup()中使用onXXX形式的生命周期钩子
2. 在Options API中使用与Vue2相似的方式

## Composition API中的生命周期钩子

### 1. setup()
```javascript
import { onMounted, onUpdated, onUnmounted } from 'vue'

export default {
  setup() {
    // setup 本身就是在组件创建之前执行的
    console.log('组件初始化')
    
    onMounted(() => {
      console.log('组件已挂载')
    })
    
    return {}
  }
}
```

### 2. beforeCreate 和 created
在 Composition API 中，这两个钩子被 setup() 函数本身替代。setup() 函数在组件创建之前就被调用。

### 3. onMounted
```javascript
import { onMounted } from 'vue'

export default {
  setup() {
    onMounted(() => {
      // DOM已经可用
      console.log('组件已挂载到DOM')
      // 可以进行DOM操作
      const element = document.getElementById('my-element')
      // 可以调用第三方库初始化
      initializeThirdPartyLibrary()
    })
  }
}
```

### 4. onUpdated
```javascript
import { onUpdated, ref } from 'vue'

export default {
  setup() {
    const count = ref(0)
    
    onUpdated(() => {
      // 当响应式数据更新导致组件更新后调用
      console.log('组件已更新')
      console.log('当前计数:', count.value)
    })
    
    return {
      count
    }
  }
}
```

### 5. onUnmounted
```javascript
import { onUnmounted } from 'vue'

export default {
  setup() {
    // 设置定时器
    const timer = setInterval(() => {
      console.log('定时器执行中')
    }, 1000)
    
    onUnmounted(() => {
      // 清理工作
      clearInterval(timer)
      console.log('组件已卸载，资源已清理')
    })
  }
}
```

### 6. onBeforeMount
```javascript
import { onBeforeMount } from 'vue'

export default {
  setup() {
    onBeforeMount(() => {
      // 组件挂载到DOM之前调用
      console.log('组件即将挂载')
    })
  }
}
```

### 7. onBeforeUpdate
```javascript
import { onBeforeUpdate, ref } from 'vue'

export default {
  setup() {
    const message = ref('Hello')
    
    onBeforeUpdate(() => {
      // 组件更新之前调用
      console.log('组件即将更新')
      console.log('更新前的消息:', message.value)
    })
    
    return {
      message
    }
  }
}
```

### 8. onBeforeUnmount
```javascript
import { onBeforeUnmount } from 'vue'

export default {
  setup() {
    onBeforeUnmount(() => {
      // 组件卸载之前调用
      console.log('组件即将卸载')
    })
  }
}
```

### 9. onErrorCaptured
```javascript
import { onErrorCaptured } from 'vue'

export default {
  setup() {
    onErrorCaptured((err, instance, info) => {
      // 捕获来自子组件的错误
      console.error('捕获到错误:', err)
      console.log('错误来源组件:', instance)
      console.log('错误信息:', info)
      
      // 返回false阻止错误继续向上传播
      return false
    })
  }
}
```

## 完整的生命周期示例
```javascript
import {
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
  onErrorCaptured,
  ref
} from 'vue'

export default {
  setup() {
    const count = ref(0)
    
    console.log('setup: 组件初始化')
    
    onBeforeMount(() => {
      console.log('onBeforeMount: 组件即将挂载')
    })
    
    onMounted(() => {
      console.log('onMounted: 组件已挂载')
    })
    
    onBeforeUpdate(() => {
      console.log('onBeforeUpdate: 组件即将更新')
    })
    
    onUpdated(() => {
      console.log('onUpdated: 组件已更新')
    })
    
    onBeforeUnmount(() => {
      console.log('onBeforeUnmount: 组件即将卸载')
    })
    
    onUnmounted(() => {
      console.log('onUnmounted: 组件已卸载')
    })
    
    onErrorCaptured((err, instance, info) => {
      console.log('onErrorCaptured: 捕获到错误')
      return false
    })
    
    return {
      count
    }
  }
}
```

## 生命周期钩子最佳实践

1. 数据初始化
```javascript
export default {
  setup() {
    const data = ref(null)
    
    onMounted(async () => {
      // 在mounted时获取初始数据
      try {
        data.value = await fetchData()
      } catch (error) {
        console.error('数据获取失败:', error)
      }
    })
    
    return {
      data
    }
  }
}
```

2. 资源清理
```javascript
export default {
  setup() {
    const eventHandler = () => {
      console.log('处理窗口调整事件')
    }
    
    onMounted(() => {
      // 添加事件监听
      window.addEventListener('resize', eventHandler)
    })
    
    onUnmounted(() => {
      // 移除事件监听
      window.removeEventListener('resize', eventHandler)
    })
  }
}
```

## 注意事项

1. setup() 中没有 this
2. 生命周期钩子只能在 setup() 中同步调用
3. 确保在条件语句外调用生命周期钩子
4. 多个同类型的生命周期钩子会按照定义顺序依次执行

## 调试技巧
```javascript
export default {
  setup() {
    // 使用开发工具进行调试
    onMounted(() => {
      console.log('组件ID:', componentId)
      debugger // 可以设置断点
    })
  }
}
```