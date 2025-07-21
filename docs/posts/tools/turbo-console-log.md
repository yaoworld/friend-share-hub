---
title: Turbo Console Log：让调试变得更高效的 VS Code 插件
description: 探索 Turbo Console Log，一个能够自动化生成有意义日志消息的 VS Code 插件，让 JavaScript 和 TypeScript 调试变得更加高效
tags:
  - VS Code
  - 插件
  - 调试
  - JavaScript
  - TypeScript
  - 开发工具
createTime: 2025-07-21
---

# Turbo Console Log：让调试变得更高效的 VS Code 插件

## 什么是 Turbo Console Log？

Turbo Console Log 是一个专为 JavaScript 和 TypeScript 开发者设计的 VS Code 插件，它能够自动化生成有意义的日志消息，让调试过程变得更加高效。<mcreference link="https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log" index="1">1</mcreference> 这个插件深受开发者喜爱，已被近 200 万开发者信赖使用。<mcreference link="https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log" index="1">1</mcreference>

## 为什么选择 Turbo Console Log？

在日常开发中，`console.log` 是 JavaScript 开发者最好的朋友。<mcreference link="https://stevenmercatante.com/my-favorite-vscode-extensions/" index="4">4</mcreference> 它无需安装或导入，易于识别，并且能够完成调试任务。但是，每次手动输入完整的 console.log 语句既繁琐又耗时。

Turbo Console Log 解决了这个痛点，它将简单的控制台日志记录转变为一个流线化的自动化过程。<mcreference link="https://www.turboconsolelog.io/documentation/overview/motivation" index="3">3</mcreference> 只需选择变量并按下快捷键，就能自动插入一个信息丰富的日志消息。

## 核心功能特性

### 🚀 快速插入有意义的日志消息

**基本用法：**
1. 选择要调试的变量
2. 按下快捷键：`Ctrl + Alt + L`（Windows）或 `Ctrl + Option + L`（Mac）
3. 插件会在下一行自动插入格式化的日志消息

**生成的日志格式：**
```javascript
console.log("🚀 ~ FileName:LineNumber ~ FunctionName ~ VariableName:", VariableName)
```

### 🎯 多光标支持

Turbo Console Log 支持多光标操作，这意味着你可以同时为多个变量生成日志消息，大大提高调试效率。<mcreference link="https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log" index="1">1</mcreference> 这个功能在需要同时调试多个变量时特别有用。

### 🛠️ 日志管理功能

插件提供了完整的日志管理快捷键：<mcreference link="https://www.salesforcebolt.com/2022/05/make-debugging-much-easier-.html" index="5">5</mcreference>

- **注释所有日志**：`Shift + Alt + C`
- **取消注释所有日志**：`Shift + Alt + U`
- **删除所有日志**：`Shift + Alt + D`
- **修正日志消息**：`Shift + Alt + X`

### 🎨 高度可定制化

Turbo Console Log 允许你根据个人喜好和项目需求自定义日志格式：<mcreference link="https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log" index="1">1</mcreference>

- ✅ 前缀和后缀
- ✅ 日志函数类型（console.log, console.warn, console.error 等）
- ✅ 引号类型（单引号、双引号或反引号）
- ✅ 文件名和行号包含
- ✅ 类名和函数名包含

## 安装方法

### 通过 VS Code 扩展市场安装

1. 打开 VS Code
2. 点击左侧活动栏的扩展图标（或按 `Ctrl+Shift+X`）
3. 在搜索框中输入 "Turbo Console Log"
4. 找到由 ChakrounAnas 开发的插件
5. 点击 "Install" 按钮

### 通过命令行安装

```bash
code --install-extension ChakrounAnas.turbo-console-log
```

## 详细配置选项

### 基础设置

在 VS Code 设置中搜索 "Turbo Console Log"，你可以找到以下配置选项：<mcreference link="https://www.salesforcebolt.com/2022/05/make-debugging-much-easier-.html" index="5">5</mcreference>

#### 日志消息格式

```json
{
  "turboConsoleLog.logMessagePrefix": "🚀",
  "turboConsoleLog.logMessageSuffix": ":",
  "turboConsoleLog.quote": "\"\"",
  "turboConsoleLog.addSemicolonInTheEnd": true,
  "turboConsoleLog.delimiter": "~"
}
```

#### 日志函数类型

```json
{
  "turboConsoleLog.logType": "log",
  "turboConsoleLog.logFunction": "console.log"
}
```

可选值：
- `"log"` - 标准日志
- `"warn"` - 警告日志
- `"error"` - 错误日志
- `"debug"` - 调试日志
- `"table"` - 表格日志

#### 上下文信息

```json
{
  "turboConsoleLog.insertEnclosingClass": false,
  "turboConsoleLog.insertEnclosingFunction": true,
  "turboConsoleLog.includeFileNameAndLineNum": true
}
```

这些设置可以帮助你在日志中包含更多上下文信息：<mcreference link="https://www.turboconsolelog.io/documentation/overview/motivation" index="3">3</mcreference>

- **insertEnclosingClass**：包含封闭类名
- **insertEnclosingFunction**：包含封闭函数名
- **includeFileNameAndLineNum**：包含文件名和行号

#### 日志包装

```json
{
  "turboConsoleLog.wrapLogMessage": false
}
```

启用后，日志消息会被包装在多行中，提高可读性。

## 实际使用示例

