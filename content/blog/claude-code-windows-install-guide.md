+++
title = "Windows 下 Claude Code 安装教程"
date = 2026-06-25
description = "从零开始，在 Windows 上安装 Git、Node.js、VS Code、Anaconda 和 Claude Code，并配置 DeepSeek API Key 的完整指南。"
[taxonomies]
categories = ["Tutorial"]
tags = ["claude-code", "windows", "dev-tools"]
+++

# Windows 下 Claude Code 安装教程

本教程使用 **命令提示符 (cmd)** 完成所有安装步骤。请按顺序执行。

---

## 0. 准备工作

> **提示：** 建议以管理员身份运行 cmd（右键"命令提示符" → "以管理员身份运行"），避免安装过程中的权限问题。

```cmd
:: 确认当前用户有管理员权限
whoami
```

---

## 1. 安装 Git

Git 是版本控制工具，Claude Code 依赖 Git 进行部分操作。

### 1.1 下载安装

访问 [https://git-scm.com/download/win](https://git-scm.com/download/win) 下载 64-bit 安装包，双击运行。


### 1.2 验证安装

安装完成后，**重新打开 cmd**，运行：

```cmd
git --version
:: 应输出类似：git version 2.47.x.windows.x
```



---

## 2. 安装 Node.js

Claude Code 是一个 Node.js CLI 工具，需要 Node.js 18+。


### 2.2 或手动下载安装

访问 [https://nodejs.org](https://nodejs.org) 下载 LTS 版本（.msi 安装包），双击运行，一路 Next 即可。

### 2.3 验证安装

**重新打开 cmd**：

```cmd
node --version
:: 应输出类似：v22.x.x

npm --version
:: 应输出类似：10.x.x
```

---

## 3. 安装 Visual Studio Code



### 3.2 或手动下载安装

访问 [https://code.visualstudio.com](https://code.visualstudio.com) 下载并安装。

### 3.3 验证安装

```cmd
code --version
:: 应输出版本信息

:: 如果提示 'code' 不是内部命令，将 VS Code 加入 PATH：
:: 打开 VS Code → Ctrl+Shift+P → 输入 "Shell Command: Install 'code' command in PATH"
```

---

## 4. 安装 Anaconda

Anaconda 用于管理 Python 环境和数据科学包。

### 4.1 下载安装

访问 [https://www.anaconda.com/download](https://www.anaconda.com/download) 下载 Windows 64-Bit 安装包，双击运行。



>
### 4.2 手动添加 conda 到 PATH（如未勾选）

```cmd
:: 将以下路径加入系统 PATH（根据实际安装路径调整）
setx PATH "%PATH%;C:\Users\%USERNAME%\anaconda3"
setx PATH "%PATH%;C:\Users\%USERNAME%\anaconda3\Scripts"
setx PATH "%PATH%;C:\Users\%USERNAME%\anaconda3\Library\bin"
```

### 4.3 验证安装

**重新打开 cmd**：

```cmd
conda --version
:: 应输出类似：conda 24.x.x

python --version
:: 应输出类似：Python 3.12.x
```


---

## 5. 安装 Claude Code

所有依赖就绪后，安装 Claude Code 本身。

### 5.1 全局安装

```cmd
npm install -g @anthropic-ai/claude-code
```

> 如果国内网络慢，可临时使用镜像：
> ```cmd
> npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
> ```

### 5.2 验证安装

```cmd
claude --version
:: 应输出版本号
```

### 5.3 启动并登录

```cmd
claude
```

首次运行会引导你在浏览器中完成 Anthropic 账号登录。登录后即可在终端中使用 Claude Code。

---

## 6. 获取 DeepSeek API Key

Claude Code 支持通过第三方 API 提供商使用模型，DeepSeek 是国内可直接访问的高性价比选择。

### 6.1 注册并购买

1. 打开浏览器访问 [https://platform.deepseek.com](https://platform.deepseek.com)
2. 点击右上角 **Sign Up** 注册账号（支持手机号 / 邮箱注册）
3. 登录后进入控制台，点击左侧 **API Keys**
4. 点击 **创建 API Key**，输入名称（如 `claude-code`），生成后 **立即复制保存**（只显示一次）
5. 进入 **充值 / Top Up** 页面，按需充值（支持支付宝/微信）

### 6.2 配置 Claude Code 使用 DeepSeek

创建或编辑 Claude Code 的配置文件：

```cmd
:: 创建配置目录（如果不存在）
mkdir %USERPROFILE%\.claude

:: 用 VS Code 打开配置文件
code %USERPROFILE%\.claude\settings.json
```

在 `settings.json` 中添加以下内容（将 `<你的API-KEY>` 替换为实际 key）：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "你的key",
    "ANTHROPIC_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_EFFORT_LEVEL": "max"
  }
}

```

### 6.3 验证连接

```cmd
claude --version
:: 确认 Claude Code 可正常启动

claude
:: 进入交互界面，测试是否能正常对话
```

> **说明：** DeepSeek 按 token 计费，费用远低于海外 API。具体定价见 [platform.deepseek.com/pricing](https://platform.deepseek.com/pricing)。

---

## 7. 环境变量速查

安装完成后，确认以下命令全部可用：

```cmd
git --version
node --version
npm --version
code --version
conda --version
python --version
claude --version
```

如有命令不可用，检查对应的 PATH 环境变量：

```cmd
echo %PATH%
```

---

## 常见问题

| 问题 | 解决方案 |
|------|----------|
| `claude` 不是内部命令 | 确认 `npm install -g` 成功，检查 `%APPDATA%\npm` 在 PATH 中 |
| npm 安装报权限错误 | 以管理员身份运行 cmd 重试 |
| conda 命令不可用 | 从开始菜单打开 Anaconda Prompt，或手动添加 PATH |


---

## 8. Claude Code 常用用法

### 8.1 基础交互

```cmd
:: 进入交互模式
claude

:: 直接提问（单次，不进入交互）
claude "帮我写一个 Python 爬虫"

:: 从文件读取指令
claude < instructions.txt

:: 管道输入
type error.log | claude "帮我分析这个报错"
```

### 8.2 文件操作

在 Claude Code 交互模式下：

```
:: 指定要操作的文件
> 读取 src/main.py 的内容并优化性能

:: 直接让 Claude 写代码到文件
> 在 utils/helpers.py 中写一个日期格式化函数

:: 跨文件修改
> 把 config/settings.py 中的数据库配置抽到 .env 文件中

:: 修复 bug
> 运行测试报错了，帮我修复 test_user.py 中的断言错误
```

### 8.3 项目管理

```cmd
:: 初始化当前目录为 Claude Code 项目（生成 CLAUDE.md）
claude init

:: 让 Claude 记住项目偏好
> 以后这个项目都用 pytest 运行测试，不要用 unittest

:: 查看已记住的项目设置
claude config
```

### 8.4 会话管理

```cmd
:: 继续上一次对话
claude --continue
:: 或简写
claude -c

:: 查看历史会话列表
claude --resume
:: 或简写
claude -r
```

### 8.5 常用斜杠命令（交互模式下）

| 命令 | 作用 |
|------|------|
| `/help` | 查看所有可用命令 |
| `/clear` | 清空当前对话上下文 |
| `/compact` | 压缩上下文（长对话后释放 token） |
| `/init` | 初始化项目 CLAUDE.md |
| `/config` | 查看/修改配置 |
| `/doctor` | 诊断安装问题 |
| `/review` | 审查代码变更 |
| `/git` | 查看 Git 状态 / 生成 commit message |
| `/theme` | 切换主题（亮色/暗色） |
| `/mcp` | 管理 MCP 服务器 |
| `/memory` | 管理跨会话记忆 |

### 8.6 快捷键（交互模式下）

| 快捷键 | 作用 |
|--------|------|
| `Ctrl+C` | 中断当前生成 |
| `Ctrl+D` | 退出 Claude Code |
| `↑` / `↓` | 浏览历史命令 |
| `Ctrl+R` | 搜索历史命令 |
| `Shift+Enter` | 多行输入（换行不提交） |
| `Ctrl+O` | 切换思考过程显示 |
| `TAB` | 自动补全文件路径 |


### 8.8 实用技巧

```cmd
:: 设置模型（如果使用 DeepSeek 等多模型 API）
claude --model deepseek-v4-pro[1m]

:: 跳过权限确认（自动化脚本中慎用）
claude --dangerously-skip-permissions

:: 调试模式（打印详细日志）
claude --debug
```

### 8.9 典型工作流

**日常开发：**
```
1. cd 到项目目录
2. claude "帮我实现用户登录接口，用 Flask"
3. 检查生成的代码，运行测试
4. claude -c  "把密码改成 bcrypt 加密"
5. git add . && git commit -m "..."
```

**代码审查：**
```
1. claude
2. > /review
3. Claude 审查当前未提交的变更，给出建议
```

**学习新技术：**
```
1. claude "解释一下 Docker 的核心概念，我是新手"
2. 追问：> 帮我写一个 Flask 应用的 Dockerfile
```

---

## 版本信息

| 工具 | 安装方式 | 验证命令 |
|------|----------|----------|
| Git | 官网下载 / winget | `git --version` |
| Node.js | winget / 官网下载 | `node --version` |
| VS Code | winget / 官网下载 | `code --version` |
| Anaconda | 官网下载 | `conda --version` |
| Claude Code | npm 全局安装 | `claude --version` |
| DeepSeek API Key | [platform.deepseek.com](https://platform.deepseek.com) 注册购买 | `claude` 启动后正常对话 |

> 📅 编写日期：2025年6月
