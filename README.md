# Skills 使用手册

## 1. 目标

这份手册用于说明当前已安装的自定义 skill 应该在什么场景下使用、哪些 skill 容易混淆、怎么组合使用最顺手。

当前手册覆盖以下 10 个自定义 skill：

- `frontend-business-delivery`
- `admin-crud-page-builder`
- `cross-project-module-port`
- `api-service-typing`
- `generate-vue-types`
- `debug-vue-bugs`
- `review-vue-pr`
- `refactor-vue-components`
- `analyze-code-logic`
- `multi-env-build-doctor`

---

## 2. 总体使用原则

### 2.1 默认主 skill

默认优先使用：

- `$frontend-business-delivery`

它适合作为日常业务开发主入口。

### 2.2 专项任务优先走专项 skill

如果任务已经非常明确，不要把所有事情都交给主 skill。

例如：

- 做后台 CRUD 页面：`$admin-crud-page-builder`
- 查 Vue3 bug：`$debug-vue-bugs`
- 做 PR Review：`$review-vue-pr`
- 看懂代码逻辑：`$analyze-code-logic`
- 跨项目迁移模块：`$cross-project-module-port`

### 2.3 推荐工作链路

很多任务不是只用一个 skill，而是按顺序组合：

- 先看懂：分析类 skill
- 再实现：交付类 skill
- 再把关：review 类 skill

---

## 3. Skill 总览

### 3.1 交付型

#### `$frontend-business-delivery`

适合：

- 日常业务需求开发
- 页面、表单、列表、详情、弹窗、抽屉
- 接口对接、状态管理、权限、埋点
- 缺陷修复
- 小到中等规模重构

不适合：

- 只想做专项分析
- 只想做 PR Review
- 明确是 CRUD 页面模板化搭建

示例：

```text
$frontend-business-delivery 请在当前项目里新增一个客户详情页，并接好接口、权限和表单校验
```

#### `$admin-crud-page-builder`

适合：

- 后台管理类 CRUD 页面
- 搜索、筛选、分页、表格、行操作
- 新增/编辑弹窗或抽屉
- 详情、批量操作、导出

不适合：

- 非 CRUD 型复杂页面
- 纯重构或纯调试任务

示例：

```text
$admin-crud-page-builder 新增一个供应商管理页面，包含搜索、分页、增删改查和详情
```

#### `$cross-project-module-port`

适合：

- 把 A 项目的页面/模块迁到 B 项目
- 迁移时补 router、api、store、权限、样式依赖
- 多个相似后台项目之间搬功能

不适合：

- 单项目内新开发
- 只是复制一个独立组件

示例：

```text
$cross-project-module-port 把 yeb-erp-pc 的订单列表迁到 mpm-neobill-pc，并补齐依赖
```

---

### 3.2 类型与接口型

#### `$generate-vue-types`

适合：

- 根据 JSON、接口返回值生成 Vue3 可用类型
- 快速补 interface/type
- 提取枚举、分页、列表项类型

不适合：

- 想把 api/service 层一起建好

示例：

```text
$generate-vue-types 根据这段接口返回 JSON 生成类型定义
```

#### `$api-service-typing`

适合：

- 生成类型安全的 api 层和 service 层
- 请求参数、响应 DTO、分页模型
- DTO 转视图模型
- 错误处理约定

不适合：

- 只想快速要几个类型

示例：

```text
$api-service-typing 根据这组接口定义生成 api.ts、service.ts 和类型
```

---

### 3.3 分析与质量型

#### `$debug-vue-bugs`

适合：

- Vue3 bug 根因定位
- `ref` / `reactive` 使用错误
- `watch` / `watchEffect` 误用
- `computed` 不更新
- props/emits 问题
- 异步覆盖、表单状态错乱

不适合：

- 单纯想了解代码用途

示例：

```text
$debug-vue-bugs 这是报错和相关 Vue3 代码，帮我定位根因并给出修复代码
```

#### `$review-vue-pr`

适合：

- Vue3 + TypeScript 代码评审
- 按严重程度输出问题
- 找 bug、风险、缺测试、响应式问题、性能问题

不适合：

- 直接写实现代码

示例：

```text
$review-vue-pr review 这段改动，按严重程度列出问题和修改建议
```

#### `$refactor-vue-components`

适合：

- 重构复杂 Vue3 组件
- 拆 composables、services、状态边界
- 降低耦合，提升复用性

不适合：

- 小范围修补
- 纯 bug 修复

示例：

```text
$refactor-vue-components 重构这个大组件，拆分 UI、状态和业务逻辑
```

#### `$analyze-code-logic`

适合：

- 结构化梳理代码整体作用
- 执行流程
- 核心逻辑
- 关键变量
- 数据流
- 依赖关系

不适合：

- 查 bug
- 直接做功能交付

示例：

```text
$analyze-code-logic 分析这段代码的整体作用、执行流程和数据流
```

---

### 3.4 工程环境型

#### `$multi-env-build-doctor`

适合：

- `dev/test/test2/pre/prod` 多环境问题
- env 配置差异
- proxy、baseURL、打包差异
- Vite、Vue CLI、uni-app 构建问题