### 基础示例

**原始代码：**
```javascript
function calculateTotal(price, tax) {
  const total = price + (price * tax);
  return total;
}
```

**选择 `total` 变量并按 `Ctrl + Alt + L` 后：**
```javascript
function calculateTotal(price, tax) {
  const total = price + (price * tax);
  console.log("🚀 ~ calculateTotal ~ total:", total);
  return total;
}
```

### 包含类和函数信息的示例

**原始代码：**
```javascript
class ShoppingCart {
  addItem(item) {
    const itemId = item.id;
    // 选择 itemId 变量
  }
}
```

**生成的日志：**
```javascript
class ShoppingCart {
  addItem(item) {
    const itemId = item.id;
    console.log("🚀 ~ ShoppingCart ~ addItem ~ itemId:", itemId);
  }
}
```

### 多光标示例

**原始代码：**
```javascript
function processData(data) {
  const name = data.name;     // 选择 name
  const age = data.age;       // 选择 age
  const email = data.email;   // 选择 email
}
```

**同时选择多个变量并按快捷键后：**
```javascript
function processData(data) {
  const name = data.name;
  console.log("🚀 ~ processData ~ name:", name);
  const age = data.age;
  console.log("🚀 ~ processData ~ age:", age);
  const email = data.email;
  console.log("🚀 ~ processData ~ email:", email);
}
```

## 高级功能

### AST 引擎支持

Turbo Console Log 最新版本引入了 AST（抽象语法树）解析功能，提供了更精确的日志插入位置：<mcreference link="https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log" index="1">1</mcreference>

- 📐 **AST 支持**：为三元表达式和函数参数引入 AST 解析
- 🧠 **更可靠的日志放置**：提高三元表达式和函数参数的准确性
- 🧪 **测试覆盖**：新增测试用例验证 AST 逻辑

### Turbo PRO 功能预览

即将推出的 Turbo PRO 版本将包含：<mcreference link="https://www.turboconsolelog.io/" index="2">2</mcreference>

- 🚀 **原生 VSCode 面板**：专用的扩展面板
- 🌲 **工作区日志列表**：通过 VSCode Tree View API 列出当前文件夹/工作区的日志
- ⚡️ **实时同步**：实时同步当前文件夹/工作区日志
- 🚦 **上下文菜单**：在文件级别执行 turbo 命令操作
- 🖊️ **自动修正**：当树同步时自动修正日志消息的行号和文件名

## 最佳实践

### 1. 合理使用日志级别

根据不同的调试需求选择合适的日志级别：

```javascript
// 一般信息
console.log("🚀 ~ functionName ~ variable:", variable);

// 警告信息
console.warn("⚠️ ~ functionName ~ variable:", variable);

// 错误信息
console.error("❌ ~ functionName ~ variable:", variable);
```

### 2. 及时清理日志

使用 `Shift + Alt + D` 快捷键及时删除调试完成后的日志消息，保持代码整洁。

### 3. 自定义前缀

为不同类型的调试设置不同的前缀，便于在控制台中快速识别：

```json
{
  "turboConsoleLog.logMessagePrefix": "[DEBUG]"
}
```

### 4. 团队协作

在团队项目中，建议统一 Turbo Console Log 的配置，确保所有成员生成的日志格式一致。

## 性能和兼容性

### 支持的语言

- ✅ JavaScript
- ✅ TypeScript
- ✅ JSX/TSX
- ✅ Vue.js
- ✅ React
- ✅ Angular

### 性能特点

- 🚀 **轻量级**：不会影响 VS Code 的启动速度
- ⚡ **快速响应**：即时生成日志消息
- 🧠 **智能识别**：准确识别变量作用域和上下文

## 社区和支持

### 获取帮助

- 📖 **官方文档**：[turboconsolelog.io](https://www.turboconsolelog.io/)
- 💬 **GitHub 讨论**：参与社区讨论和反馈
- 📧 **邮件支持**：support@turboconsolelog.io

### 贡献代码

Turbo Console Log 是开源项目，欢迎社区贡献：<mcreference link="https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log" index="1">1</mcreference>

- 🐛 报告问题
- 💡 建议新功能
- 🔧 提交 Pull Request

## 总结

Turbo Console Log 是一个功能强大且高度可定制的 VS Code 插件，它通过自动化日志消息生成，显著提高了 JavaScript 和 TypeScript 开发者的调试效率。<mcreference link="https://stevenmercatante.com/my-favorite-vscode-extensions/" index="4">4</mcreference> 无论你是初学者还是经验丰富的开发者，这个插件都能为你的开发工作流程带来实质性的改进。

### 主要优势

- ⚡ **提高效率**：快速生成格式化的日志消息
- 🎯 **精确定位**：包含文件名、行号、函数名等上下文信息
- 🛠️ **易于管理**：一键注释、取消注释或删除所有日志
- 🎨 **高度定制**：支持个性化配置
- 👥 **社区支持**：活跃的开源社区

立即安装 Turbo Console Log，让你的调试过程变得更加高效和愉快！

## 相关资源

- [VS Code 扩展市场](https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log)
- [官方网站](https://www.turboconsolelog.io/)
- [GitHub 仓库](https://github.com/Chakroun-Anas/turbo-console-log)
- [完整文档](https://www.turboconsolelog.io/documentation/overview/motivation)

---

*让调试变得简单高效，从安装 Turbo Console Log 开始！*