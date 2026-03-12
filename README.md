# AgentsMakingFriends
agents should making friends and asking for help from their friends when necessary .

<img width="793" height="809" alt="1773309377493" src="https://github.com/user-attachments/assets/825bea00-ed67-46c8-ad74-1f5688f15dbe" />

# OpenClaw A2A Protocol Skill
## 中文文档  
---

## English Documentation

### 📦 Skill Introduction
This is an A2A (Agent-to-Agent) protocol skill implementation for OpenClaw agents, following the open A2A standard initiated by Google. It enables different AI agents to communicate with each other and collaborate on tasks.

### ✨ Features
- 🚀 **Server Mode**: Expose OpenClaw agent as an A2A service for other agents to call
- 📞 **Client Mode**: Call other remote A2A-compatible agents
- 🔒 **Optional Authentication**: Support Bearer Token authentication to protect service security
- 📝 **Standard Protocol**: Fully compliant with A2A protocol standard, compatible with all A2A-enabled agents
- ⚡ **Lightweight**: Implemented with Python standard library, no extra dependencies, easy to deploy

### 🛠️ Installation & Usage
#### Prerequisites
- OpenClaw runtime environment
- Python 3.7+

#### Installation Steps
1. Copy the skill folder to OpenClaw skills directory:
```bash
cp -r a2a-protocol /usr/lib/node_modules/openclaw/skills/
```

2. Verify installation:
```bash
openclaw skills list | grep a2a-protocol
```
If you see `✓ ready   📦 a2a-protocol`, the installation is successful.

### 🚀 Quick Start
#### Scenario 1: As server, for other agents to call
1. Start A2A server, listen on specified port:
```bash
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_server.py --port 8090
```

2. (Optional) Add authentication:
```bash
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_server.py --port 8090 --token your-secret-token
```

3. Other agents can access via:
   - Agent Card: `http://<your-server-ip>:8090/.well-known/agent.json`
   - API Endpoint: `http://<your-server-ip>:8090/rpc`

#### Scenario 2: As client, call other agents
Use the provided client script to call remote A2A agents:
```bash
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_client.py \
  "http://remote-agent-ip:8080/a2a" \
  "Please help me write a quicksort algorithm in Python" \
  "remote-agent-token (if any)"
```

### 💡 Usage Example: Two Agents Collaboration
Suppose we have two agents:
- Agent A: Deployed on server 192.168.1.100, running A2A server, good at programming
- Agent B: Deployed on server 192.168.1.200, good at data analysis

#### Step 1: Start service on Agent A
```bash
# Execute on Agent A
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_server.py --port 8090
```

#### Step 2: Agent B calls Agent A's capability
```bash
# Execute on Agent B, request Agent A to write code
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_client.py \
  "http://192.168.1.100:8090/rpc" \
  "Please write a Python script to read CSV file and calculate average value"
```

#### Step 3: Agent A returns result
After receiving the request, Agent A will process and return the Python code. Agent B can directly use this code for data analysis work.

### 📡 API Description
#### Get Agent Card
```http
GET /.well-known/agent.json
```

Response example:
```json
{
  "name": "OpenClaw Agent",
  "description": "OpenClaw Personal AI Assistant",
  "url": "http://localhost:8090/a2a",
  "capabilities": {
    "streaming": false,
    "pushNotifications": false
  },
  "skills": [
    {"name": "chat", "description": "Conversational capability"},
    {"name": "execute", "description": "Execute commands"}
  ],
  "authentication": {
    "schemes": []
  }
}
```

#### Send Message
```http
POST /rpc
Content-Type: application/json
```

Request body:
```json
{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "1",
  "params": {
    "message": {
      "role": "user",
      "parts": [
        {"type": "text", "text": "Your question content"}
      ]
    }
  }
}
```

Response example:
```json
{
  "jsonrpc": "2.0",
  "result": {
    "id": "task-77c392877850b742",
    "status": "completed",
    "artifacts": [
      {
        "parts": [
          {"type": "text", "text": "Agent response content"}
        ]
      }
    ]
  },
  "id": "1"
}
```

### 📄 Directory Structure
```
a2a-protocol/
├── SKILL.md              # Skill description file
├── references/
│   └── a2a-api.md        # Complete A2A API reference documentation
└── scripts/
    ├── a2a_server.py     # A2A server script
    ├── a2a_client.py     # A2A client script
    └── agent_card.json   # Agent card template
```

---

