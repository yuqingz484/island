# DeepSeek API 配置全流程记录（Mac）

> ⚠️ **保密与风控声明**
> 
> 本文档仅供**手动参考**，不可作为 Claude Code Skill 自动执行。其中包含的 shell 命令涉及系统软件安装和配置文件修改，已全部注释处理，需逐条手动复制执行。
> 
> **凭证安全**：将真实 API Key 写入环境变量后，切勿将包含 Key 的任何配置文件（如 `~/.zshrc`）内容粘贴给大模型。一旦密码给了大模型，视为已泄露。

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

> ⚠️ 以下命令涉及远程脚本下载后执行（`curl | bash`），务必确认来源可信后再手动运行。

```bash
# 如果没有 Homebrew，先安装 Homebrew（以下为参考，请手动前往 https://brew.sh 确认最新安装方式）
# /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 安装 Node.js
# brew install node

# 验证版本（需 >= 18）
# node --version
```

---

## 三、安装 Claude Code

```bash
# npm install -g @anthropic-ai/claude-code

# 验证安装
# claude --version
```

---

## 四、配置环境变量

### 4.1 临时配置（当前终端有效）

> 以下为参考格式，`<你的 DeepSeek API Key>` 替换为真实 key 后**手动**逐行粘贴到终端执行。

```bash
# export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
# export ANTHROPIC_AUTH_TOKEN=<你的 DeepSeek API Key>
# export ANTHROPIC_MODEL=deepseek-v4-pro
# export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro
# export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro
# export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash
# export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-flash
# export CLAUDE_CODE_EFFORT_LEVEL=max
```

### 4.2 持久化配置（重启终端后仍然有效）

> ⚠️ 以下操作会修改 `~/.zshrc`，请手动用编辑器打开该文件添加内容，或逐行执行注释中的命令。

将以下环境变量手动添加到 `~/.zshrc`（Mac 默认 shell 为 zsh）：

```bash
# DeepSeek API + Claude Code 配置
# export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
# export ANTHROPIC_AUTH_TOKEN=<你的 DeepSeek API Key>
# export ANTHROPIC_MODEL=deepseek-v4-pro
# export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro
# export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro
# export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash
# export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-flash
# export CLAUDE_CODE_EFFORT_LEVEL=max
```

> 添加完成后，手动执行 `source ~/.zshrc` 使配置生效。

> 🚫 **凭证安全提醒**：将真实 API Key 写入 `~/.zshrc` 后，切勿将该文件内容粘贴给任何大模型。如需调试环境变量问题，先用 `sed` 脱敏或只贴变量名不贴值。

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

command option p显示文件夹路径
claude
cd /desktop/（想去的文件夹）
/（想运行的文件）


启动后 Claude Code 将通过 DeepSeek 的 Anthropic 兼容接口运行，默认使用 `deepseek-v4-pro` 模型。

---

## 附：Windows 用户配置（PowerShell）

```powershell
# $env:ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
# $env:ANTHROPIC_AUTH_TOKEN="<你的 DeepSeek API Key>"
# $env:ANTHROPIC_MODEL="deepseek-v4-pro"
# $env:ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-v4-pro"
# $env:ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-v4-pro"
# $env:ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
# $env:CLAUDE_CODE_SUBAGENT_MODEL="deepseek-v4-flash"
# $env:CLAUDE_CODE_EFFORT_LEVEL="max"
```

Windows 持久化：将以上变量添加到"系统环境变量"中即可。


## 附：claude更新命令
# brew upgrade claude-code
