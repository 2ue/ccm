# 版本管理

## 🎯 版本工具

项目使用独立的版本工具模块 `src/utils/version.ts`：

```typescript
import { getCurrentVersion } from './utils/version';

// CLI 自动获取版本
program.version(getCurrentVersion());
```

## 📦 发布脚本功能

### 智能版本推荐
脚本会分析最近的 git 提交，自动推荐合适的版本类型：
- `feat`, `feature` → minor 版本
- `breaking`, `major` → major 版本  
- 其他 → patch 版本

### 交互式界面
```bash
📦 版本升级选项:

✨ [推荐] 1) 🔧 patch (修订版本)    0.0.1 → 0.0.2
          2) ✨ minor (次版本)     0.0.1 → 0.1.0  
          3) 🚀 major (主版本)     0.0.1 → 1.0.0

请选择版本类型 (1-3, 回车默认选择推荐项):
```

## 🚀 使用方式

### 快速发布
```bash
pnpm run release:patch      # 直接发布 patch 版本
pnpm run release:minor      # 直接发布 minor 版本
pnpm run release:major      # 直接发布 major 版本
```

### 交互式发布
```bash
pnpm run release:interactive  # 交互选择版本类型
pnpm run release             # 完整发布流程
```

### 本地发布
```bash
pnpm run publish:local      # 绕过 GitHub Actions
```

## 🎯 版本管理最佳实践

### 语义化版本
- **patch (0.0.x)**: Bug 修复、小改进
- **minor (0.x.0)**: 新功能、向后兼容
- **major (x.0.0)**: 破坏性更改、重大重构

### 工具特性
- ✅ 版本号统一管理（package.json）
- ✅ 智能推荐基于 git 提交分析
- ✅ 交互式界面友好
- ✅ 支持多种发布模式