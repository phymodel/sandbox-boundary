# Sandbox Boundary

定义 Agent 在 ChatStudio 工作区中的**安全边界规则**，防止 Agent 误修改构建产物或打包文件。

## 为什么需要沙箱边界

ChatStudio 项目包含多个看似相同的目录，但只有部分目录是真正的源码：

| 目录 | 类型 | 说明 |
|------|------|------|
| `web_editor/backend/` | ✅ 源码 | 后端代码的唯一修改入口 |
| `web_editor/frontend/src/` | ✅ 源码 | 前端代码的唯一修改入口 |
| `web_editor/frontend/build-resources/backend/` | ❌ 构建暂存区 | 由 `pack-electron.sh` 从源码自动生成，**不可直接修改** |
| `/Applications/ChatStudio.app/` | ❌ 打包产物 | macOS 安装的应用包，**不可修改** |

如果在构建暂存区或打包产物中直接修改代码，变更会在下次打包时被**静默覆盖**，甚至导致运行中的应用崩溃。

## 规则文件

- `RULE.md` — 沙箱边界规则（必读），ChatStudio Agent 在每次对话中自动加载

## 正确的工作流

```bash
# 1. 修改源码
#    → 后端：编辑 web_editor/backend/
#    → 前端：编辑 web_editor/frontend/src/

# 2. 构建前端
cd web_editor/frontend && npm run build

# 3. 打包
./run_pack_electron.command

# 4. 测试
open web_editor/frontend/release/*.app
```
