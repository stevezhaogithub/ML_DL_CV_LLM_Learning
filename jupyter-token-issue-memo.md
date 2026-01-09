# Jupyter Notebook Token 认证问题备忘录

## 📋 问题描述

**场景**：运行 `jupyter notebook` 启动后，默认在 Safari 浏览器中可以正常使用，但将 URL 地址复制到 Chrome 浏览器后，提示需要登录，却不知道密码。

**现象**：
- Safari 浏览器：正常访问 ✅
- Chrome 浏览器：提示需要 Token 或密码 ❌

---

## 🔍 原因分析

### 1. Jupyter 使用 Token 认证，而非密码

Jupyter Notebook 默认使用 **Token（令牌）认证机制**，而不是传统的用户名/密码方式。

每次启动 Jupyter Notebook 时，系统会自动生成一个随机的 Token 字符串用于身份验证。

### 2. Token 存储在浏览器 Cookie 中

当你首次通过带 Token 的完整 URL 访问 Jupyter 时：
- 浏览器会将 Token 存储在 Cookie 中
- 后续访问同一浏览器时，Cookie 自动验证身份

### 3. 为什么复制 URL 到 Chrome 无���访问？

| 原因 | 说明 |
|------|------|
| **Cookie 不共享** | Safari 的 Cookie 不会同步到 Chrome |
| **URL 可能不完整** | 复制时可能丢失了 `?token=xxx` 参数 |
| **Token 是一次性验证** | 首次访问后，Token 信息存入 Cookie，URL 栏可能不再显示完整 Token |

### 4. 完整的 Token URL 示例

```
http://localhost:8888/?token=abc123def456ghi789jkl012mno345pqr678stu901vwx234
                       └──────────────────────────────────────────────────┘
                                        这部分是 Token
```

---

## ✅ 解决方案

### 方案一：获取完整 Token URL（推荐 - 临时解决）

```bash
# 在终端中运行此命令查看所有运行中的 Jupyter 服务
jupyter notebook list
```

**输出示例**：
```
Currently running servers:
http://localhost:8888/?token=abc123def456...  ::  /Users/username/notebooks
```

将完整的带 Token 的 URL 复制到 Chrome 中即可访问。

---

### 方案二：在登录页面手动输入 Token

1. 在 Chrome 打开 `http://localhost:8888`
2. 在登录页面找到 **"Token"** 输入框
3. 运行 `jupyter notebook list` 获取 Token
4. 只复制 `token=` 后面的字符串粘贴到输入框

---

### 方案三：设置永久密码（推荐 - 一劳永逸）

```bash
# 设置 Jupyter 密码
jupyter notebook password
```

**交互过程**：
```
Enter password: ********
Verify password: ********
[NotebookPasswordApp] Wrote hashed password to ~/.jupyter/jupyter_notebook_config.json
```

设置后，所有浏览器都可以使用这个密码登录。

---

### 方案四：指定默认浏览器启动

```bash
# 使用 Chrome 启动
jupyter notebook --browser=chrome

# macOS 完整命令
jupyter notebook --browser="open -a /Applications/Google\ Chrome.app %s"
```

---

### 方案五：配置文件设置（永久生效）

```bash
# 生成配置文件（如果不存在）
jupyter notebook --generate-config
```

编辑 `~/.jupyter/jupyter_notebook_config.py`：

```python
# 设置默认浏览器
c.NotebookApp.browser = 'chrome'

# 或者禁用 Token（仅限本地安全环境，不推荐）
c.NotebookApp.token = ''

# 禁用密码（仅限本地安全环境，不推荐）
c.NotebookApp.password = ''
```

---

## 📊 方案对比

| 方案 | 难度 | 持久性 | 安全性 | 推荐场景 |
|------|------|--------|--------|----------|
| 获取 Token URL | ⭐ | 临时 | 高 | 临时切换浏览器 |
| 手动输入 Token | ⭐ | 临时 | 高 | 偶尔使用 |
| 设置密码 | ⭐⭐ | 永久 | 高 | **日常使用（推荐）** |
| 指定浏览器 | ⭐⭐ | 永久 | 高 | 固定使用某浏览器 |
| 禁用 Token | ⭐⭐ | 永久 | ⚠️ 低 | 仅限本地测试环境 |

---

## 🚀 快速参考命令

```bash
# 查看运行中的 Jupyter 服务和 Token
jupyter notebook list

# 设置密码
jupyter notebook password

# 指定浏览器启动
jupyter notebook --browser=chrome

# 指定端口启动
jupyter notebook --port=9999

# 生成配置文件
jupyter notebook --generate-config
```

---

## ⚠️ 安全提示

1. **不要在公共网络上禁用 Token**：这会让任何人都能访问你的 Notebook
2. **设置强密码**：如果选择使用密码认证，请使用复杂密码
3. **Token 不要分享**：Token 相当于临时密码，不要发送给他人

---

*最后更新：2026-01-09*