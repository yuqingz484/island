# DeepSeek API 配置全流程记录（Mac）

## 参考资料

DeepSeek API 接入 Claude Code 官方文档：
https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/claude_code

---

## 一、获取 DeepSeek API Key

1. 打开 [DeepSeek Platform](https://platform.deepseek.com) 注册账号
2. 进入 API Keys 页面，点击"创建 API Key"，复制保存（格式为 `sk-xxx...`）
3. 充值（最低 $5 即可），否则 API 调用会因余额不足被拒绝

---

## 二、安装 Node.js（Mac）

Claude Code 依赖 Node.js 18+。Mac 推荐用 Homebrew 安装：

```bash
# 如果没有 Homebrew，先安装 Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 安装 Node.js
brew install node

# 验证版本（需 >= 18）
node --version
```

---

## 三、安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code

# 验证安装
claude --version
```

---

## 四、配置环境变量

### 4.1 临时配置（当前终端有效）

```bash
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=<你的 DeepSeek API Key>
export ANTHROPIC_MODEL=deepseek-v4-pro
export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro
export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro
export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash
export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-flash
export CLAUDE_CODE_EFFORT_LEVEL=max
```

### 4.2 持久化配置（重启终端后仍然有效）

将以上环境变量写入 `~/.zshrc`（Mac 默认 shell 为 zsh）：

```bash
cat >> ~/.zshrc << 'EOF'

# DeepSeek API + Claude Code 配置
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=<你的 DeepSeek API Key>
export ANTHROPIC_MODEL=deepseek-v4-pro
export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro
export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro
export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash
export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-flash
export CLAUDE_CODE_EFFORT_LEVEL=max
EOF

# 使配置立即生效
source ~/.zshrc
```

---

## 五、环境变量说明

| 变量名 | 值 | 说明 |
|--------|-----|------|
| `ANTHROPIC_BASE_URL` | `https://api.deepseek.com/anthropic` | DeepSeek Anthropic 兼容端点 |
| `ANTHROPIC_AUTH_TOKEN` | `sk-xxx...` | DeepSeek API Key |
| `ANTHROPIC_MODEL` | `deepseek-v4-pro` | 主模型 |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | `deepseek-v4-pro` | 对应 Opus 级别任务 |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | `deepseek-v4-pro` | 对应 Sonnet 级别任务 |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | `deepseek-v4-flash` | 轻量级子任务（速度快、成本低） |
| `CLAUDE_CODE_SUBAGENT_MODEL` | `deepseek-v4-flash` | 子代理使用的模型 |
| `CLAUDE_CODE_EFFORT_LEVEL` | `max` | 推理努力级别（agent 任务自动设为 max） |

> DeepSeek 收到不支持的模型名时会自动回退到 `deepseek-v4-flash`，因此如果模型名配置错误，可能不会报错但实际使用的是 flash 而非 pro。

---

## 六、启动 Claude Code

```bash
cd /path/to/my-project
claude
```

启动后 Claude Code 将通过 DeepSeek 的 Anthropic 兼容接口运行，默认使用 `deepseek-v4-pro` 模型。

---

## 附：Windows 用户配置（PowerShell）

```powershell
$env:ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
$env:ANTHROPIC_AUTH_TOKEN="<你的 DeepSeek API Key>"
$env:ANTHROPIC_MODEL="deepseek-v4-pro"
$env:ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-v4-pro"
$env:ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-v4-pro"
$env:ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
$env:CLAUDE_CODE_SUBAGENT_MODEL="deepseek-v4-flash"
$env:CLAUDE_CODE_EFFORT_LEVEL="max"
```

Windows 持久化：将以上变量添加到"系统环境变量"中即可。
