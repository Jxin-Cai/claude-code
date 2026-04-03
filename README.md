# Claude Code 源码还原

> 从 `@anthropic-ai/claude-code` npm 包的 source map 中还原的完整 TypeScript 源码，**可本地运行**

> [!WARNING]
> 本仓库为**非官方**版本，基于公开 npm 发布包 source map 还原，**仅供研究学习**。源码版权归 [Anthropic](https://www.anthropic.com) 所有。

---

## 从零开始

### 1. 安装前置工具

#### Node.js >= 24

```bash
# macOS (Homebrew)
brew install node@24

# 或使用 nvm
nvm install 24
nvm use 24

# 验证
node -v   # 应显示 v24.x.x
```

#### Bun >= 1.3.5

```bash
# macOS / Linux
curl -fsSL https://bun.sh/install | bash

# 验证
bun -v   # 应显示 >= 1.3.5
```

### 2. 克隆仓库

```bash
git clone https://gitee.com/SingleXin/jxin-code.git
cd jxin-code
```

### 3. 安装依赖

```bash
bun install
```

### 4. 配置环境变量

至少设置一个认证方式（二选一）：

```bash
# 方式一：直接使用 API Key
export ANTHROPIC_API_KEY="sk-ant-xxxxx"

# 方式二：OAuth 登录（启动后执行 /login）
```

### 5. 构建

```bash
bun run build.ts
```

构建产物为 `cli.js`（单文件 ESM），所有 `feature()` 编译开关已设为 `true`（解锁全部隐藏功能）。

### 6. 全局安装

#### 方式一：npm 全局安装（推荐）

```bash
npm install -g .
```

npm 会自动将 `package.json` 中 `bin.claude` 指向的 `cli.js` 链接到全局 PATH，一步到位。

#### 方式二：手动软链接

```bash
chmod +x cli.js
sudo ln -sf "$(pwd)/cli.js" /usr/local/bin/claude
```

#### 验证

```bash
claude --version
```

现在在任意目录下执行 `claude` 即可启动。

---

## 环境变量参考

### 认证

| 环境变量 | 说明 |
|----------|------|
| `ANTHROPIC_API_KEY` | Anthropic API 密钥（最简方式） |
| `CLAUDE_CODE_OAUTH_TOKEN` | OAuth 令牌（通过 `/login` 获取） |

### 云服务提供商

默认直连 Anthropic API。设置以下变量可切换提供商：

| 环境变量 | 说明 |
|----------|------|
| `CLAUDE_CODE_USE_BEDROCK=1` | 切换到 AWS Bedrock |
| `CLAUDE_CODE_USE_VERTEX=1` | 切换到 Google Vertex AI |
| `CLAUDE_CODE_USE_FOUNDRY=1` | 切换到 Azure Foundry |

#### AWS Bedrock

| 环境变量 | 说明 |
|----------|------|
| `AWS_REGION` | AWS 区域（默认 `us-east-1`） |
| `AWS_PROFILE` | AWS 配置文件名称 |
| `CLAUDE_CODE_SKIP_BEDROCK_AUTH` | 跳过凭证检查 |

#### Google Vertex AI

| 环境变量 | 说明 |
|----------|------|
| `ANTHROPIC_VERTEX_PROJECT_ID` | **必填**，GCP 项目 ID |
| `CLOUD_ML_REGION` | GCP 区域（默认 `us-east5`） |
| `CLAUDE_CODE_SKIP_VERTEX_AUTH` | 跳过凭证检查 |

#### Azure Foundry

| 环境变量 | 说明 |
|----------|------|
| `ANTHROPIC_FOUNDRY_RESOURCE` | Azure 资源名称 |
| `ANTHROPIC_FOUNDRY_BASE_URL` | 或直接提供完整 Base URL |
| `ANTHROPIC_FOUNDRY_API_KEY` | Foundry API 密钥（不设则用 Azure AD） |

### 模型配置

| 环境变量 | 说明 |
|----------|------|
| `ANTHROPIC_MODEL` | 覆盖主对话模型 |
| `ANTHROPIC_SMALL_FAST_MODEL` | 覆盖轻量任务模型（默认 Haiku） |
| `CLAUDE_CODE_SUBAGENT_MODEL` | 子 Agent 使用的模型 |
| `ANTHROPIC_BASE_URL` | 覆盖 API Base URL（用于代理/自定义端点） |

### 运行行为

| 环境变量 | 说明 |
|----------|------|
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS` | 单次回复最大 token 数 |
| `MAX_THINKING_TOKENS` | 思考 token 上限 |
| `BASH_DEFAULT_TIMEOUT_MS` | Bash 命令默认超时（毫秒） |
| `HTTPS_PROXY` / `HTTP_PROXY` | 网络代理 |
| `DISABLE_AUTOUPDATER` | 禁用自动更新 |
| `DISABLE_TELEMETRY` | 禁用遥测 |
| `CLAUDE_CONFIG_DIR` | 覆盖配置目录（默认 `~/.claude`） |
