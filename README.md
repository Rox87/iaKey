# ⚡ iaKey (AI Hotkeys App)

<p align="center">
  <img src="assets/icon.jpg" alt="iaKey Logo" width="140" style="border-radius: 16px;"/>
</p>

<p align="center">
  <b>Integre qualquer modelo de IA diretamente ao seu fluxo de trabalho no Windows através de atalhos globais de teclado e um dashboard visual moderno.</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white" alt="Python Version"/>
  <img src="https://img.shields.io/badge/FastAPI-0.136%2B-009688?logo=fastapi&logoColor=white" alt="FastAPI"/>
  <img src="https://img.shields.io/badge/Plataforma-Windows-0078D6?logo=windows&logoColor=white" alt="Windows"/>
  <img src="https://img.shields.io/badge/Security-OS%20Keyring-success" alt="Keyring"/>
  <img src="https://img.shields.io/badge/Licen%C3%A7a-MIT-green" alt="MIT License"/>
</p>

---

## 📖 Visão Geral

O **iaKey** é uma aplicação desktop leve desenvolvida em **Python + FastAPI** projetada para eliminar o atrito de alternar entre janelas para consultar inteligências artificiais.

Basta selecionar qualquer texto em qualquer aplicativo (editores de código, navegadores, processadores de texto, mensageiros), pressionar um atalho configurado no teclado (ex: `Alt+T`, `Ctrl+Shift+R`) e o **iaKey** substitui o texto selecionado instantaneamente pela resposta gerada pela IA.

Além da automação de teclado, a aplicação fornece um **Dashboard Web** completo (inspirado na estética do *Raycast*) para gerenciar visualmente seus provedores, modelos e combinações de teclas.

---

## 💡 Como Funciona na Prática

```
[Qualquer App] -> Seleciona texto -> Pressiona Atalho (ex: Alt+T)
      │
      ▼ (Ctrl+X automático)
[iaKey Backend] -> Captura texto do Clipboard
      │
      ▼
[API de IA] -> Envia prompt customizado + texto via OpenAI SDK
      │
      ▼
[iaKey Backend] -> Recebe a resposta gerada
      │
      ▼ (Ctrl+V automático)
[Qualquer App] -> Texto substituído instantaneamente com o resultado!
```

---

## ✨ Principais Funcionalidades

- **⌨️ Atalhos Globais Totalmente Customizáveis:**
  - Crie e edite combinações de teclas à vontade (ex: `Alt+T`, `Ctrl+Shift+E`, `F4`).
  - Gravador visual interativo no dashboard para capturar teclas sem precisar digitar nomes manualmente.
  - Atribua instruções e prefixos específicos para cada atalho (ex: *Melhorar escrita*, *Explicar código*, *Traduzir para Inglês*, *Gerar testes*).

- **🤖 Suporte Amplo a Provedores (API OpenAI Compatible):**
  - Conecte-se a múltiplos provedores simultaneamente: **OpenAI**, **OpenRouter**, **Groq**, **DeepSeek**, **Google Gemini** (via gateway OpenAI) ou **Ollama / LLMs Locais** (`http://localhost:11434/v1`).
  - Cadastre e associe múltiplos modelos para cada provedor.

- **🔒 Segurança com Credenciais do Sistema Operacional (Keyring):**
  - Suas chaves de API **não** ficam expostas em arquivos de texto plano ou no banco de dados SQLite.
  - Armazenamento nativo seguro utilizando o **Gerenciador de Credenciais do Windows** através da biblioteca `keyring`.

- **🎨 Dashboard Web Moderno:**
  - Interface local servida automaticamente em `http://127.0.0.1:8002`.
  - Design intuitivo com múltiplos temas (Raycast Dark, Clean Light, etc.), gerenciamento de cartões e feedback visual.

- **⚡ Performance e Execução Não Bloqueante:**
  - Requisições para os modelos de IA executadas em threads assíncronas em segundo plano, mantendo o sistema responsivo e sem congelamentos.
  - Opção de execução silenciosa em segundo plano no Windows via `run.vbs`.

---

## 📁 Estrutura do Projeto