### 📦 技能介绍
这是一个为 OpenClaw 智能体实现的 A2A (Agent-to-Agent) 协议技能，遵循 Google 发起的 A2A 开放标准，让不同的 AI 智能体之间可以互相通信、协作完成任务。

### ✨ 功能特性
- 🚀 **服务端模式**：将 OpenClaw 智能体暴露为 A2A 服务，供其他智能体调用
- 📞 **客户端模式**：调用其他支持 A2A 协议的远程智能体
- 🔒 **可选认证**：支持 Bearer Token 身份验证，保护服务安全
- 📝 **标准协议**：完全遵循 A2A 协议标准，兼容所有 A2A 兼容的智能体
- ⚡ **轻量高效**：基于 Python 标准库实现，无额外依赖，部署简单

### 🛠️ 安装使用
#### 前置要求
- OpenClaw 运行环境
- Python 3.7+

#### 安装步骤
1. 将技能文件夹复制到 OpenClaw 技能目录：
```bash
cp -r a2a-protocol /usr/lib/node_modules/openclaw/skills/
```

2. 验证安装：
```bash
openclaw skills list | grep a2a-protocol
```
看到 `✓ ready   📦 a2a-protocol` 即为安装成功。

### 🚀 快速开始
#### 场景1：作为服务端，供其他智能体调用
1. 启动 A2A 服务端，监听指定端口：
```bash
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_server.py --port 8090
```

2. （可选）添加身份验证：
```bash
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_server.py --port 8090 --token your-secret-token
```

3. 其他智能体可以通过以下地址访问：
   - Agent 卡片：`http://<your-server-ip>:8090/.well-known/agent.json`
   - API 端点：`http://<your-server-ip>:8090/rpc`

#### 场景2：作为客户端，调用其他智能体
使用提供的客户端脚本调用远程 A2A 智能体：
```bash
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_client.py \
  "http://remote-agent-ip:8080/a2a" \
  "请帮我写一个Python的快速排序算法" \
  "remote-agent-token（如果有）"
```

### 💡 使用示例：两个智能体协作
假设我们有两个智能体：
- 智能体A：部署在服务器 192.168.1.100，运行 A2A 服务端，擅长编程
- 智能体B：部署在服务器 192.168.1.200，擅长数据分析

#### 步骤1：在智能体A上启动服务
```bash
# 智能体A上执行
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_server.py --port 8090
```

#### 步骤2：智能体B调用智能体A的能力
```bash
# 智能体B上执行，请求智能体A写代码
python3 /usr/lib/node_modules/openclaw/skills/a2a-protocol/scripts/a2a_client.py \
  "http://192.168.1.100:8090/rpc" \
  "请写一个Python脚本，读取CSV文件并计算平均值"
```

#### 步骤3：智能体A返回结果
智能体A接收到请求后，会处理并返回Python代码，智能体B可以直接使用这个代码进行数据分析工作。

### 📡 API 说明
#### 获取 Agent 卡片
```http
GET /.well-known/agent.json
```

返回示例：
```json
{
  "name": "OpenClaw Agent",
  "description": "OpenClaw Personal AI Assistant",
  "url": "http://localhost:8090/a2a",
  "capabilities": {
    "streaming": false,
    "pushNotifications": false
  },
  "skills": [
    {"name": "chat", "description": "Conversational capability"},
    {"name": "execute", "description": "Execute commands"}
  ],
  "authentication": {
    "schemes": []
  }
}
```

#### 发送消息
```http
POST /rpc
Content-Type: application/json
```

请求体：
```json
{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "1",
  "params": {
    "message": {
      "role": "user",
      "parts": [
        {"type": "text", "text": "你的问题内容"}
      ]
    }
  }
}
```

返回示例：
```json
{
  "jsonrpc": "2.0",
  "result": {
    "id": "task-77c392877850b742",
    "status": "completed",
    "artifacts": [
      {
        "parts": [
          {"type": "text", "text": "智能体的响应内容"}
        ]
      }
    ]
  },
  "id": "1"
}
```

### 📄 目录结构
```
a2a-protocol/
├── SKILL.md              # 技能说明文件
├── references/
│   └── a2a-api.md        # 完整的A2A API参考文档
└── scripts/
    ├── a2a_server.py     # A2A服务端脚本
    ├── a2a_client.py     # A2A客户端脚本
    └── agent_card.json   # Agent卡片模板
```


## 📝 License
MIT License - Feel free to use and modify as needed.
