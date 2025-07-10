# 小智服务端文档中心

欢迎使用小智服务端！这里汇集了所有相关文档，帮助您快速了解和集成小智的各项功能。

## 📚 文档导航

### 🚀 快速开始
- [API快速参考](./API快速参考.md) - 快速查阅所有接口和参数
- [WebSocket使用示例](./WebSocket使用示例.md) - 完整的代码示例和最佳实践

### 📖 详细文档
- [API接口文档](./API接口文档.md) - 完整的API接口说明文档
- [客户端消息处理流程详细分析](./客户端消息处理流程详细分析.md) - 深入理解消息处理机制
- [系统架构和流程图说明](./系统架构和流程图说明.md) - 系统整体架构设计
- [项目工作流程详细分析](./项目工作流程详细分析.md) - 开发和部署流程

### 🛠️ 部署指南
- [Centos_Guide.md](./Centos_Guide.md) - CentOS系统部署指南

---

## 🌟 核心功能

### 1. 智能对话 🤖
- **实时语音交互**: 支持语音识别(ASR)和语音合成(TTS)
- **智能文本对话**: 基于大语言模型的自然对话
- **多模态交互**: 支持文本、语音、图像多种输入

### 2. 视觉分析 👁️
- **图像识别**: 智能识别图像内容
- **图像问答**: 基于图像内容的智能问答
- **多格式支持**: JPEG、PNG、GIF、BMP、WEBP

### 3. 设备管理 📱
- **OTA升级**: 设备固件在线升级
- **IoT控制**: 智能设备控制和状态监控
- **设备认证**: 安全的设备身份验证

### 4. 扩展功能 🔧
- **MCP集成**: 多协议通信和功能扩展
- **配置管理**: 灵活的系统配置
- **模块化架构**: 可插拔的服务提供者

---

## 🚀 快速开始

### 1. 服务启动
```bash
# 启动服务
go run src/main.go

# 或使用编译后的二进制文件
./xiaozhi-server-go
```

### 2. 基础测试
```bash
# 测试HTTP API
curl "http://localhost:8080/api/ota/"

# 查看Swagger文档
http://localhost:8080/swagger/index.html
```

### 3. WebSocket连接
```javascript
const ws = new WebSocket('ws://localhost:8080/ws');
ws.onopen = () => {
    ws.send(JSON.stringify({
        type: 'hello',
        version: 1,
        audio_params: {
            format: 'pcm',
            sample_rate: 16000,
            channels: 1,
            frame_duration: 20
        }
    }));
};
```

---

## 📋 API概览

### HTTP REST API
| 服务 | 端点 | 描述 |
|------|------|------|
| OTA升级 | `/api/ota/` | 固件版本检查和下载 |
| 视觉分析 | `/api/vision` | 图像识别和分析 |
| 配置管理 | `/api/cfg` | 系统配置管理 |
| 文件下载 | `/api/ota_bin/{filename}` | 固件文件下载 |

### WebSocket API
| 消息类型 | 方向 | 描述 |
|----------|------|------|
| `hello` | 双向 | 连接握手和参数协商 |
| `chat` | 客户端→服务端 | 文本聊天消息 |
| `listen` | 客户端→服务端 | 语音控制消息 |
| `image` | 客户端→服务端 | 图像处理消息 |
| `stt` | 服务端→客户端 | 语音识别结果 |
| `tts` | 服务端→客户端 | 语音合成状态 |
| `llm` | 服务端→客户端 | 大语言模型回复 |

---

## 🛠️ 开发指南

### 技术栈
- **后端**: Go 1.19+, Gin框架, WebSocket
- **数据库**: SQLite/MySQL/PostgreSQL (GORM)
- **AI服务**: 支持多种LLM、ASR、TTS提供者
- **音频处理**: PCM、Opus格式支持
- **图像处理**: 多格式图像解析和分析

### 项目结构
```
src/
├── main.go              # 程序入口
├── configs/             # 配置管理
├── core/                # 核心功能
│   ├── connection*      # WebSocket连接处理
│   ├── providers/       # AI服务提供者
│   ├── utils/          # 工具函数
│   └── ...
├── models/             # 数据模型
├── ota/                # OTA升级服务
├── vision/             # 视觉分析服务
└── docs/               # API文档
```

### 配置文件
主要配置文件为 `config.yaml`，包含：
- 服务器配置 (端口、认证等)
- AI服务配置 (LLM、ASR、TTS等)
- 日志配置
- 数据库配置

