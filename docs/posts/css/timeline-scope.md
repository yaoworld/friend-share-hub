---
title: CSS timeline-scope 属性详解
description: 深入了解 CSS timeline-scope 属性，学习如何扩展动画时间线的作用域，实现更灵活的滚动驱动动画效果
date: 2025-08-06
tags:
  - CSS
  - Animation
  - Scroll-driven
  - Timeline
---

# CSS timeline-scope 属性详解

`timeline-scope` 是 CSS 中用于修改命名动画时间线作用域的属性。它允许我们将时间线的作用域从默认的直接子元素扩展到更广泛的元素范围。

## 基本概念

默认情况下，命名时间线（通过 `scroll-timeline-name` 或 `view-timeline-name` 声明）只能作为其直接子元素的控制时间线。`timeline-scope` 属性可以扩展这个作用域范围。

### 语法

```css
timeline-scope: none;
timeline-scope: --custom-timeline;
timeline-scope: --timeline1, --timeline2;
```

### 属性值

- `none`：不改变时间线作用域（默认值）
- `<dashed-ident>`：指定要扩展作用域的时间线名称，必须以 `--` 开头

## 实际应用示例

下面是一个完整的示例，展示如何使用 `timeline-scope` 实现跨元素的滚动驱动动画。

### 完整代码示例

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Timeline Scope 演示</title>
  <style>
    .container {
      margin: 0;
      height: 400px;
      display: flex;
      border: 2px solid #ddd;
      border-radius: 8px;
      overflow: hidden;
      font-family: Arial, sans-serif;
      
      /* 🔑 关键：扩展时间线作用域到整个容器 */
      timeline-scope: --demo-scroller;
    }

    .content,
    .scroller {
      flex: 1;
    }

    .scroller {
      overflow: scroll;
      /* 🔑 关键：定义滚动时间线 */
      scroll-timeline-name: --demo-scroller;
      background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
      position: relative;
    }

    .long-element {
      height: 1200px;
      background: linear-gradient(to bottom, 
        rgba(255,255,255,0.1) 0%, 
        rgba(255,255,255,0.3) 50%, 
        rgba(255,255,255,0.1) 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 18px;
      font-weight: bold;
      text-align: center;
    }

    .animated-box {
      width: 80px;
      height: 80px;
      border-radius: 12px;
      background: linear-gradient(45deg, #667eea, #764ba2);
      margin: 20px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
      
      /* 🔑 关键：应用时间线到动画 */
      animation: rotate-and-move 1s linear;
      animation-timeline: --demo-scroller;
      animation-fill-mode: both;
    }

    @keyframes rotate-and-move {
      from {
        transform: rotate(0deg) translateX(0px) scale(1);
        border-radius: 12px;
      }
      
      50% {
        transform: rotate(360deg) translateX(120px) scale(1.3);
        border-radius: 50%;
        background: linear-gradient(45deg, #f093fb, #f5576c);
      }
      
      to {
        transform: rotate(720deg) translateX(240px) scale(1);
        border-radius: 12px;
        background: linear-gradient(45deg, #4facfe, #00f2fe);
      }
    }

    .info-box {
      padding: 15px;
      background: #f8f9fa;
      border-radius: 6px;
      margin: 20px;
      font-size: 14px;
      color: #666;
      border-left: 4px solid #007acc;
    }
  </style>
</head>
<body>
  <div class="info-box">
    💡 <strong>演示说明：</strong>滚动右侧彩色区域，观察左侧方块的动画效果。
  </div>

  <div class="container">
    <div class="content">
      <div class="animated-box"></div>
      <p style="margin: 20px; color: #666; font-size: 14px;">
        这个方块的动画由右侧滚动区域控制，<br>
        展示了 timeline-scope 的跨元素作用域功能。
      </p>
    </div>

    <div class="scroller">
      <div class="long-element">
        📜 滚动这个区域<br>
        控制左侧动画
      </div>
    </div>
  </div>
</body>
</html>
```

### 交互式演示

<iframe src="/friend-share-hub/demos/timeline-scope-demo.html" width="100%" height="600" frameborder="0" style="border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.1);"></iframe>

### 使用 Demo 容器

你也可以使用 VuePress 的 demo 容器来创建简单的演示：

::: demo 基础 timeline-scope 示例

```html
<div class="timeline-demo">
  <div class="demo-content">
    <div class="demo-box"></div>
    <p>滚动右侧区域控制动画</p>
  </div>
  <div class="demo-scroll">
    <div class="scroll-content">滚动内容</div>
  </div>
</div>

<style>
.timeline-demo {
  display: flex;
  height: 200px;
  border: 1px solid #ddd;
  border-radius: 6px;
  overflow: hidden;
  timeline-scope: --simple-scroller;
}

.demo-content, .demo-scroll {
  flex: 1;
}

.demo-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #f9f9f9;
}

.demo-box {
  width: 40px;
  height: 40px;
  background: #007acc;
  border-radius: 4px;
  animation: simple-rotate 1s linear;
  animation-timeline: --simple-scroller;
  animation-fill-mode: both;
}

.demo-scroll {
  overflow: scroll;
  scroll-timeline-name: --simple-scroller;
  background: linear-gradient(45deg, #ff9a9e, #fecfef);
}

.scroll-content {
  height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: bold;
}

@keyframes simple-rotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>
```

:::

### 在线编辑器

你也可以在以下在线编辑器中尝试 timeline-scope：

- [CodePen 示例](https://codepen.io) - 创建新的 Pen 并粘贴上述代码
- [CodeSandbox](https://codesandbox.io) - 适合更复杂的项目演示

## 代码解析

### HTML 结构

```html
<div class="container">
  <!-- 动画元素区域 -->
  <div class="content">
    <div class="box animation"></div>
  </div>
  
  <!-- 滚动控制区域 -->
  <div class="scroller">
    <div class="long-element"></div>
  </div>
</div>
```

### 关键 CSS 实现

```css
/* 1. 在容器上设置 timeline-scope */
.container {
  timeline-scope: --my-scroller;
  display: flex;
}

/* 2. 定义滚动时间线 */
.scroller {
  overflow: scroll;
  scroll-timeline-name: --my-scroller;
}

/* 3. 应用时间线到动画 */
.animation {
  animation: my-animation 1s linear;
  animation-timeline: --my-scroller;
}

@keyframes my-animation {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
```

## 工作原理

1. **默认作用域限制**：通常情况下，`scroll-timeline-name` 定义的时间线只能被其直接子元素使用

2. **作用域扩展**：通过在父容器上设置 `timeline-scope: --timeline-name`，可以将时间线的作用域扩展到该容器及其所有后代元素

3. **跨元素控制**：这样就实现了一个元素的滚动控制另一个非子元素的动画效果

## 浏览器兼容性

`timeline-scope` 属性是较新的 CSS 特性，目前支持情况：

- ✅ Chrome 115+
- ✅ Edge 115+
- ❌ Firefox（开发中）
- ❌ Safari（开发中）

## 实用技巧

### 1. 多时间线管理

```css
.container {
  /* 同时管理多个时间线 */
  timeline-scope: --scroll-timeline, --view-timeline;
}
```

### 2. 嵌套作用域

```css
.outer {
  timeline-scope: --outer-timeline;
}

.inner {
  timeline-scope: --inner-timeline;
  /* 可以同时访问外层和内层时间线 */
}
```

### 3. 条件性应用

```css
@supports (timeline-scope: --test) {
  .modern-browser {
    timeline-scope: --advanced-timeline;
  }
}
```

## 总结

`timeline-scope` 属性为 CSS 动画提供了更大的灵活性，特别是在创建复杂的滚动驱动动画时。它打破了传统的父子关系限制，允许我们创建更加丰富和互动的用户界面效果。

虽然目前浏览器支持还不够广泛，但这个特性代表了 CSS 动画发展的重要方向，值得前端开发者关注和学习。