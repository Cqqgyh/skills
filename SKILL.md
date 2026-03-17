---
name: yh-icon-auto
description: Automates adding new SVG icons to yh-icon Vue component library. Invoke when user wants to add new icons or mentions SVG to Vue icon conversion.
---

# YH-Icon Auto - SVG 图标自动添加工具

自动化将 SVG 文件转换为 Vue 组件并添加到 yh-icon 图标库的流程。

## 触发条件

当用户需要：

- 添加新的 SVG 图标到项目
- 将 SVG 转换为 Vue 组件
- 批量添加多个图标
- 提到"添加图标"、"新增图标"、"svg转vue"等关键词

## 执行步骤

### 1. 将 SVG 文件重命名为 Vue 组件，直接修改原文件，不要新生成一个

将在 `packages/components/Svgs/components/` 目录下的.svg文件直接重命名为.vue组件（直接修改原文件，不要新生成一个），命名格式为 `YH{IconName}.vue`，其中IconName如果为中文则需要转化为英文，遇到重复文件则在后面添加序号。必须使用终端命令进行重命名，例如：

```bash
Rename-Item -Path "d:\dcode\yh-icon\packages\components\Svgs\components\xxx.svg" -NewName "YH{IconName}.vue" -Force
```

**模板结构：**

```vue
<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    fill="none"
    version="1.1"
    width="100"
    height="100"
    viewBox="0 0 100 100"
  >
    <defs>
      <clipPath id="master_svg0_{unique_id}">
        <rect x="0" y="0" width="100" height="100" rx="0" />
      </clipPath>
    </defs>
    <g clip-path="url(#master_svg0_{unique_id})">
      <!-- SVG paths here -->
      <path
        d="..."
        fill="currentColor"
        fill-opacity="1"
        style="mix-blend-mode: passthrough"
      />
    </g>
  </svg>
</template>
```

**注意事项：**

- 文件名使用 PascalCase，以 `YH` 开头
- `fill` 属性使用 `currentColor` 以支持颜色自定义
- `clipPath` 的 id 需要唯一，格式为 `master_svg0_{随机数}`

### 2. 导出组件

在 `packages/components/Svgs/components/index.ts` 文件末尾添加导出语句：

```typescript
export { default as YH{IconName} } from "./YH{IconName}.vue";
```

**示例：**

```typescript
export { default as YHIgnore } from "./YHIgnore.vue";
```

### 3. 添加类型定义

在 `packages/components/Svgs/components/type.ts` 的 `YhIconTypes` 类型中添加新图标名称：

```typescript
export type YhIconTypes =
  | string
  | "ExistingIcon"
  // ... 其他图标
  | "YH{IconName}"; // 添加在最后
```

**示例：**

```typescript
  | "YHReturn"
  | "YHIgnore";  // 新增
```

### 4. 添加使用示例（可选）

在 `src/pages/Icon/icon.vue` 中添加图标展示：

```vue
<YhIcon name="YH{IconName}" color="red" />
```

**示例：**

```vue
<YhIcon name="YHIgnore" color="blue" />
```

### 5. 更新版本号

在 `package.json` 中更新版本号，遵循语义化版本：

```json
{
  "version": "0.3.1974" // 递增补丁版本
}
```

## 完整示例

假设要添加名为 `YHNewIcon` 的新图标：

1. **将svg文件重命名为vue文件，直接修改原文件，不要新生成一个** `packages/components/Svgs/components/YHNewIcon.vue`
2. **添加导出** 在 `index.ts` 末尾：`export { default as YHNewIcon } from "./YHNewIcon.vue";`
3. **添加类型** 在 `type.ts` 末尾：`| "YHNewIcon";`
4. **添加示例** 在 `icon.vue`：`<YhIcon name="YHNewIcon" color="green" />`，颜色属性与上次git提交最好不相同red，green，blue轮流使用
5. **更新版本** `package.json` 版本号 +1，如果版本号可以整除10，则新增一位，例如0.3.1970 -> 0.3.1971

## 批量添加

当需要批量添加多个图标时，按以下顺序执行：

1. 将所有 `.svg` 文件重命名为 `.vue` 组件文件，直接修改原文件，不要新生成一个
2. 在 `index.ts` 中批量添加导出语句
3. 在 `type.ts` 中批量添加类型定义
4. 在 `icon.vue` 中批量添加示例
5. 更新一次版本号，如果版本号可以整除10，则新增一位，例如0.3.1970 -> 0.3.1971

## 文件路径参考

| 文件     | 路径                                                   |
| -------- | ------------------------------------------------------ |
| SVG 组件 | `packages/components/Svgs/components/YH{IconName}.vue` |
| 导出索引 | `packages/components/Svgs/components/index.ts`         |
| 类型定义 | `packages/components/Svgs/components/type.ts`          |
| 使用示例 | `src/pages/Icon/icon.vue`                              |
| 版本配置 | `package.json`                                         |
