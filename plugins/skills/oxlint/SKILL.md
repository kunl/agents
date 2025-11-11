---
name: oxlint
description: Check and fix JavaScript/TypeScript/Vue code using oxlint. Use when checking code quality, fixing lint errors, or before commits. Supports .js/.mjs/.cjs/.jsx/.ts/.mts/.cts/.tsx/.vue files. Works with or without git.
allowed-tools: Bash
---

# Oxlint Code Checker

使用 oxlint 检查和修复 JavaScript/TypeScript/Vue 代码质量问题。

## 快速使用

**检查代码**：
- `"检查代码"` - 检查 git 变更的文件（如果有 git）
- `"检查 src 目录"` - 检查指定目录
- `"检查 main.js"` - 检查指定文件
- `"检查 packages/common 和 packages/marketing"` - 检查多个目录

**修复代码**：
- `"修复 lint 错误"` - 自动修复 git 变更的文件
- `"修复 src 目录的问题"` - 修复指定目录
- `"修复 main.js"` - 修复指定文件

**支持任意路径**：可以指定项目中的任何文件或目录！

## 支持的文件类型

- `.js` - JavaScript
- `.mjs` - ES Module JavaScript
- `.cjs` - CommonJS JavaScript
- `.jsx` - React JSX
- `.ts` - TypeScript
- `.mts` - ES Module TypeScript
- `.cts` - CommonJS TypeScript
- `.tsx` - React TypeScript
- `.vue` - Vue Single File Component

## 工作流程

### 1. 确定要检查的文件

按以下优先级确定检查范围：

**优先级 1：用户明确指定的路径**
- 如果用户说"检查 src 目录"，使用 `src/`
- 如果用户说"检查 main.js"，使用 `main.js`
- 用户指定的路径优先级最高

**优先级 2：Git 变更的文件**（如果在 git 仓库中）
- 首先检查是否是 git 仓库：
  ```bash
  git rev-parse --git-dir 2>/dev/null
  ```
- 如果是 git 仓库，获取变更的文件：
  ```bash
  git diff HEAD --name-only | grep -E '\.(js|mjs|cjs|jsx|ts|mts|cts|tsx|vue)$'
  ```
- 只检查变更过的文件（包括 staged 和 unstaged）

**优先级 3：询问用户**
- 如果不在 git 仓库且用户没有指定路径
- 询问用户："要检查哪些文件或目录？"
- 建议选项：
  - `.` (当前目录所有文件)
  - `src/` (源代码目录)
  - 特定文件或目录路径

### 2. 执行检查或修复

根据用户的意图选择模式：

**检查模式**（当用户说"检查"/"check"/"lint"/"运行 lint"时）：
```bash
oxlint --quiet [files or directories]
```

**修复模式**（当用户说"修复"/"fix"/"自动修复"/"fix lint"时）：
```bash
oxlint --fix --quiet [files or directories]
```

**重要**：
- 使用 `--quiet` 减少不必要的输出
- 如果有多个文件，将它们作为参数传递给 oxlint
- 如果是目录，直接传递目录路径

### 3. 报告结果

清晰地向用户报告结果：
- 如果没有发现问题：告知用户"没有发现 lint 问题"
- 如果发现问题：显示问题详情
- 如果进行了修复：告知用户哪些文件被修复了

## 使用场景示例

### 场景 1：有 git，检查变更文件
```
用户："检查代码"
→ 检查 git diff 中的变更文件
→ 运行：oxlint --quiet file1.js file2.ts
```

### 场景 2：检查指定目录
```
用户："检查 src 目录"
→ 运行：oxlint --quiet src/

用户："检查 packages/marketing 目录"
→ 运行：oxlint --quiet packages/marketing/

用户："检查整个项目"
→ 运行：oxlint --quiet .
```

### 场景 3：检查指定文件
```
用户："检查 packages/common/src/index.ts"
→ 运行：oxlint --quiet packages/common/src/index.ts

用户："检查 main.js 和 utils.js"
→ 运行：oxlint --quiet main.js utils.js
```

### 场景 4：修复指定目录
```
用户："修复 src 目录的 lint 错误"
→ 运行：oxlint --fix --quiet src/

用户："修复 packages/common 的代码"
→ 运行：oxlint --fix --quiet packages/common/
```

### 场景 5：修复指定文件
```
用户："修复 packages/marketing/src/views/qiwei/component/bo-note.vue"
→ 运行：oxlint --fix --quiet packages/marketing/src/views/qiwei/component/bo-note.vue

用户："自动修复 main.ts 的问题"
→ 运行：oxlint --fix --quiet main.ts
```

### 场景 6：修复 git 变更文件
```
用户："修复 lint 错误"
→ 检查变更文件并自动修复
→ 运行：oxlint --fix --quiet file1.js file2.ts
```

### 场景 7：检查多个目录
```
用户："检查 packages/common 和 packages/marketing 目录"
→ 运行：oxlint --quiet packages/common/ packages/marketing/
```

### 场景 8：没有 git，没有指定路径
```
用户："检查代码"
→ 询问："要检查哪些文件或目录？（例如：src/, . 或特定文件）"
→ 用户回复："src/"
→ 运行：oxlint --quiet src/
```

## 注意事项

1. **项目配置**：
   - 项目已配置 oxlint (v1.25.0)
   - package.json 中已有 lint-staged 配置

2. **文件过滤**：
   - 只检查支持的文件类型
   - 自动过滤掉不支持的文件

3. **错误处理**：
   - 如果 oxlint 未安装，提示用户安装
   - 如果没有找到可检查的文件，告知用户

4. **输出格式**：
   - 使用 `--quiet` 减少不必要的输出
   - 只显示实际的问题和错误

## 常见命令参考

```bash
# 检查变更的文件（有 git）
git diff HEAD --name-only | grep -E '\.(js|mjs|cjs|jsx|ts|mts|cts|tsx|vue)$' | xargs oxlint --quiet

# 检查整个 src 目录
oxlint --quiet src/

# 检查并修复整个项目
oxlint --fix --quiet .

# 检查特定文件
oxlint --quiet path/to/file.ts
```
