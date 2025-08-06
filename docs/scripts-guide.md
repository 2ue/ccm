# Scripts 使用指南

项目提供了多个发布和管理脚本，位于 `./scripts/` 目录下。这些脚本提供了比npm scripts更强大和灵活的功能。

## 📂 Scripts 目录结构

```
scripts/
├── release.sh          # 完整发布流程（推荐用于重要版本）
├── quick-release.sh     # 快速发布流程（日常开发推荐）
└── publish-local.sh     # 本地发布脚本（调试用）
```

## 🚀 发布脚本详解

### 1. `quick-release.sh` - 快速发布（推荐）

**适用场景**: 日常开发中的小版本发布

**特点**:
- 简单高效，一键发布
- 自动构建、测试、版本管理
- 支持patch/minor/major版本升级
- 直接在主分支操作

**使用方法**:
```bash
# 交互式选择版本类型
./scripts/quick-release.sh

# 直接指定版本类型  
./scripts/quick-release.sh patch   # 修订版本 0.0.3 → 0.0.4
./scripts/quick-release.sh minor   # 次版本   0.0.3 → 0.1.0
./scripts/quick-release.sh major   # 主版本   0.0.3 → 1.0.0
```

**执行流程**:
1. ✅ 检查工作目录状态
2. 🔧 运行构建和代码检查
3. 📝 更新版本号
4. 📤 创建提交和标签
5. 🚀 推送到远程仓库，触发GitHub Actions

### 2. `release.sh` - 完整发布流程

**适用场景**: 重要版本发布，需要详细管理和记录

**特点**:
- 创建专用的release分支
- 智能版本建议（基于git提交分析）
- 生成详细的changelog
- 支持自定义版本号和预发布版本
- 更严格的检查和确认流程

**使用方法**:
```bash
# 启动完整发布流程
./scripts/release.sh
```

**版本选择**:
- **patch (修订版本)** - bug修复、小改进
- **minor (次版本)** - 新功能、向后兼容改动
- **major (主版本)** - 破坏性更改、重大重构  
- **prerelease (预发布)** - 测试版本、beta版本
- **custom (自定义)** - 手动指定版本号

**智能版本建议**:
脚本会分析最近的git提交，智能推荐合适的版本类型：
- 检测到 "breaking"/"major" → 推荐 major
- 检测到 "feat"/"feature"/"add" → 推荐 minor  
- 其他情况 → 推荐 patch

### 3. `publish-local.sh` - 本地发布

**适用场景**: 调试发布流程，或GitHub Actions不可用时

**使用方法**:
```bash
./scripts/publish-local.sh
```

## 🆚 Scripts vs NPM Scripts 对比

| 特性 | Scripts目录 | NPM Scripts |
|------|-------------|-------------|
| **功能** | 完整发布流程 | 单一命令 |
| **交互性** | 丰富的交互提示 | 基本执行 |
| **错误处理** | 详细检查和提示 | 基本错误 |
| **版本管理** | 智能推荐和验证 | 需手动指定 |
| **分支管理** | 支持release分支 | 仅主分支 |
| **日志记录** | 自动生成changelog | 无 |

## 🔧 NPM Scripts 快捷方式

在package.json中已配置的快捷方式：

```bash
# 等价于 ./scripts/release.sh
pnpm run release

# 等价于 ./scripts/quick-release.sh
pnpm run release:interactive  

# 等价于 ./scripts/quick-release.sh patch/minor/major
pnpm run release:patch
pnpm run release:minor  
pnpm run release:major

# 等价于 ./scripts/publish-local.sh
pnpm run publish:local
```

## 📋 使用建议

### 日常开发推荐流程:

```bash
# 1. 日常修复和小改进
./scripts/quick-release.sh patch

# 2. 新功能发布
./scripts/quick-release.sh minor

# 3. 重大版本发布
./scripts/release.sh
```

### 首次使用:

```bash
# 1. 给脚本添加执行权限
chmod +x scripts/*.sh

# 2. 测试脚本（会检查环境但不执行发布）
./scripts/quick-release.sh --help || echo "脚本正常"

# 3. 执行第一次发布
./scripts/quick-release.sh patch
```

## ⚠️ 注意事项

### 1. 前置条件
- ✅ 工作目录必须干净（无未提交更改）
- ✅ 必须在git仓库中
- ✅ NPM_TOKEN已在GitHub Secrets中设置
- ✅ 必须安装：git, node, pnpm

### 2. 脚本特性
- **自动构建**: 发布前自动运行`pnpm run build`和`pnpm run lint`
- **版本验证**: 防止重复版本号
- **自动推送**: 创建tag后自动推送触发GitHub Actions
- **错误保护**: 遇到错误会立即停止

### 3. 故障排查

```bash
# 如果脚本执行失败，检查：
1. git status                    # 确保工作目录干净
2. pnpm run build               # 确保构建成功  
3. pnpm run lint                # 确保代码检查通过
4. git log --oneline -5         # 检查最近提交记录
```

## 🎯 最佳实践

1. **日常使用**: 优先使用`quick-release.sh`，简单高效
2. **重要版本**: 使用`release.sh`，完整流程和记录
3. **版本规划**: 遵循[语义化版本](https://semver.org/lang/zh-CN/)规范
4. **提交规范**: 使用[约定式提交](https://www.conventionalcommits.org/zh-hans/)格式
5. **发布验证**: 发布后及时检查GitHub Actions和NPM包

---

💡 **提示**: 推荐将`./scripts/quick-release.sh`加入你的开发工作流，一键完成版本发布！