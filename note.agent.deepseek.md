---
id: 3kfukreovxggurh3iawab1r
title: Deepseek
desc: ''
updated: 1782618738726
created: 1782612543287
---

## 创建API KEY 以及充值

<https://platform.deepseek.com>

## 验证

```python
import requests
import json

# 将这里替换为你刚刚创建的 API Key
API_KEY = "sk-44f497b3da974d6ea9aae706f9cb9b93"

url = "https://api.deepseek.com/chat/completions"
headers = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer {API_KEY}"
}
data = {
    "model": "deepseek-chat",
    "messages": [{"role": "user", "content": "你好，这是一条API连通性测试消息。"}]
}

response = requests.post(url, headers=headers, data=json.dumps(data))

if response.status_code == 200:
    print("✅ API Key 验证成功！")
    print("模型回复:", response.json()["choices"][0]["message"]["content"])
else:
    print("❌ 验证失败。状态码:", response.status_code)
    print("错误详情:", response.text)

# PS D:\win_workspace\python\pyclient> & D:/python/python.exe d:/win_workspace/python/pyclient/demo/api.py
# ✅ API Key 验证成功！
# 模型回复: 你好！收到你的API连通性测试消息了。✅
```

## agent 适配

https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/claude_code

https://github.com/deepseek-ai/awesome-deepseek-agent/tree/main

https://github.com/deepseek-ai/awesome-deepseek-agent/blob/main/docs/codex.md

