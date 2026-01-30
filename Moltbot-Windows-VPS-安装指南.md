# 🦞 Moltbot (Clawdbot) Windows VPS 完整安装指南

> 本指南专为编程小白设计，一步一步带你在 Windows VPS 上安装 Moltbot 个人AI助手。

---

## 📋 目录

1. [什么是 Moltbot？](#什么是-moltbot)
2. [安装前准备](#安装前准备)
3. [方法一：使用 WSL2 安装（推荐）](#方法一使用-wsl2-安装推荐)
4. [方法二：直接在 Windows 上安装](#方法二直接在-windows-上安装)
5. [配置 Moltbot](#配置-moltbot)
6. [连接聊天平台](#连接聊天平台)
7. [常见问题解答](#常见问题解答)

---

## 什么是 Moltbot？

Moltbot（原名 Clawdbot）是一个**个人AI助手**，可以：
- 连接到你常用的聊天平台（WhatsApp、Telegram、Discord、微信等）
- 使用 Claude、GPT 等大语言模型回复消息
- 在你自己的设备上运行，保护你的隐私

---

## 安装前准备

### 你需要准备：

1. **一台 Windows VPS**（Windows 10/11 或 Windows Server 2019/2022）
2. **远程桌面连接**（用于连接到你的 VPS）
3. **AI 服务的订阅**（以下任选一个）：
   - Anthropic Claude Pro/Max 订阅（推荐）
   - OpenAI ChatGPT Plus 订阅
   - 或者 API Key

### 连接到你的 VPS

1. 在你的电脑上，按 `Win + R` 键
2. 输入 `mstsc` 并按回车
3. 输入你的 VPS IP 地址
4. 输入用户名和密码登录

---

## 方法一：使用 WSL2 安装（推荐）

> **官方强烈推荐此方法**，因为 Moltbot 在 Linux 环境下运行更稳定。

### 第一步：安装 WSL2

1. **以管理员身份打开 PowerShell**
   - 右键点击屏幕左下角的 Windows 图标
   - 选择 "Windows PowerShell (管理员)" 或 "终端 (管理员)"

2. **输入以下命令安装 WSL2**
   ```powershell
   wsl --install
   ```

3. **等待安装完成**（可能需要几分钟）

4. **重启 VPS**
   ```powershell
   shutdown /r /t 0
   ```

5. **重新连接到 VPS 后，WSL 会自动启动并要求你创建用户**
   - 输入一个用户名（只能用小写字母，例如：`moltbot`）
   - 输入一个密码（输入时不会显示，这是正常的）
   - 再次输入密码确认

### 第二步：安装 Node.js 22

1. **打开 WSL**（在开始菜单搜索 "Ubuntu" 或 "WSL"）

2. **依次输入以下命令**（每输入一行按回车）：

   ```bash
   # 更新软件包列表
   sudo apt update
   ```
   （系统会要求输入你刚才设置的密码）

   ```bash
   # 安装必要工具
   sudo apt install -y curl
   ```

   ```bash
   # 安装 Node.js 版本管理器 (nvm)
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
   ```

   ```bash
   # 重新加载配置
   source ~/.bashrc
   ```

   ```bash
   # 安装 Node.js 22
   nvm install 22
   ```

   ```bash
   # 设置默认版本
   nvm use 22
   nvm alias default 22
   ```

3. **验证安装**
   ```bash
   node --version
   ```
   应该显示类似 `v22.x.x` 的版本号

### 第三步：安装 Moltbot

1. **使用 npm 全局安装 Moltbot**
   ```bash
   npm install -g moltbot@latest
   ```

2. **运行入门向导**
   ```bash
   moltbot onboard --install-daemon
   ```

3. **按照向导提示操作**：
   - 选择你要使用的 AI 模型提供商（推荐 Anthropic）
   - 输入你的 API Key 或登录你的订阅账号
   - 选择你要连接的聊天平台
   - 配置其他选项

### 第四步：启动 Moltbot

```bash
# 启动 Gateway 服务
moltbot gateway --port 18789 --verbose
```

你应该看到类似这样的输出：
```
🦞 Moltbot Gateway running at http://127.0.0.1:18789
```

**恭喜！Moltbot 已经在运行了！**

---

## 方法二：直接在 Windows 上安装

> 如果 WSL2 安装失败，可以尝试此方法。

### 第一步：安装 Node.js

1. **打开浏览器**，访问：https://nodejs.org/

2. **下载 Node.js 22 LTS**
   - 点击绿色的 "LTS" 下载按钮
   - 或直接访问：https://nodejs.org/dist/v22.13.1/node-v22.13.1-x64.msi

3. **运行安装程序**
   - 双击下载的 `.msi` 文件
   - 点击 "Next" 继续
   - 勾选 "I accept the terms..." 接受协议
   - 继续点击 "Next"
   - **重要**：在 "Tools for Native Modules" 页面，勾选 "Automatically install the necessary tools"
   - 点击 "Install" 开始安装
   - 等待安装完成，点击 "Finish"

4. **验证安装**
   - 按 `Win + R`，输入 `cmd`，按回车打开命令提示符
   - 输入：
     ```cmd
     node --version
     ```
   - 应该显示 `v22.x.x`

### 第二步：安装 Moltbot

1. **打开命令提示符**（按 `Win + R`，输入 `cmd`，按回车）

2. **安装 Moltbot**
   ```cmd
   npm install -g moltbot@latest
   ```

3. **运行入门向导**
   ```cmd
   moltbot onboard --install-daemon
   ```

4. **按照向导提示完成配置**

### 第三步：启动 Moltbot

```cmd
moltbot gateway --port 18789 --verbose
```

---

## 配置 Moltbot

### 配置 AI 模型

Moltbot 支持多种 AI 模型：

| 提供商 | 模型 | 推荐程度 |
|--------|------|----------|
| Anthropic | Claude 4 Opus | ⭐⭐⭐⭐⭐ 强烈推荐 |
| Anthropic | Claude 4 Sonnet | ⭐⭐⭐⭐ |
| OpenAI | GPT-4 | ⭐⭐⭐ |
| 其他 | 本地模型 (Ollama) | ⭐⭐ |

### 使用 Anthropic Claude（推荐）

1. 访问 https://www.anthropic.com/ 注册账号
2. 订阅 Claude Pro 或 Max 计划
3. 在 Moltbot 向导中选择 "Anthropic" 并登录

### 使用 API Key

如果你有 API Key：
```bash
moltbot config set models.default.apiKey "你的API密钥"
```

---

## 连接聊天平台

### WhatsApp

1. 运行配置命令：
   ```bash
   moltbot channel add whatsapp
   ```

2. 扫描显示的二维码（用你的 WhatsApp 扫描）

3. 等待连接成功

### Telegram

1. 在 Telegram 中找到 @BotFather
2. 发送 `/newbot` 创建一个新机器人
3. 记下给你的 Token
4. 运行：
   ```bash
   moltbot channel add telegram --token "你的Bot Token"
   ```

### Discord

1. 访问 https://discord.com/developers/applications
2. 创建一个新应用
3. 在 Bot 页面创建机器人并获取 Token
4. 运行：
   ```bash
   moltbot channel add discord --token "你的Bot Token"
   ```

### 微信（通过 WebChat）

Moltbot 提供了一个网页聊天界面：
```bash
moltbot gateway --port 18789
```
然后在浏览器访问：http://你的VPS_IP:18789

---

## 让 Moltbot 后台运行

### 在 WSL2 中使用 Screen

1. 安装 screen：
   ```bash
   sudo apt install -y screen
   ```

2. 创建一个新的 screen 会话：
   ```bash
   screen -S moltbot
   ```

3. 启动 Moltbot：
   ```bash
   moltbot gateway --port 18789
   ```

4. 按 `Ctrl + A`，然后按 `D` 分离会话

5. 以后要重新连接：
   ```bash
   screen -r moltbot
   ```

### 在 Windows 中使用任务计划程序

1. 按 `Win + R`，输入 `taskschd.msc`
2. 右侧点击 "创建基本任务"
3. 名称填写 "Moltbot"
4. 触发器选择 "当计算机启动时"
5. 操作选择 "启动程序"
6. 程序填写 `moltbot`，参数填写 `gateway --port 18789`
7. 完成创建

---

## 常用命令速查表

| 命令 | 说明 |
|------|------|
| `moltbot onboard` | 运行入门向导 |
| `moltbot gateway` | 启动 Gateway 服务 |
| `moltbot doctor` | 检查配置问题 |
| `moltbot update` | 更新 Moltbot |
| `moltbot config show` | 显示当前配置 |
| `moltbot channel list` | 列出已连接的频道 |
| `moltbot agent --message "你好"` | 发送测试消息 |

---

## 常见问题解答

### Q: 安装时报错 "npm not found"
**A:** Node.js 没有正确安装。请重新按照步骤安装 Node.js，并重启命令提示符。

### Q: WSL 安装失败
**A:** 
1. 确保你的 Windows 版本是 Windows 10 (1903+) 或 Windows 11
2. 确保已启用虚拟化（在 BIOS 中）
3. 尝试运行：`dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`

### Q: moltbot 命令找不到
**A:** 
- 在 Windows 中，关闭并重新打开命令提示符
- 在 WSL 中，运行：`source ~/.bashrc`

### Q: WhatsApp 二维码扫描失败
**A:** 
1. 确保你的 WhatsApp 是最新版本
2. 尝试删除 WhatsApp 频道并重新添加：
   ```bash
   moltbot channel remove whatsapp
   moltbot channel add whatsapp
   ```

### Q: Gateway 启动后无法访问
**A:** 
1. 检查防火墙设置，确保端口 18789 已开放
2. 在 Windows 防火墙中添加入站规则

### Q: 如何查看日志？
**A:** 
```bash
moltbot gateway --verbose
```

### Q: 如何完全卸载 Moltbot？
**A:** 
```bash
npm uninstall -g moltbot
rm -rf ~/.config/moltbot  # WSL/Linux
# 或在 Windows 中删除 %APPDATA%\moltbot 文件夹
```

---

## 获取帮助

- **官方文档**: https://docs.molt.bot
- **Discord 社区**: https://discord.gg/clawd
- **GitHub Issues**: https://github.com/moltbot/moltbot/issues
- **入门指南**: https://docs.molt.bot/start/getting-started

---

## 下一步

安装完成后，你可以：

1. 📱 连接更多聊天平台
2. 🔧 安装技能插件（Skills）
3. 🎨 自定义助手行为
4. 🔒 配置安全设置

详细说明请参阅官方文档：https://docs.molt.bot

---

*本指南最后更新于 2026年1月*
*适用于 Moltbot 版本 v2026.1.24 及以上*
