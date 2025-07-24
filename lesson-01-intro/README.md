# Lesson 1: 什么是Claude Code - 安装配置 + 终端集成

## 🎯 学习目标
- 理解Claude Code的核心概念和应用场景
- 完成Claude Code的完整安装配置
- 掌握终端集成的基本操作方法
- 完成第一个AI辅助编程项目

## 📖 课程内容

### 1. Claude Code简介

#### 什么是Claude Code？
Claude Code是Anthropic公司开发的AI编程助手，基于Claude 3.5 Sonnet模型，专门为代码生成、理解和协作而设计。

#### 核心特性
- 🤖 智能代码生成和自动补全
- 🔍 代码解释和技术文档生成
- 🐛 错误诊断和修复建议
- 🔄 代码重构和性能优化
- 💬 自然语言编程交互

#### 支持的编程语言
- Python 3.12+ (本课程主要使用)
- JavaScript/TypeScript
- Go, Rust, Java, C++
- Shell脚本和配置文件

### 2. 安装配置详解

#### 系统要求
- macOS 10.15+ / Windows 10+ / Linux
- Python 3.12+ (已配置)
- Node.js 18+ (用于CLI工具)
- Git (版本控制)

#### 安装步骤

**Step 1: 安装Claude CLI**
```bash
# 使用npm安装Claude命令行工具
npm install -g @anthropic-ai/claude-cli

# 验证安装
claude --version
```

**Step 2: API密钥配置**
```bash
# 设置API密钥
export ANTHROPIC_API_KEY="your-api-key-here"

# 或使用配置命令
claude auth login
```

**Step 3: 项目初始化**
```bash
# 创建课程项目目录
mkdir claude-lesson-01
cd claude-lesson-01

# 初始化Claude项目
claude init
```

### 3. 终端集成实战

#### 基本命令操作
```bash
# 启动交互式对话
claude chat

# 单次问答
claude ask "如何创建Python虚拟环境？"

# 代码文件分析
claude analyze main.py

# 生成代码文件
claude generate --file calculator.py --prompt "创建一个科学计算器"
```

#### 配置文件设置
```json
// .claude/config.json
{
  "model": "claude-3-5-sonnet-20241022",
  "max_tokens": 4000,
  "temperature": 0.1,
  "system_prompt": "你是一个专业的Python编程助手，使用Python 3.12语法。"
}
```

### 4. 实战项目：智能计算器 (15分钟)

我们将使用Claude Code创建一个功能完整的命令行计算器。

#### 项目需求
- 支持基本四则运算
- 包含科学计算功能
- 具备历史记录功能
- 错误处理和用户友好提示

#### 实现步骤
1. 使用Claude生成基础计算器框架
2. 添加科学计算功能
3. 实现历史记录存储
4. 完善错误处理机制

## 🛠️ 课堂练习

### 练习1: 环境验证
- [ ] 完成Claude CLI安装
- [ ] 配置API密钥
- [ ] 验证所有命令正常工作

### 练习2: 基础交互
- [ ] 使用`claude chat`进入交互模式
- [ ] 询问Python基础语法问题
- [ ] 让Claude解释一段示例代码

### 练习3: 代码生成
- [ ] 生成一个文件操作的Python脚本
- [ ] 要求添加异常处理
- [ ] 让Claude为代码添加详细注释

## 📝 课后作业

### 基础任务
使用Claude Code创建一个个人信息管理系统，包含以下功能：
- 添加联系人信息
- 查看所有联系人
- 搜索特定联系人
- 删除联系人记录
- 数据持久化存储

### 进阶任务
扩展个人信息管理系统：
- 添加数据导入/导出功能
- 实现联系人分组管理
- 添加生日提醒功能
- 创建简单的Web界面

### 挑战任务
创建一个智能文件整理工具：
- 自动识别文件类型
- 按规则分类整理文件
- 生成整理报告
- 支持自定义整理规则

## 🔗 相关资源

- [Claude API官方文档](https://docs.anthropic.com/claude/reference)
- [Python 3.12新特性](https://docs.python.org/3.12/whatsnew/3.12.html)
- [Prompt工程指南](https://docs.anthropic.com/claude/docs/prompt-engineering)

## 📋 检查清单

- [ ] 理解Claude Code的核心概念
- [ ] 完成安装和配置
- [ ] 掌握基本命令操作
- [ ] 完成课堂练习
- [ ] 提交课后作业