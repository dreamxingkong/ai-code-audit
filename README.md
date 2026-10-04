# AI Code Audit

A VS Code extension that runs **multi-model AI code security audits**: calls OpenAI / Claude / GLM and custom providers in parallel, scores overall risk, and highlights vulnerabilities directly in the editor.

> ⚠️ This repository only distributes the compiled `.vsix` package. Source code is not published.

## 功能特性

- **多模型协同**：并行调用多个 AI 模型独立审查，单个模型失败不影响整体
- **自定义供应商**：在配置界面里添加任意 OpenAI / Claude 兼容的 API 端点（如 DeepSeek、Kimi、通义千问等）
- **综合风险评分**：跨模型聚类合并，交叉确认标记，按 high / medium / low 扣分，0–100 分
- **编辑器高亮**：漏洞行三色半透明背景（红/橙/黄），鼠标悬停显示详情
- **现代化配置界面**：卡片式 Webview，自动适配 VS Code 亮/暗主题
- **密钥安全**：API Key 仅存于操作系统凭据管理器（Windows Credential Manager / macOS Keychain），绝不写入文件或日志

---

## 安装（三步）

### 第一步：下载 `.vsix` 文件

1. 打开本仓库的 **Releases** 页面。
2. 找到最新版本（例如 `v0.1.0`）。
3. 在 **Assets** 区域点击 `ai-code-audit-0.1.0.vsix` 下载到本地。

> 如果你看不到 Assets，点一下 Release 标题下方那个折叠箭头就能展开。

### 第二步：在 VS Code 里安装

1. 打开 VS Code。
2. 按 `Ctrl + Shift + X` 打开左侧**扩展面板**。
3. 点扩展面板**右上角的 `…`**（三个点）。
4. 在下拉菜单里选 **“从 VSIX 安装…”**。
5. 在弹出的文件选择框里，找到你刚下载的 `ai-code-audit-0.1.0.vsix`，选中，点“打开”。
6. 右下角会提示“已完成安装”，点 **“重新加载”**。

### 第三步：验证安装成功

1. 按 `Ctrl + Shift + P` 打开命令面板。
2. 输入 `AI Code Audit`。
3. 如果能看到下面 5 条命令，说明安装成功：
   - `AI Code Audit: AI 协同安全审查当前文件`
   - `AI Code Audit: 打开配置界面`
   - `AI Code Audit: 设置模型 API Key`
   - `AI Code Audit: 清除模型 API Key`
   - `AI Code Audit: 查看上次审查报告`

> 如果命令面板里搜不到，按 `Ctrl + Shift + P` → 输入 `Developer: Reload Window` → 回车，重载窗口后再搜一次。

---

## 使用方法（五步）

### 第一步：录入 API Key

1. 按 `Ctrl + Shift + P`，输入 `AI Code Audit: 打开配置界面`，回车。
2. 配置界面里有三张**内置供应商卡片**：OpenAI GPT、Anthropic Claude、智谱 GLM。
3. 在你要用的供应商卡片里，把 **API Key** 粘贴进输入框，点卡片里的 **“保存”**。
4. 状态点会从红色变成绿色，文字从“未配置”变成“已配置”。
5. （可选）如果要接入 DeepSeek、Kimi 等**自定义供应商**：
   - 在“自定义供应商”区域点 **“＋ 添加供应商”**。
   - 依次填：`id`、`显示名`、`API 地址`、`模型名`、`接口格式`。
   - 保存后，新卡片会出现在列表里，再按第 3 步填 Key。
6. 关掉配置面板，配置自动保存。

### 第二步：勾选参与审查的模型

1. 配置界面顶部有 **“参与审查的模型”** 区域。
2. 每个供应商名旁边有一个开关，**打开你想用的模型**，关闭不想用的。
3. **第一次测试建议只勾一个模型**（比如智谱 GLM），确认流程没问题后，再勾多个做并行审查。

### 第三步：发起审查

1. 在 VS Code 里打开一个代码文件（`.js`、`.ts`、`.py`、`.java` 都行）。
2. 在编辑器里**右键**。
3. 选 **“AI 协同安全审查当前文件”**。
4. 右下角会出现进度条“AI 协同安全审查中…”，等它跑完。

### 第四步：查看结果

审查完成后，会同时出现三处反馈：

1. **右侧报告面板**：自动弹出，显示：
   - 顶部环形评分图（0–100 分）
   - “综合问题”列表（跨模型聚类合并）
   - “各模型明细”（每个模型单独报出的漏洞）

2. **编辑器内高亮**：有漏洞的行会被加上半透明背景色：
   - 红色 = 高危
   - 橙色 = 中危
   - 黄色 = 低危
   - 鼠标悬停在有颜色的行上，会显示“【模型名】漏洞类型：修复建议”。

3. **问题面板**：按 `Ctrl + Shift + M` 打开，能看到全部诊断列表，每条带行号和严重级别。

### 第五步（可选）：再次审查

- **想重新审查同一个文件**：再右键一次，新的结果会**覆盖**旧的高亮和报告，不会堆叠。
- **关闭文件**：诊断和高亮会自动清除。
- **想查看上次报告**：按 `Ctrl + Shift + P` → `AI Code Audit: 查看上次审查报告`。

---

## 支持的模型

| 类型 | 供应商 | 说明 |
|---|---|---|
| 内置 | OpenAI GPT | 默认 `gpt-4o-mini`，可在配置界面修改模型名 |
| 内置 | Anthropic Claude | 默认 `claude-3-5-sonnet-latest` |
| 内置 | 智谱 GLM | 默认 `glm-4-flash` |
| 自定义 | 任意 OpenAI 兼容端点 | 如 DeepSeek、Kimi、通义千问 |
| 自定义 | 任意 Claude 兼容端点 | 走 `/v1/messages` 协议 |

---

## 密钥安全

- API Key 只通过 VS Code 官方的 `SecretStorage` API 读写，由操作系统加密存储
- 录入使用密码模式输入框；配置界面只显示「已配置 / 未配置」状态点，**密钥永不回显**
- 密钥不写入任何文件、不进日志、不进报告
- 网络请求仅走 HTTPS，无任何第三方运行时依赖、无遥测

---

## 常见问题

- **提示鉴权失败（HTTP 401/403）**：API Key 不对或已过期，到配置界面重新保存
- **请求超时**：在配置界面把「超时(秒)」调大
- **想用代理或中转**：直接修改对应供应商的 `Base URL`
- **看不到高亮**：确认审查完成弹出过报告面板；重新触发一次审查即可刷新

---

## 许可证

MIT License. 本仓库仅分发编译后的安装包，不提供源码。
