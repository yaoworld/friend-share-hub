---
title: AbortController 应用指南 - 优雅地取消异步操作
description: 深入了解 AbortController 的实际应用，包括事件管理、网络请求取消、定时器控制等场景的最佳实践。
date: 2025-08-05
tags:
  - AbortController
  - JavaScript
  - 异步编程
  - 事件管理
  - 前端开发
---

# AbortController 应用指南 - 优雅地取消异步操作

AbortController 是现代 JavaScript 中用于取消异步操作的标准 API，它提供了一种统一的方式来管理可取消的操作。

## 基本用法

```javascript
const controller = new AbortController();
const signal = controller.signal;

// 取消操作
controller.abort();
```

## 1. 网络请求取消

### Fetch API

```javascript
const controller = new AbortController();

fetch('/api/data', {
  signal: controller.signal
})
.then(response => response.json())
.then(data => console.log(data))
.catch(err => {
  if (err.name === 'AbortError') {
    console.log('请求被取消');
  }
});

// 5秒后取消请求
setTimeout(() => controller.abort(), 5000);
```

### Axios 集成

```javascript
const controller = new AbortController();

axios.get('/api/data', {
  signal: controller.signal
})
.catch(err => {
  if (axios.isCancel(err)) {
    console.log('请求被取消');
  }
});

controller.abort();
```

## 2. 事件监听器管理

### 自动清理事件监听器

```javascript
function setupEventListeners(element, signal) {
  element.addEventListener('click', handleClick, { signal });
  element.addEventListener('mouseover', handleMouseOver, { signal });
  element.addEventListener('keydown', handleKeyDown, { signal });
}

const controller = new AbortController();
setupEventListeners(document.body, controller.signal);

// 一次性移除所有监听器
controller.abort();
```

### 组件卸载时清理

```javascript
class Component {
  constructor() {
    this.controller = new AbortController();
    this.setupListeners();
  }

  setupListeners() {
    window.addEventListener('resize', this.handleResize, {
      signal: this.controller.signal
    });
    
    document.addEventListener('click', this.handleClick, {
      signal: this.controller.signal
    });
  }

  destroy() {
    // 自动移除所有事件监听器
    this.controller.abort();
  }

  handleResize = () => { /* ... */ }
  handleClick = () => { /* ... */ }
}
```

## 3. 定时器控制

### 可取消的延时执行

```javascript
function delay(ms, signal) {
  return new Promise((resolve, reject) => {
    const timeoutId = setTimeout(resolve, ms);
    
    signal.addEventListener('abort', () => {
      clearTimeout(timeoutId);
      reject(new DOMException('操作被取消', 'AbortError'));
    });
  });
}

// 使用示例
const controller = new AbortController();

delay(3000, controller.signal)
  .then(() => console.log('延时完成'))
  .catch(err => {
    if (err.name === 'AbortError') {
      console.log('延时被取消');
    }
  });

// 提前取消
controller.abort();
```

### 可取消的轮询

```javascript
function startPolling(url, interval, signal) {
  const poll = async () => {
    try {
      const response = await fetch(url, { signal });
      const data = await response.json();
      console.log('轮询数据:', data);
      
      // 检查是否已取消
      if (!signal.aborted) {
        setTimeout(poll, interval);
      }
    } catch (err) {
      if (err.name !== 'AbortError') {
        console.error('轮询错误:', err);
      }
    }
  };
  
  poll();
}

const controller = new AbortController();
startPolling('/api/status', 5000, controller.signal);

// 停止轮询
controller.abort();
```

## 4. React Hooks 集成

### 自定义 Hook

```javascript
import { useEffect, useRef } from 'react';

function useAbortController() {
  const controllerRef = useRef();
  
  useEffect(() => {
    controllerRef.current = new AbortController();
    
    return () => {
      controllerRef.current?.abort();
    };
  }, []);
  
  return controllerRef.current;
}

// 使用示例
function DataComponent() {
  const controller = useAbortController();
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetch('/api/data', { signal: controller.signal })
      .then(res => res.json())
      .then(setData)
      .catch(err => {
        if (err.name !== 'AbortError') {
          console.error(err);
        }
      });
  }, [controller]);
  
  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```

## 5. 多操作协调

### 批量取消操作

```javascript
class TaskManager {
  constructor() {
    this.controllers = new Set();
  }
  
  createTask(taskFn) {
    const controller = new AbortController();
    this.controllers.add(controller);
    
    const cleanup = () => {
      this.controllers.delete(controller);
    };
    
    return taskFn(controller.signal).finally(cleanup);
  }
  
  abortAll() {
    this.controllers.forEach(controller => controller.abort());
    this.controllers.clear();
  }
}

// 使用示例
const taskManager = new TaskManager();

taskManager.createTask(signal => 
  fetch('/api/data1', { signal })
);

taskManager.createTask(signal => 
  fetch('/api/data2', { signal })
);

// 取消所有任务
taskManager.abortAll();
```

## 6. 信号组合

### 多个信号的组合

```javascript
function combineSignals(...signals) {
  const controller = new AbortController();
  
  const onAbort = () => controller.abort();
  
  signals.forEach(signal => {
    if (signal.aborted) {
      controller.abort();
      return;
    }
    signal.addEventListener('abort', onAbort);
  });
  
  return controller.signal;
}

// 使用示例
const userController = new AbortController();
const timeoutController = new AbortController();

// 5秒超时
setTimeout(() => timeoutController.abort(), 5000);

const combinedSignal = combineSignals(
  userController.signal,
  timeoutController.signal
);

fetch('/api/data', { signal: combinedSignal })
  .catch(err => {
    if (err.name === 'AbortError') {
      console.log('请求被取消（用户取消或超时）');
    }
  });
```

## 最佳实践

1. **及时清理**：确保在组件卸载或页面离开时调用 `abort()`
2. **错误处理**：始终检查 `AbortError` 并适当处理
3. **信号传递**：将 signal 作为参数传递给需要取消的函数
4. **避免内存泄漏**：在不需要时及时移除 AbortController 引用

## 浏览器兼容性

AbortController 在现代浏览器中得到良好支持：
- Chrome 66+
- Firefox 57+
- Safari 11.1+
- Edge 16+

对于旧浏览器，可以使用 polyfill：

```bash
npm install abortcontroller-polyfill
```

```javascript
import 'abortcontroller-polyfill/dist/polyfill-patch-fetch';
```

## 总结

AbortController 提供了一种标准化的方式来取消异步操作，特别适用于：
- 网络请求管理
- 事件监听器清理
- 定时器控制
- 组件生命周期管理
- 复杂异步流程控制

合理使用 AbortController 可以有效防止内存泄漏，提升应用性能和用户体验。