```text
iaKey2/
├── assets/                  # Ícones e recursos visuais da aplicação
│   └── icon.jpg
├── backend/                 # Núcleo da aplicação (FastAPI & Automação)
│   ├── db.py                # Inicialização do SQLite e gerenciamento de chaves (Keyring)
│   ├── hotkeys.py           # Listener global de teclado e integração com OpenAI SDK
│   ├── main.py              # API RESTful FastAPI e servidor de arquivos estáticos
│   └── test_api.py          # Testes automatizados da API
├── frontend/                # Dashboard Web da aplicação
│   └── index.html           # Interface visual (SPA responsiva com suporte a temas)
├── pyproject.toml           # Gerenciamento de dependências e scripts do Poetry
├── requirements.txt         # Dependências para instalação alternativa via pip
├── run.py                   # Ponto de entrada principal da aplicação (Uvicorn runner)
├── run.vbs                  # Inicializador silencioso em segundo plano para Windows
└── README.md                # Esta documentação
```

---

## 🚀 Instalação e Inicialização

### Pré-requisitos
- **Windows 10/11**
- **Python 3.10 ou superior**
- [Poetry](https://python-poetry.org/) (recomendado) ou gerenciador de pacotes `pip`

---

### Opção 1: Usando Poetry (Recomendado)

1. Clone o repositório ou navegue até a pasta do projeto:
   ```bash
   cd c:\Users\rodri\gitSpace\iaKey2
   ```

2. Instale as dependências:
   ```bash
   poetry install
   ```

3. Inicie a aplicação:
   ```bash
   poetry run start
   ```
   *(ou `poetry run python run.py`)*

---

### Opção 2: Usando venv e pip

1. Crie e ative um ambiente virtual:
   ```bash
   python -m venv .venv
   .\.venv\Scripts\activate
   ```

2. Instale os pacotes necessários:
   ```bash
   pip install -r requirements.txt
   ```

3. Execute o servidor:
   ```bash
   python run.py
   ```

---

## 🖥️ Acessando o Dashboard

Com a aplicação em execução, acesse em seu navegador:
👉 **[http://127.0.0.1:8002](http://127.0.0.1:8002)**

No painel você pode:
1. **Adicionar Provedores**: Nome da API, URL Base (ex: `https://api.openai.com/v1`, `https://openrouter.ai/api/v1` ou `http://localhost:11434/v1`) e sua respectiva API Key.
2. **Cadastrar Modelos**: Vincular nomes de modelos (ex: `gpt-4o`, `deepseek-chat`, `claude-3-5-sonnet`) a um dos provedores cadastrados.
3. **Criar e Gravar Atalhos**: Escolher o modelo desejado, definir o prefixo do prompt (ex: *"Resuma o texto a seguir em tópicos claros:"*) e pressionar a combinação de teclas para gravar.

---

## 🔕 Executando em Segundo Plano no Windows

Se você deseja que o **iaKey** fique ativo continuamente sem manter uma janela de terminal aberta:

1. Dê um duplo-clique no arquivo **`run.vbs`** ou adicione um atalho para ele na pasta de inicialização do Windows (`shell:startup`).
2. O script iniciará o servidor silenciosamente em segundo plano.
3. Para encerrar o processo em segundo plano quando necessário, localize os processos do Python no Gerenciador de Tarefas ou execute via PowerShell:
   ```powershell
   Get-Process python | Where-Object { (Get-CimInstance Win32_Process -Filter "ProcessId = $($_.Id)").CommandLine -match "run.py" } | Stop-Process -Force
   ```

---

## ⚙️ Variáveis de Ambiente (Opcional)

A aplicação aceita as seguintes variáveis de ambiente opcionais:

| Variável | Padrão | Descrição |
| :--- | :--- | :--- |
| `HOST` | `127.0.0.1` | Endereço de host para o servidor web |
| `PORT` | `8002` | Porta onde o dashboard e a API serão expostos |
| `SERVE_FRONTEND` | `true` | Se deve servir os arquivos estáticos do frontend em `/` |

---

## 🧪 Testes

Para executar a suíte de testes unitários da API:

```bash
poetry run pytest backend/test_api.py
```

---

## 🛡️ Licença

Distribuído sob a licença **MIT**. Veja o arquivo `LICENSE` para mais informações.