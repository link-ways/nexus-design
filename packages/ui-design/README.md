# @linkways/nexus-design

Nexus 社交与商业平台的企业级 UI 组件库。基于 **Vue 3**、**Element Plus** 和 **UnoCSS** 构建。

## 🚀 特性

- 💎 **现代设计**：专为企业级应用打造的简洁、专业的 UI 界面。
- 🛠 **强类型支持**：所有组件和 Props 均提供完善的 TypeScript 支持。
- 📦 **支持 Tree Shaking**：按需引入，优化打包体积。
- 🎨 **高度可定制**：通过 CSS 变量和 UnoCSS 轻松定制主题。

## 📦 安装

```bash
pnpm add @linkways/nexus-design
```

请确保你已经安装了以下对等依赖（Peer Dependencies）：

```bash
pnpm add vue element-plus @element-plus/icons-vue
```

## 🧩 核心组件

| 组件名 | 描述 |
|-----------|-------------|
| `StatsCards` | 仪表盘统计摘要卡片。 |
| `SystemStatus` | 实时系统状态与健康检查指标。 |
| `InsightCards` | AI 驱动的洞察与高亮卡片。 |
| `NxContactList` | 功能完备的联系人与部门管理列表。 |
| `NxWikiList` | 文档管理与知识库列表。 |
| `NxMeetingList` | 会议预定与会议室管理。 |
| `NxWorkflowList` | 审批工作流与任务追踪。 |

## 💻 使用示例

### 基础示例：NxContactList

```vue
<script setup lang="ts">
import { NxContactList } from '@linkways/nexus-design';
import { User, OfficeBuilding } from '@element-plus/icons-vue';

const depts = [
  { name: "工程部", count: 24, icon: OfficeBuilding },
  { name: "设计部", count: 12, icon: User }
];

const contacts = [
  {
    id: "1",
    name: "张三",
    role: "首席工程师",
    dept: "工程部",
    email: "zhangsan@example.com",
    avatar: "ZS"
  }
];

const handleSelectContact = (contact) => {
  console.log('已选择联系人:', contact);
};
</script>

<template>
  <NxContactList 
    :contacts="contacts"
    :depts="depts"
    @select-contact="handleSelectContact"
  />
</template>
```

## 🛠 开发指南

### 环境准备

```bash
pnpm install
```

### 编译打包

```bash
pnpm build
```

### 发布到 npm

由于这是一个作用域包（Scoped Package），首次发布时需要指定公开访问权限：

```bash
npm publish --access public
```

或者使用 `pnpm`（已在 `package.json` 中配置 `publishConfig`）：

```bash
pnpm publish
```

## 📄 开源协议

MIT © Linkways