---

## 🔧 集成示例

### 1. JavaScript/TypeScript
```javascript
import { XiaoZhiWebSocketClient } from './xiaozhi-client';

const client = new XiaoZhiWebSocketClient('ws://localhost:8080/ws');
await client.connect();

// 发送聊天消息
await client.sendMessage({
    type: 'chat',
    text: '你好，小智！'
});
```

### 2. Python
```python
import asyncio
import websockets
import json

async def chat_with_xiaozhi():
    uri = "ws://localhost:8080/ws"
    async with websockets.connect(uri) as websocket:
        # 发送Hello消息
        hello = {
            "type": "hello",
            "version": 1,
            "audio_params": {
                "format": "pcm",
                "sample_rate": 16000,
                "channels": 1,
                "frame_duration": 20
            }
        }
        await websocket.send(json.dumps(hello))
        
        # 发送聊天消息
        chat_msg = {
            "type": "chat",
            "text": "你好，小智！"
        }
        await websocket.send(json.dumps(chat_msg))

asyncio.run(chat_with_xiaozhi())
```

### 3. React组件
```jsx
import React, { useState, useEffect } from 'react';
import { XiaoZhiClient } from './xiaozhi-client';

function ChatComponent() {
    const [client, setClient] = useState(null);
    const [messages, setMessages] = useState([]);
    
    useEffect(() => {
        const xiaozhi = new XiaoZhiClient();
        xiaozhi.onMessage = (msg) => {
            if (msg.type === 'llm') {
                setMessages(prev => [...prev, msg.text]);
            }
        };
        xiaozhi.connect();
        setClient(xiaozhi);
        
        return () => xiaozhi.disconnect();
    }, []);
    
    const sendMessage = (text) => {
        client?.sendMessage({ type: 'chat', text });
    };
    
    return (
        <div>
            {/* 聊天界面 */}
        </div>
    );
}
```

---

## 🔐 安全说明

### 认证机制
- **Token认证**: Bearer Token方式
- **设备ID验证**: 设备唯一标识验证
- **权限控制**: 基于Token的权限管理

### 安全建议
1. 在生产环境中使用HTTPS和WSS
2. 定期轮换Token
3. 限制文件上传大小和格式
4. 实现请求频率限制
5. 记录和监控异常访问

---

## 📊 性能优化

### 连接管理
- WebSocket连接池
- 自动重连机制
- 心跳检测

### 音频处理
- 流式音频传输
- 音频格式优化
- 缓冲区管理

### 图像处理
- 图像压缩
- 异步处理
- 缓存机制

---

## 🐛 故障排除

### 常见问题

**1. WebSocket连接失败**
- 检查服务器是否启动
- 确认端口是否正确
- 检查防火墙设置

**2. 音频无法播放**
- 确认浏览器音频权限
- 检查音频格式支持
- 验证音频编解码器

**3. 图像分析失败**
- 确认图像格式和大小
- 检查Token和设备ID
- 验证网络连接

**4. 认证失败**
- 确认Token格式正确
- 检查设备ID匹配
- 验证Token是否过期

### 日志查看
```bash
# 查看服务日志
tail -f logs/server.log

# 查看错误日志
grep "ERROR" logs/server.log
```

---

## 🤝 贡献指南

### 开发环境设置
1. 安装Go 1.19+
2. 克隆项目仓库
3. 安装依赖: `go mod tidy`
4. 运行测试: `go test ./...`
5. 启动服务: `go run src/main.go`

### 代码规范
- 遵循Go代码规范
- 编写单元测试
- 更新相关文档
- 提交前运行`go fmt`

### 提交流程
1. Fork项目
2. 创建特性分支
3. 提交更改
4. 创建Pull Request

---

## 📞 支持与联系

### 文档反馈
如果您在使用过程中遇到问题或有改进建议，欢迎：
- 提交Issue
- 贡献代码
- 完善文档

### 技术支持
- 查看[故障排除](#故障排除)部分
- 参考[API文档](./API接口文档.md)
- 查看[使用示例](./WebSocket使用示例.md)

---

## 📄 许可证

本项目采用[MIT许可证](../LICENSE)，您可以自由使用、修改和分发。

---

*最后更新: 2024-01-01*  
*版本: v1.0*  
*维护者: 小智开发团队*