---
id: w8tmwls08x0wsgwl37rs6ct
title: Gemini
desc: ''
updated: 1782615119297
created: 1782220891970
---

## usage

* 网页使用
  * 网页提问 <https://gemini.google.com/app>
  * ai studio:可享会员额外 <https://aistudio.google.com/prompts/new_chat>
* Antigravity 客户端 (超集)
* Antigravity IDE (子集)
* gemini API
  * ai studio 创建

## 工具

* Antigravity Manager （开启后可以帮助 Antigravity 客户端登录通过）
  * 可以进行账号管理 + 本地代理
* Antigravity tools （垃圾不可用）
  * 账号管理 + 本地代理

## gemini API

### 查询API支持模型

```bash
curl https://generativelanguage.googleapis.com/v1/models?key=<your_key>
```

```bash
{
  "models": [
    {
      "name": "models/gemini-2.5-flash",
      "version": "001",
      "displayName": "Gemini 2.5 Flash",
      "description": "Stable version of Gemini 2.5 Flash, our mid-size multimodal model that supports up to 1 million tokens, released in June of 2025.",
      "inputTokenLimit": 1048576,
      "outputTokenLimit": 65536,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "createCachedContent",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 64,
      "maxTemperature": 2,
      "thinking": true
    },
    {
      "name": "models/gemini-2.5-pro",
      "version": "2.5",
      "displayName": "Gemini 2.5 Pro",
      "description": "Stable release (June 17th, 2025) of Gemini 2.5 Pro",
      "inputTokenLimit": 1048576,
      "outputTokenLimit": 65536,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "createCachedContent",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 64,
      "maxTemperature": 2,
      "thinking": true
    },
    {
      "name": "models/gemini-2.0-flash",
      "version": "2.0",
      "displayName": "Gemini 2.0 Flash",
      "description": "Gemini 2.0 Flash",
      "inputTokenLimit": 1048576,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "createCachedContent",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 40,
      "maxTemperature": 2
    },
    {
      "name": "models/gemini-2.0-flash-001",
      "version": "2.0",
      "displayName": "Gemini 2.0 Flash 001",
      "description": "Stable version of Gemini 2.0 Flash, our fast and versatile multimodal model for scaling across diverse tasks, released in January of 2025.",
      "inputTokenLimit": 1048576,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "createCachedContent",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 40,
      "maxTemperature": 2
    },
    {
      "name": "models/gemini-2.0-flash-lite-001",
      "version": "2.0",
      "displayName": "Gemini 2.0 Flash-Lite 001",
      "description": "Stable version of Gemini 2.0 Flash-Lite",
      "inputTokenLimit": 1048576,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "createCachedContent",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 40,
      "maxTemperature": 2
    },
    {
      "name": "models/gemini-2.0-flash-lite",
      "version": "2.0",
      "displayName": "Gemini 2.0 Flash-Lite",
      "description": "Gemini 2.0 Flash-Lite",
      "inputTokenLimit": 1048576,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "createCachedContent",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 40,
      "maxTemperature": 2
    },
    {
      "name": "models/gemma-4-26b-a4b-it",
      "version": "001",
      "displayName": "Gemma 4 26B A4B IT",
      "description": "Gemma 4 26B A4B IT",
      "inputTokenLimit": 262144,
      "outputTokenLimit": 32768,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 64,
      "maxTemperature": 2,
      "thinking": true
    },
    {
      "name": "models/gemma-4-31b-it",
      "version": "001",
      "displayName": "Gemma 4 31B IT",
      "description": "Gemma 4 31B IT",
      "inputTokenLimit": 262144,
      "outputTokenLimit": 32768,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 64,
      "maxTemperature": 2,
      "thinking": true
    },
    {
      "name": "models/gemini-2.5-flash-lite",
      "version": "001",
      "displayName": "Gemini 2.5 Flash-Lite",
      "description": "Stable version of Gemini 2.5 Flash-Lite, released in July of 2025",
      "inputTokenLimit": 1048576,
      "outputTokenLimit": 65536,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "createCachedContent",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 64,
      "maxTemperature": 2,
      "thinking": true
    },
    {
      "name": "models/gemini-2.5-flash-image",
      "version": "2.0",
      "displayName": "Nano Banana",
      "description": "Gemini 2.5 Flash Preview Image",
      "inputTokenLimit": 32768,
      "outputTokenLimit": 32768,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 64,
      "maxTemperature": 1
    },
    {
      "name": "models/gemini-3.1-flash-lite",
      "version": "3.1-flash-lite-05-2026",
      "displayName": "Gemini 3.1 Flash Lite",
      "description": "Gemini 3.1 Flash Lite",
      "inputTokenLimit": 1048576,
      "outputTokenLimit": 65536,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "createCachedContent",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 64,
      "maxTemperature": 2,
      "thinking": true
    },
    {
      "name": "models/gemini-3.1-flash-image",
      "version": "3.0",
      "displayName": "Nano Banana 2",
      "description": "Gemini 3.1 Flash Image.",
      "inputTokenLimit": 65536,
      "outputTokenLimit": 65536,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 64,
      "maxTemperature": 1,
      "thinking": true
    },
    {
      "name": "models/gemini-3.5-flash",
      "version": "3.5-flash-05-2026",
      "displayName": "Gemini 3.5 Flash",
      "description": "Gemini 3.5 Flash",
      "inputTokenLimit": 1048576,
      "outputTokenLimit": 65536,
      "supportedGenerationMethods": [
        "generateContent",
        "countTokens",
        "createCachedContent",
        "batchGenerateContent"
      ],
      "temperature": 1,
      "topP": 0.95,
      "topK": 64,
      "maxTemperature": 2,
      "thinking": true
    },
    {
      "name": "models/gemini-embedding-001",
      "version": "001",
      "displayName": "Gemini Embedding 001",
      "description": "Obtain a distributed representation of a text.",
      "inputTokenLimit": 2048,
      "outputTokenLimit": 1,
      "supportedGenerationMethods": [
        "embedContent",
        "countTextTokens",
        "countTokens",
        "asyncBatchEmbedContent"
      ]
    },
    {
      "name": "models/gemini-embedding-2",
      "version": "2",
      "displayName": "Gemini Embedding 2",
      "description": "Obtain a distributed representation of multimodal content.",
      "inputTokenLimit": 8192,
      "outputTokenLimit": 1,
      "supportedGenerationMethods": [
        "embedContent",
        "countTextTokens",
        "countTokens",
        "asyncBatchEmbedContent"
      ]
    },
    {
      "name": "models/imagen-4.0-generate-001",
      "version": "001",
      "displayName": "Imagen 4",
      "description": "Vertex served Imagen 4.0 model",
      "inputTokenLimit": 480,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "predict"
      ]
    },
    {
      "name": "models/imagen-4.0-ultra-generate-001",
      "version": "001",
      "displayName": "Imagen 4 Ultra",
      "description": "Vertex served Imagen 4.0 ultra model",
      "inputTokenLimit": 480,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "predict"
      ]
    },
    {
      "name": "models/imagen-4.0-fast-generate-001",
      "version": "001",
      "displayName": "Imagen 4 Fast",
      "description": "Vertex served Imagen 4.0 Fast model",
      "inputTokenLimit": 480,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "predict"
      ]
    },
    {
      "name": "models/veo-2.0-generate-001",
      "version": "2.0",
      "displayName": "Veo 2",
      "description": "Vertex served Veo 2 model. Access to this model requires billing to be enabled on the associated Google Cloud Platform account. Please visit https://console.cloud.google.com/billing to enable it.",
      "inputTokenLimit": 480,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "predictLongRunning"
      ]
    },
    {
      "name": "models/veo-3.0-generate-001",
      "version": "3.0",
      "displayName": "Veo 3",
      "description": "Veo 3",
      "inputTokenLimit": 480,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "predictLongRunning"
      ]
    },
    {
      "name": "models/veo-3.0-fast-generate-001",
      "version": "3.0",
      "displayName": "Veo 3 fast",
      "description": "Veo 3 fast",
      "inputTokenLimit": 480,
      "outputTokenLimit": 8192,
      "supportedGenerationMethods": [
        "predictLongRunning"
      ]
    }
  ]
}
```