不适合：

- 普通业务逻辑 bug

示例：

```text
$multi-env-build-doctor 为什么 test 正常，但 pre 环境接口全部报错
```

---

## 4. 最容易混淆的 Skill

### 4.1 `$frontend-business-delivery` vs `$admin-crud-page-builder`

区别：

- 前者是通用业务开发主 skill
- 后者是后台 CRUD 页面专用 skill

判断方法：

- 只要页面明显是“搜索 + 表格 + 分页 + 弹窗表单”，优先后者
- 其他普通业务页面优先前者

### 4.2 `$generate-vue-types` vs `$api-service-typing`

区别：

- 前者只偏类型定义
- 后者覆盖接口层、service 层、DTO、错误处理

判断方法：

- 只想补类型：`generate-vue-types`
- 想把接口层一起做规范：`api-service-typing`

### 4.3 `$debug-vue-bugs` vs `$analyze-code-logic`

区别：

- 前者是定位问题
- 后者是理解代码

判断方法：

- 目标是“为什么坏了”：`debug-vue-bugs`
- 目标是“这段代码到底怎么工作”：`analyze-code-logic`

### 4.4 `$refactor-vue-components` vs `$review-vue-pr`

区别：

- 前者是主动改结构
- 后者是先找问题并评审风险

判断方法：

- 已决定重构：`refactor-vue-components`
- 还在看这份代码值不值得改、哪里有问题：`review-vue-pr`

---

## 5. 推荐组合用法

### 5.1 看懂旧代码再改

```text
$analyze-code-logic -> $frontend-business-delivery
```

适合：

- 接手旧页面
- 对已有模块做功能扩展

### 5.2 从接口到页面

```text
$generate-vue-types -> $api-service-typing -> $admin-crud-page-builder
```

适合：

- 新接口刚出来
- 要快速落一个完整后台页面

### 5.3 跨项目搬页面

```text
$analyze-code-logic -> $cross-project-module-port -> $review-vue-pr
```

适合：

- 多项目之间迁模块
- 搬完还想做一次风险把关

### 5.4 组件治理

```text
$analyze-code-logic -> $refactor-vue-components -> $review-vue-pr
```

适合：

- 先理解大组件
- 再重构
- 最后 review

### 5.5 Bug 修复

```text
$debug-vue-bugs -> $frontend-business-delivery
```

适合：

- 先定位根因
- 再在真实项目里补正式修复

---

## 6. 结合你当前项目的推荐用法

你的项目主要是：

- 多个相似的 Vue3 后台项目
- 一部分新项目有 Pinia / TypeScript
- 一部分旧项目仍有 Vuex
- 存在多环境构建差异

因此最常用的 skill 组合应当是：

- 日常开发：`$frontend-business-delivery`
- 标准后台页面：`$admin-crud-page-builder`
- 跨项目复用：`$cross-project-module-port`
- 接口层规范化：`$api-service-typing`
- 环境问题：`$multi-env-build-doctor`

可以理解为：

- 默认主力：`frontend-business-delivery`
- 高频专项：`admin-crud-page-builder`
- 多项目特色：`cross-project-module-port`
- 工程兜底：`multi-env-build-doctor`

---

## 7. 建议的使用习惯

### 7.1 默认入口

如果不确定该用哪个，先判断：

- 是不是业务开发
- 是不是 CRUD 页面
- 是不是跨项目迁移
- 是不是 bug
- 是不是 review

大多数情况下：

- 业务开发：`$frontend-business-delivery`
- CRUD 页面：`$admin-crud-page-builder`

### 7.2 提高准确率的方法

调用时尽量带上：

- 项目路径
- 目标文件
- 参考页面
- 接口信息
- 期望交互
- 是否要补权限、埋点、类型、测试

例如：

```text
$frontend-business-delivery 参考 src/pages/order/list 页面，在当前项目新增一个发票管理页面，包含搜索、分页、导出、编辑弹窗和权限控制
```

### 7.3 提高复用率的方法

遇到重复任务时优先使用已有 skill，而不是重新描述一大段背景。

例如：

- 新增管理页：直接用 `admin-crud-page-builder`
- 搬功能：直接用 `cross-project-module-port`
- 接口重整：直接用 `api-service-typing`

---

## 8. 当前建议

你这套 skill 已经够强了，下一步重点不是继续无节制加数量，而是：

1. 用清楚边界
2. 形成固定调用习惯
3. 遇到高频重复任务再新增专项 skill

当前最值得继续补的空缺方向是：

- Vue2 老项目维护专项
- uni-app 页面调试专项
- 权限/菜单/路由专项
- 上传/OSS/媒体流专项

---

## 9. 一句话速查

- 做业务：`$frontend-business-delivery`
- 做 CRUD：`$admin-crud-page-builder`
- 搬模块：`$cross-project-module-port`
- 做接口层：`$api-service-typing`
- 生成类型：`$generate-vue-types`
- 查 Vue bug：`$debug-vue-bugs`
- 做 Review：`$review-vue-pr`
- 做重构：`$refactor-vue-components`
- 看逻辑：`$analyze-code-logic`
- 查环境：`$multi-env-build-doctor`
