# Scripts 使用指南

项目提供了多个发布和管理脚本，位于 `./scripts/` 目录下。现已升级为**模块化架构**，提供更灵活、专业的发布体验。

## 📂 Scripts 目录结构

### 🆕 模块化架构 (v3.0)

```
scripts/
├── modules/                        # 独立功能模块
│   ├── check-uncommitted.sh       # 智能未提交代码处理
│   ├── version-bump.sh             # 智能版本提升选择
│   ├── create-tag.sh               # 创建tag和提交
│   └── monitor-release.sh          # 发布状态监控
├── smart-release-v3.sh             # 🌟 主控智能发布脚本
├── release.sh                      # 完整发布流程（经典版本）
├── quick-release.sh                # 快速发布流程（经典版本）
└── publish-local.sh                # 本地发布脚本（调试用）
```

## 🌟 模块化智能发布脚本（推荐）

### `smart-release-v3.sh` - 主控智能发布脚本

**🎯 完全按照专业发布流程设计**，实现四个独立步骤的完美组合：

#### 📋 发布流程
1. **检查工作目录状态** - 智能处理未提交代码
2. **版本管理** - 可选择版本升级或使用现有版本
3. **创建tag和提交** - 纯tag操作，触发GitHub Actions
4. **监控发布状态** - 5分钟实时监控，自动确认成功

#### 🚀 使用方法

```bash
# 完整智能发布流程
./scripts/smart-release-v3.sh

# 跳过版本升级，使用当前版本
./scripts/smart-release-v3.sh --skip-version

# 直接指定版本类型
./scripts/smart-release-v3.sh --version-type minor

# 不监控发布状态
./scripts/smart-release-v3.sh --no-monitor

# 查看帮助
./scripts/smart-release-v3.sh --help
```

#### ✨ 智能特性

- **智能未提交代码处理**: 3种处理选项（提交/暂存/取消）
- **智能版本推荐**: 基于git提交历史分析推荐合适版本
- **实时发布监控**: 监控GitHub Actions、NPM包、GitHub Release状态
- **完整链接输出**: 自动生成所有相关监控和访问链接
- **专业错误处理**: 每步都有错误检查和恢复提示

## 🔧 独立模块脚本

每个模块都可以独立运行，便于测试和调试：

### 1. `modules/check-uncommitted.sh` - 代码状态检查

```bash
# 检查工作目录状态
./scripts/modules/check-uncommitted.sh
```

**功能**:
- 检测未提交的更改
- 提供3种智能处理选项：提交、暂存、取消
- 给出清晰的操作指导

### 2. `modules/version-bump.sh` - 版本管理

```bash
# 交互式版本选择
./scripts/modules/version-bump.sh

# 直接指定版本类型
./scripts/modules/version-bump.sh patch
./scripts/modules/version-bump.sh minor
./scripts/modules/version-bump.sh major

# 测试模式（不执行实际升级）
./scripts/modules/version-bump.sh test
```

**功能**:
- 基于git提交历史的智能版本推荐
- 5种版本选择：patch、minor、major、prerelease、custom
- 版本预览和确认机制

### 3. `modules/create-tag.sh` - Tag创建

```bash
# 根据当前package.json版本创建tag
./scripts/modules/create-tag.sh
```

**功能**:
- 根据package.json版本号创建tag
- 不进行版本升级，纯tag操作
- 自动提交和推送，触发GitHub Actions

### 4. `modules/monitor-release.sh` - 发布监控

```bash
# 监控当前版本的发布状态
./scripts/modules/monitor-release.sh
```

**功能**:
- 5分钟超时的实时监控
- 并行检查GitHub Actions、NPM包、GitHub Release
- 输出完整的监控链接
- 生成发布状态总结报告

## 📊 经典发布脚本

### 1. `quick-release.sh` - 快速发布

**适用场景**: 日常开发中的小版本发布

```bash
# 交互式选择版本类型
./scripts/quick-release.sh

# 直接指定版本类型
./scripts/quick-release.sh patch   # 修订版本
./scripts/quick-release.sh minor   # 次版本
./scripts/quick-release.sh major   # 主版本
```

### 2. `release.sh` - 完整发布流程

**适用场景**: 重要版本发布，需要分支管理

```bash
# 启动完整发布流程
./scripts/release.sh
```

**特点**:
- 创建专用的release分支
- 智能版本建议和详细changelog
- 支持prerelease和自定义版本

## 🆚 版本对比

| 特性 | smart-release-v3.sh | 经典脚本 | 独立模块 |
|------|---------------------|----------|----------|
| **模块化设计** | ✅ | ❌ | ✅ |
| **发布监控** | ✅ 5分钟实时监控 | ❌ 仅提供链接 | ✅ |
| **智能代码处理** | ✅ 3种选项 | ❌ 直接拒绝 | ✅ |
| **参数支持** | ✅ 丰富的选项 | 🟡 基本支持 | ✅ |
| **错误恢复** | ✅ 专业级 | 🟡 基本 | ✅ |
| **可测试性** | ✅ 高 | 🟡 中等 | ✅ |

## 💡 使用建议

### 🎯 推荐使用场景

1. **日常发布**: 使用 `smart-release-v3.sh` - 最完整的智能体验
2. **快速测试**: 使用 `smart-release-v3.sh --no-monitor` - 跳过监控
3. **仅升级版本**: 使用 `modules/version-bump.sh` - 独立版本管理
4. **调试监控**: 使用 `modules/monitor-release.sh` - 独立测试监控功能
5. **传统发布**: 使用经典脚本作为备用方案

### 🚀 最佳实践

1. **首次使用**: 先运行 `smart-release-v3.sh --help` 了解选项
2. **测试环境**: 使用 `--no-monitor` 跳过监控，节省时间  
3. **版本规划**: 遵循[语义化版本](https://semver.org/lang/zh-CN/)规范
4. **提交规范**: 使用[约定式提交](https://www.conventionalcommits.org/zh-hans/)，获得智能版本推荐
5. **模块测试**: 单独测试各模块功能，确保环境正确

## 🔗 NPM Scripts 快捷方式

在package.json中的快捷方式：

```bash
# 智能发布（推荐）
pnpm run release:smart      # = ./scripts/smart-release-v3.sh

# 经典发布
pnpm run release            # = ./scripts/release.sh
pnpm run release:patch      # = ./scripts/quick-release.sh patch
pnpm run release:minor      # = ./scripts/quick-release.sh minor
pnpm run release:major      # = ./scripts/quick-release.sh major

# 本地发布
pnpm run publish:local      # = ./scripts/publish-local.sh
```

## ⚠️ 注意事项

### 1. 环境要求
- ✅ Node.js 和 pnpm 已安装
- ✅ Git 仓库，工作目录相对干净
- ✅ NPM_TOKEN 已在 GitHub Secrets 中配置
- ✅ curl 已安装（用于发布状态监控）

### 2. 权限设置
```bash
# 给脚本添加执行权限（首次使用）
chmod +x scripts/**/*.sh
```

### 3. 故障排查
- **模块未找到**: 检查 `scripts/modules/` 目录是否存在
- **权限错误**: 运行 `chmod +x scripts/**/*.sh`
- **监控超时**: 5分钟后超时是正常的，请手动检查链接
- **版本冲突**: 检查 package.json 版本号是否正确

---

🎉 **推荐**: 立即体验 `./scripts/smart-release-v3.sh` 获得最佳的智能发布体验！