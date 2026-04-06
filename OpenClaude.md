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

#### .bat

```
@echo off
set OLLAMA_HOST=127.0.0.1:11435
start /b ollama serve > nul 2>&1
timeout /t 2 /nobreak >nul
cd /d D:\Specific_Software\OpenClaude
set CLAUDE_CODE_USE_OPENAI=1
set OPENAI_BASE_URL=http://localhost:11435/v1
set OPENAI_MODEL=gemma4:e4b
set OPENAI_API_KEY=ollama
bun run dev
pause
```

