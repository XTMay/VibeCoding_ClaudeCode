# 🛠️ Claude Code 安装配置指南

## 📋 系统要求

### 基础环境
- **操作系统**：macOS 10.15+ / Windows 10+ / Linux (Ubuntu 18.04+)
- **Python**：3.12+ (已配置)
- **Node.js**：18.0+ (用于CLI工具)
- **Git**：2.0+ (版本控制)
- **内存**：至少4GB RAM
- **存储**：至少2GB可用空间

### 推荐工具
- **代码编辑器**：VSCode (推荐) / PyCharm / Vim
- **终端**：iTerm2 (macOS) / Windows Terminal / Zsh
- **包管理器**：npm / yarn

## 🚀 安装步骤

### Step 1: 环境准备

#### macOS安装
```bash
# 安装Homebrew (如果未安装)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 安装Node.js
brew install node

# 验证安装
node --version
npm --version
```

#### Windows安装
```powershell
# 使用Chocolatey安装Node.js
choco install nodejs

# 或下载官方安装包
# https://nodejs.org/en/download/

# 验证安装
node --version
npm --version
```

#### Linux (Ubuntu)安装
```bash
# 更新包管理器
sudo apt update

# 安装Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# 验证安装
node --version
npm --version
```

### Step 2: Claude CLI安装

```bash
# 全局安装Claude CLI
npm install -g @anthropic-ai/claude-cli

# 验证安装
claude --version

# 查看帮助信息
claude --help
```

### Step 3: API密钥配置

#### 获取API密钥
1. 访问 [Anthropic Console](https://console.anthropic.com)
2. 注册或登录账户
3. 导航到 "API Keys" 页面
4. 点击 "Create Key" 创建新密钥
5. 复制生成的API密钥

#### 配置密钥

**方法1：环境变量 (推荐)**
```bash
# 临时设置 (当前会话)
export ANTHROPIC_API_KEY="your-api-key-here"

# 永久设置 (添加到shell配置文件)
echo 'export ANTHROPIC_API_KEY="your-api-key-here"' >> ~/.bashrc
# 或 ~/.zshrc (如果使用zsh)

# 重新加载配置
source ~/.bashrc
```

**方法2：Claude CLI配置**
```bash
# 交互式配置
claude auth login

# 验证配置
claude auth status
```

**方法3：配置文件**
```bash
# 创建配置目录
mkdir -p ~/.claude

# 创建配置文件
cat > ~/.claude/config.json << EOF
{
  "api_key": "your-api-key-here",
  "model": "claude-3-5-sonnet-20241022",
  "max_tokens": 4000,
  "temperature": 0.1
}
EOF
```

### Step 4: 验证安装

```bash
# 测试API连接
claude ask "Hello, Claude!"

# 检查配置
claude config show

# 测试代码生成
claude ask "用Python写一个Hello World程序"
```

## ⚙️ 高级配置

### 自定义配置文件

```json
// ~/.claude/config.json
{
  "api_key": "your-api-key-here",
  "model": "claude-3-5-sonnet-20241022",
  "max_tokens": 4000,
  "temperature": 0.1,
  "system_prompt": "你是一个专业的Python编程助手，使用Python 3.12语法，提供清晰、实用的代码解决方案。",
  "output_format": "markdown",
  "auto_save": true,
  "history_limit": 100
}
```

### Shell集成配置

#### Zsh集成
```bash
# 添加到 ~/.zshrc
alias claude-chat="claude chat"
alias claude-ask="claude ask"
alias claude-gen="claude generate"

# 自动补全
eval "$(claude completion zsh)"
```

#### Bash集成
```bash
# 添加到 ~/.bashrc
alias claude-chat="claude chat"
alias claude-ask="claude ask"
alias claude-gen="claude generate"

# 自动补全
eval "$(claude completion bash)"
```

### VSCode集成

1. 安装Claude扩展 (如果可用)
2. 配置快捷键
3. 设置工作区配置

```json
// .vscode/settings.json
{
  "claude.apiKey": "${env:ANTHROPIC_API_KEY}",
  "claude.model": "claude-3-5-sonnet-20241022",
  "claude.autoSave": true,
  "claude.showInlineCompletion": true
}
```

## 🔧 故障排除

### 常见问题

#### 1. API密钥错误
```bash
# 检查环境变量
echo $ANTHROPIC_API_KEY

# 重新配置
claude auth login
```

#### 2. 网络连接问题
```bash
# 测试网络连接
curl -I https://api.anthropic.com

# 使用代理 (如果需要)
export HTTPS_PROXY=http://proxy.example.com:8080
```

#### 3. 权限问题
```bash
# 修复npm权限
sudo chown -R $(whoami) ~/.npm

# 重新安装
npm uninstall -g @anthropic-ai/claude-cli
npm install -g @anthropic-ai/claude-cli
```

### 日志和调试

```bash
# 启用详细日志
export CLAUDE_DEBUG=true

# 查看日志文件
tail -f ~/.claude/logs/claude.log

# 清理缓存
claude cache clear
```

## 📚 相关资源

- [Claude API文档](https://docs.anthropic.com/claude/reference)
- [Claude CLI GitHub](https://github.com/anthropics/claude-cli)
- [Anthropic Console](https://console.anthropic.com)
- [社区论坛](https://community.anthropic.com)

## 🆘 获取帮助

如果遇到安装问题，可以通过以下方式获取帮助：

1. 查看官方文档
2. 搜索GitHub Issues
3. 联系课程支持：climbaidev@gmail.com
4. 参与社区讨论