### 查询API 具体模型信息

```bash
curl https://generativelanguage.googleapis.com/v1/models/<model_name>?key=<your_key>
```

```bash
{
  "name": "models/gemini-3.5-flash",
  "version": "3.5-flash-05-2026",
  "displayName": "Gemini 3.5 Flash",
  "description": "Gemini 3.5 Flash",
  "inputTokenLimit": 1048576,
  "outputTokenLimit": 65536,
  "supportedGenerationMethods": [
    "generateContent",
    "countTokens",
    "createCachedContent",
    "batchGenerateContent"
  ],
  "temperature": 1,
  "topP": 0.95,
  "topK": 64,
  "maxTemperature": 2,
  "thinking": true
}
```

## claude code router 配置gemini api

### 调试记录 1

<https://github.com/musistudio/claude-code-router>

```bash
{
  "APIKEY": "$GEMINI_API_KEY",
  "PROXY_URL": "http://127.0.0.1:7890",
  "LOG": true,
  "API_TIMEOUT_MS": 600000,
  "NON_INTERACTIVE_MODE": false,

  "Providers": [
    {
      "name": "gemini",
      "api_base_url": "https://generativelanguage.googleapis.com/v1beta",
      "api_key": "$GEMINI_API_KEY",
      "models": [
        "gemini-2.5-flash",
        "gemini-2.5-pro"
      ]
    }
  ],

  "Router": {
    "default": "gemini,gemini-2.5-flash",
    "think": "gemini,gemini-2.5-pro",
    "longContext": "gemini,gemini-2.5-pro",
    "longContextThreshold": 60000,
    "webSearch": "gemini,gemini-2.5-flash"
  }
}


C:\Windows\System32>ccr restart
Service was not running or failed to stop.
Starting claude code router service...
✅ Service started successfully in the background.

C:\Windows\System32>ccr start
Loaded JSON config from: C:\Users\XuJiaFei\.claude-code-router\config.json

C:\Windows\System32>ccr code
Service not running, starting service...
Service startup timeout, please manually run `ccr start` to start the service


##ccr官方
"APIKEY": "your-secret-key",
  "PROXY_URL": "http://127.0.0.1:7890",
  "LOG": true,
  "API_TIMEOUT_MS": 600000,
  "NON_INTERACTIVE_MODE": false,
  "Providers": [
{
      "name": "gemini",
      "api_base_url": "https://generativelanguage.googleapis.com/v1beta/models/",
      "api_key": "sk-xxx",
      "models": ["gemini-2.5-flash", "gemini-2.5-pro"],
      "transformer": {
        "use": ["gemini"]
      }
    },
    
    ],
  "Router": {
    "default": "deepseek,deepseek-chat",
    "background": "ollama,qwen2.5-coder:latest",
    "think": "deepseek,deepseek-reasoner",
    "longContext": "openrouter,google/gemini-2.5-pro-preview",
    "longContextThreshold": 60000,
    "webSearch": "gemini,gemini-2.5-flash"
  }
  }
```

ccr能够看到配置的参数信息，但是启动后并没有/model里并没有 gemini选项

```bash

✻ API error · Retrying in 32s · attempt 7/10
  ⎿  Tip: Use /btw to ask a quick side question without interrupting Claude's current work
```

分析路径：拉去ccr日志，使用flash模型分析，找到原因：pro模型限速，切换为flash模型后，可以ccr code进行正常访问

### 调试纪录2- Antigravity tools + claude 2.1.150

* 开启 Antigravity 代理
* 重新安装 Antigravity tool，切忌是不带 LS 后缀的
* 开启代理后，claude无法登录，模型信息是对的，python能够调用，后排查是最新claude对API格式存在要求，必须符合前缀"sk-ant-api03-<32>"
* 跳过登录成功，但API error，一直在重复尝试
* API能够发送，但是400，参看日志报错 错误参数
* 切换claude版本，2.1.150  --调试通过

总结：

1. api key 需符合特定格式
2. claude 版本不能太高，使用特定版本即可

<https://github.com/lbjlaq/Antigravity-Manager/issues/3167>