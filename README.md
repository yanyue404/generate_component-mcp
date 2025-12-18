# generate_component-mcp

让 AI 根据用户输入生成组件代码

设计一套高质量的 AI 提示词（Prompt）关键在于**结构化**和**约束性**。为了让 AI 生成的代码既符合 Vue 的最佳实践，又能精准满足你的业务需求，我为你设计了一套**模块化**的提示词框架。

你可以根据实际情况，复制对应的模块进行组合。

---

### 🚀 核心提示词框架 (Prompt Framework)

在使用时，请按照以下顺序填充内容。

#### 1. **角色设定 (Role)**
> 明确 AI 的身份，设定基调。

`你是一个资深的前端开发专家，精通 Vue.js 生态系统、TypeScript、ES6+ 语法以及主流 UI 库。请根据我的需求编写一个高质量、可维护、符合最佳实践的 Vue 组件。`

#### 2. **基础配置 (Configuration)**
> 这是最关键的部分，区分 Vue 2/3 及技术栈。

**【Vue 3 版本】**
```text
- 版本: Vue 3.x
- 语法: <script setup lang="ts"> (Composition API)
- 样式: Scoped SCSS
- UI库: Element Plus (或 Ant Design Vue / Tailwind CSS)
- 类型定义: 需要完整的 TypeScript 接口定义 (interface/type)
```

**【Vue 2 版本】**
```text
- 版本: Vue 2.6+
- 语法: Options API (或 @Component Class components)
- 样式: Scoped LESS
- UI库: Element UI
- 兼容性: 考虑 mixins 或 filters 的使用（如果需要）
```

#### 3. **组件详情 (Component Details)**
> 详细描述组件的功能、输入输出和行为。

**模板示例：**
```text
- 组件名称: UserProfileCard
- 功能描述: 展示用户头像、姓名、简介，并包含一个“关注”按钮。
- Props (输入):
  1. user: Object (包含 id, name, avatarUrl, bio), 必传
  2. isFollowing: Boolean, 默认为 false
- Events (输出):
  1. emit('toggle-follow', userId): 点击关注按钮时触发
- Slots (插槽):
  1. #actions: 底部操作栏的自定义插槽
- 逻辑要求:
  1. 头像加载失败时显示默认占位图。
  2. 关注按钮需有防抖处理 (debounce)。
```

#### 4. **代码规范与质量 (Quality & Standards)**
> 约束 AI 的编码风格，避免烂代码。

```text
请遵循以下编码规范：
1. 代码结构: 严格保持 <script>, <template>, <style> 的顺序。
2. 响应式: Vue 3 使用 ref/reactive，Vue 2 确保数据在 data() 中初始化。
3. 命名规范: Props 使用 camelCase，Template 中使用 kebab-case，变量名语义化。
4. 注释: 关键逻辑添加简短中文注释。
5. 错误处理: 包含基本的 try-catch 或错误边界处理。
6. 样式: 避免使用 ::v-deep，尽量使用 CSS 变量或工具类。
```

---

### 💡 拿来即用的组合示例 (Copy & Paste)

#### 场景一：生成一个 Vue 3 + TS 的复杂表单组件

**复制以下内容发送给 AI：**

```markdown
# Role
你是一个资深 Vue 3 开发专家。

# Tech Stack
- 框架: Vue 3.3+ (Script Setup)
- 语言: TypeScript
- UI库: Element Plus
- 样式: Tailwind CSS (无需写 style 标签，直接写 class)
- 图标: Element Plus Icons

# Task
请生成一个名为 `CreateOrderForm` 的组件。

# Requirements
1. **Props**:
   - `initialData`: 可选，用于编辑模式回显数据。
2. **Form Fields**:
   - 订单名称 (Input, 必填, max 50字)
   - 订单类型 (Select, 选项: [普通, 加急, 预约], 必填)
   - 数量 (InputNumber, min 1)
   - 期望交付日期 (DatePicker, 不能早于今天)
3. **Logic**:
   - 使用 `FormInstance` 进行表单验证。
   - 提交时调用模拟 API `submitOrder`，期间按钮显示 loading 状态。
   - 提交成功后 emit `success` 事件，失败弹出 ElMessage 错误提示。
4. **Code Quality**:
   - 定义完整的 Interface (`OrderForm`, `OrderType`)。
   - 逻辑代码抽离为独立的 composable 函数 (useOrderSubmit) 写在同一个文件中。
```

---

#### 场景二：生成一个 Vue 2 的大屏数据展示组件

**复制以下内容发送给 AI：**

```markdown
# Role
你是一个专注于数据可视化的前端工程师。

# Tech Stack
- 框架: Vue 2.7
- 样式: Scoped SCSS
- 图表库: ECharts 5.x
- 布局: Flexbox

# Task
生成一个名为 `SalesBarChart` 的组件。

# Requirements
1. **Props**:
   - `chartData`: Array<{ label: string, value: number }>
   - `title`: String
2. **Features**:
   - 组件挂载后初始化 ECharts 实例。
   - 必须监听 `chartData` 的变化并调用 `setOption` 更新图表。
   - 实现窗口 resize 监听，自动调用 `chart.resize()` (注意组件销毁时移除监听器)。
3. **Style**:
   - 容器高度固定 300px，宽度 100%。
   - 背景色为深蓝色 (#0f172a)，文字白色。
```

---

### 🌟 进阶技巧：如何让 AI 帮你优化代码？

如果你已经有代码，想让 AI 帮你重构或生成对应的组件，可以使用这个提示词：

**“Review & Refactor” 模式：**

> “请阅读以下代码逻辑 [粘贴代码]。请将其重构为一个 **Vue 3 (Script Setup + TS)** 组件。要求：
> 1. 将 Options API 转换为 Composition API。
> 2. 优化性能（例如使用 `computed` 替代复杂的 `watch`）。
> 3. 补充缺失的类型定义。
> 4. 解释你做了哪些优化。”

### 总结

好的提示词公式 = **技术栈限定** + **详细的 Props/Events 定义** + **特定的逻辑约束** + **代码风格要求**。

越早告诉 AI 不要用什么（例如：不要用 mixins，不要用 jQuery），它的输出质量就越高。
