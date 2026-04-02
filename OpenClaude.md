#### 下载

```
git clone https://node.gitlawb.com/z6MkqDnb7Siv3Cwj7pGJq4T5EsUisECqR8KpnDLwcaZq5TPr/openclaude.git
cd openclaude
bun install
set CLAUDE_CODE_USE_OPENAI = "1"
set OPENAI_BASE_URL = "http://localhost:11434/v1"
set OPENAI_MODEL = "deepseek-v3.1:cloud"
set OPENAI_API_KEY = "ollama"
bun run dev
```

**注** bun.exe在c盘user下

#### Ollama

```
OllamaSetup.exe /DIR="D:\Ollama"  //安装
//添加环境变量
OLLAMA_MODELS  D:\Ollama\models
ollama serve
ollama pull deepseek-coder:6.7b
ollama pull qwen2.5-coder:7b
ollama pull codegemma
```

