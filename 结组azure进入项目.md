要通过 **Azure OpenAI 的 REST API 或 SDK（Python/JavaScript）接入项目**，你可以遵循以下步骤。我会以 **GPT 模型调用为例**，帮你快速入门。

----------

## 🧩 一、你需要准备的内容

1.  ✅ 一个已开通的 **Azure OpenAI 资源**
    
2.  ✅ 拿到以下关键信息（可以在 Azure 门户中找到）：
    
    -   `资源名称`（如：`my-openai-resource`）
        
    -   `部署名称`（如：`gpt-35-turbo`，自己创建的）
        
    -   `API Key`
        
    -   `API 终结点`（形如：`https://your-resource-name.openai.azure.com/`）
        
    -   `API 版本号`（常用如 `2023-05-15`）
        

----------

## 🐍 二、使用 Python SDK（推荐）

### 1. 安装依赖

bash

复制编辑

`pip install openai`

### 2. 示例代码（ChatGPT 模式）

python

复制编辑

`import openai  openai.api_type = "azure" openai.api_base = "https://<你的资源名称>.openai.azure.com/" openai.api_version = "2023-05-15" openai.api_key = "<你的API密钥>"  response = openai.ChatCompletion.create(     engine="gpt-35-turbo",  # 这是你在 Azure 上部署的模型名称     messages=[         {"role": "system", "content": "You are an assistant that answers politely."},         {"role": "user", "content": "How does voice recognition work?"}     ] )  print(response['choices'][0]['message']['content'])`

----------

## 💻 三、使用 JavaScript / Node.js

### 1. 安装 SDK

bash

复制编辑

`npm install openai`

### 2. 示例代码

js

复制编辑

`const { OpenAIClient, AzureKeyCredential } = require("@azure/openai");  const endpoint = "https://<你的资源名称>.openai.azure.com/"; const apiKey = "<你的API密钥>"; const deploymentName = "gpt-35-turbo";  // 你的部署名  const client = new OpenAIClient(endpoint, new AzureKeyCredential(apiKey));  async function chat() {     const response = await client.getChatCompletions(deploymentName, [         { role: "system", content: "You're a helpful assistant." },         { role: "user", content: "Explain how speech-to-text works." }     ]);     console.log(response.choices[0].message.content); }  chat();`

----------

## 🌐 四、使用纯 REST API（不通过 SDK）

### 请求示例（`curl`）

bash

复制编辑

`curl https://<你的资源名称>.openai.azure.com/openai/deployments/gpt-35-turbo/chat/completions?api-version=2023-05-15 \   -H "Content-Type: application/json" \   -H "api-key: <你的API密钥>" \   -d '{     "messages": [{"role": "user", "content": "Tell me a joke"}],     "temperature": 0.7   }'`

----------

## 🧠 温馨提示：

-   每个模型部署后都有一个唯一的 `部署名称`，你**不能直接用 gpt-4**，而是要根据你在 Azure 门户中创建时起的名字使用。
    
-   如果你还**没部署模型实例**，请先在 Azure OpenAI 中点击“部署模型”。
    

----------

## ✅ 想法延伸（语音识别后接 GPT 分析）：

你可以使用 **Azure Speech to Text** 获得用户语音 → 文本，再用 Azure OpenAI 接入分析命令、生成回复等。

如果你希望我帮你写一个完整的 **语音识别 + GPT 结合处理的项目代码模板**，可以告诉我你用什么语言/平台，我可以一步步给出。是否需要这部分？


> Written with [StackEdit中文版](https://stackedit.cn/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNTQwMDU1NzddfQ==
-->