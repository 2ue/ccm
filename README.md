# ccman

> Claude Code & Codex API 服务商配置管理工具

一个轻量级的命令行工具和桌面应用，用于管理 **Claude Code** 和 **Codex** 的 API 服务商配置，轻松切换不同的 API 提供商。

## ✨ 特性

- **🔧 统一管理**: 集中管理所有 API 服务商配置
- **⚡ 快速切换**: 一键切换不同的服务商
- **📦 零破坏性**: 只修改必要字段，保留用户自定义配置
- **🎯 预设模板**: 内置 Anthropic Official 和 PackyCode 预设
- **💻 双界面**: 同时提供命令行工具(CLI)和桌面应用(Desktop)
- **🔒 安全隔离**: 测试、开发、生产环境自动隔离

## 📦 安装

### 使用 CLI

```bash
# 使用 npm
npm install -g @ccman/cli

# 使用 pnpm
pnpm add -g @ccman/cli
```

### 使用 Desktop 应用

从 [Releases](https://github.com/2ue/ccm/releases) 下载适合您操作系统的安装包：

- macOS: `.dmg` 文件（Universal Binary，支持 Intel 和 Apple Silicon）
- Windows: `.exe` 文件

## 🚀 快速开始

### CLI 使用

ccman 提供 `cx`（Codex）和 `cc`（Claude Code）两个子命令来管理不同工具的服务商。

#### 交互式菜单

```bash
# 直接运行进入主菜单
ccman

# 或者进入 Codex 管理菜单
ccman cx

# 或者进入 Claude Code 管理菜单
ccman cc
```

#### Codex 命令行操作

```bash
# 添加服务商（交互式）
ccman cx add

# 列出所有服务商
ccman cx list

# 切换服务商（交互式）
ccman cx use

# 查看当前服务商
ccman cx current

# 编辑服务商（交互式）
ccman cx edit

# 删除服务商（交互式）
ccman cx remove

# 克隆服务商（交互式）
ccman cx clone
```

#### Claude Code 命令行操作

```bash
# 添加服务商（交互式）
ccman cc add

# 列出所有服务商
ccman cc list

# 切换服务商（交互式）
ccman cc use

# 查看当前服务商
ccman cc current

# 编辑服务商（交互式）
ccman cc edit

# 删除服务商（交互式）
ccman cc remove

# 克隆服务商（交互式）
ccman cc clone
```

### Desktop 应用使用

1. 启动应用
2. 点击"Add Provider"按钮
3. 选择预设模板或自定义配置
4. 保存后即可在列表中切换

## 📚 架构

ccman 采用 Monorepo 架构，包含 3 个模块：

```
ccman/
├── packages/
│   ├── core/       # 核心逻辑（配置读写、服务商 CRUD）
│   ├── cli/        # 命令行工具
│   └── desktop/    # Electron 桌面应用
```

### 核心模块 (@ccman/core)

- **零依赖哲学**: 只依赖 2 个必需库（`@iarna/toml`, `proper-lockfile`）
- **同步 I/O**: 使用同步文件操作，避免异步复杂性
- **环境隔离**: 通过 `NODE_ENV` 自动切换测试/开发/生产路径
- **单文件配置**: `~/.ccman/config.json` 存储所有配置

### CLI 模块 (@ccman/cli)

- **框架**: Commander.js
- **交互式 UI**: Inquirer.js
- **彩色输出**: Chalk
- **100% 代码复用**: 直接调用 @ccman/core，零重复代码

### Desktop 模块 (@ccman/desktop)

- **桌面框架**: Electron ^28.0
- **UI 框架**: React ^18.2
- **样式**: Tailwind CSS ^3.4
- **图标**: Lucide React
- **构建工具**: Vite ^5.0
- **100% 代码复用**: 主进程直接调用 @ccman/core

## 🛠️ 开发

### 前置要求

- Node.js >= 18.20.8
- pnpm >= 9.0.0

### 安装依赖

```bash
pnpm install
```

### 构建所有模块

```bash
pnpm build
```

### 运行测试

```bash
# 测试 Core 模块
cd packages/core && pnpm test

# 测试所有模块
pnpm -r test
```

### 开发 Desktop 应用

```bash
cd packages/desktop
pnpm dev
```

### 开发 CLI

```bash
cd packages/cli
pnpm dev
```

## 📝 设计原则

ccman 遵循以下核心设计原则：

1. **最小依赖**: 只引入绝对必要的依赖
2. **零破坏性写入**: 使用"读取-合并-写入"模式，保留用户其他字段
3. **同步 I/O**: 配置读写使用同步操作，避免异步复杂性
4. **单一职责**: 每个模块只做一件事并做好
5. **100% 代码复用**: CLI 和 Desktop 直接调用 Core，零重复代码

详见 `docs/` 目录下的设计文档。

## 🔒 安全性

- **文件权限**: 配置文件权限设为 `0o600`（仅所有者可读写）
- **Electron 安全**: 启用 `contextIsolation`，禁用 `nodeIntegration`
- **API Key 保护**: 敏感信息只存储在本地配置文件

## 🧪 测试覆盖

- Core 模块: 11/11 测试通过 ✅
  - writers/codex.test.ts (5 tests)
  - writers/claudecode.test.ts (6 tests)

## 🚀 发布与部署

本项目使用 GitHub Actions 自动发布：

- **npm 包发布**: 推送 `v*` 格式的 tag 自动发布到 npm
- **Desktop 应用打包**: 自动构建 macOS 和 Windows 安装包

详见 [GitHub Actions 使用指南](./docs/GitHub_Actions.md)

### 快速发布

```bash
# 更新版本号
npm version 0.2.0

# 创建并推送 tag
git tag v0.2.0
git push origin v0.2.0

# GitHub Actions 会自动：
# ✅ 发布 npm 包
# ✅ 构建 Desktop 应用
# ✅ 创建 GitHub Release
```

## 📄 许可证

MIT

## 🙏 致谢

- [Anthropic Claude](https://www.anthropic.com/) - Claude Code 官方 API
- [PackyCode](https://www.packycode.com/) - Codex API 服务提供商

## 🐛 问题反馈

如果您遇到问题或有功能建议，请在 [GitHub Issues](https://github.com/2ue/ccm/issues) 提交。
