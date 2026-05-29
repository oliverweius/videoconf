# VideoConf — Python 视频会议应用

基于 Python + FastAPI + WebRTC 的完整视频会议系统。

## 功能

- 🎥 **多人实时视频通话**（WebRTC P2P）
- 🎤 **语音通话**
- 💬 **文字聊天**（含历史记录）
- 👑 **主持人控制**：
  - 关闭/开启他人麦克风
  - 关闭/开启他人摄像头
  - 踢出参与者
  - 锁定会议室（禁止新人加入）
- 📋 **AI 会议纪要**（会议结束后自动生成，发给主持人）
- 🔗 **邀请链接**（一键复制，分享给他人）

## 快速启动

### 1. 安装依赖
```bash
cd videoconf
pip install -r requirements.txt
```

### 2. 设置 API Key（可选，用于 AI 会议纪要）
```bash
# macOS/Linux
export ANTHROPIC_API_KEY=你的key

# Windows
set ANTHROPIC_API_KEY=你的key
```

### 3. 启动服务器
```bash
python server.py
```

### 4. 打开浏览器
- 本机：http://localhost:8000
- 局域网其他设备：http://你的IP:8000
  （运行 `ipconfig`(Win) 或 `ifconfig`(Mac/Linux) 查看 IP）

## 局域网多人使用

1. 主持人电脑运行 `python server.py`
2. 查看本机局域网 IP（如 `192.168.1.100`）
3. 把 `http://192.168.1.100:8000` 发给同一 WiFi 下的参与者
4. 或者在 VideoConf 界面点「邀请」复制链接

## 公网部署（让外网访问）

### 方法 A：ngrok（最简单）
```bash
# 安装 ngrok: https://ngrok.com/download
ngrok http 8000
# 得到 https://xxxx.ngrok.io 的公网地址
```

### 方法 B：部署到服务器
```bash
# 在有公网 IP 的服务器上运行
python server.py
# 用 nginx 做反向代理（需要 WSS 支持）
```

## 技术架构

```
浏览器A  ←─WebRTC P2P─→  浏览器B
   │                          │
   └──WebSocket──┐  ┌──WebSocket──┘
                 ▼  ▼
            FastAPI Server
            (信令 + 房间管理)
                 │
            Anthropic API
            (会议纪要生成)
```

## 项目结构

```
videoconf/
├── server.py          # FastAPI 服务器（信令 + 房间管理）
├── requirements.txt   # Python 依赖
├── README.md
└── static/
    └── index.html     # 前端（WebRTC + UI）
```
