**MVP Bot Consultant — Technical Specification for AI‑Agent (Codex) Implementation**

_Version 0.3 — 11 June 2025_

---

### 0. Reading Guide for Codex

- Treat every "MUST" as _mandatory_.
    
- All code examples are **reference**, not verbatim; maintain naming and structure.
    
- Use [`asyncio`](https://docs.python.org/3/library/asyncio.html) across the project; avoid synchronous I/O.
    

### 1. Purpose

Build a Telegram bot that consults small‑business owners on the basis of a single Google Sheet with financial data. The bot reads the sheet, sends the data to the OpenAI **Agents API** (model `gpt‑4o-mini`), and returns results to the user. **When the Agent proposes changes, the bot overwrites the original sheet with the modified data returned by the Agent.**

### 2. High‑Level Flow

```
┌────────┐   /start, /menu, /purchase_proposal, /custom_query
│Telegram├──────────────▶ BotCore
└────────┘                │
                          ▼
                     DialogManager ─┐
                                    ▼
                              AgentClient ──▶ OpenAI Agents API
                                    ▲
                                    │
                              SheetsAdapter ──▶ Google Sheets API (read/write)
```

### 3. Runtime Constraints

|Item|Value|
|---|---|
|Python version|3.12|
|Avg response time|≤ 2 s (sheet ≤ 500 rows)|
|Max concurrent users|150 (Telegram long‑polling)|
|Max tokens per agent call|32 000|
|Unit‑test coverage|≥ 60 %|

### 4. Deliverables

```
repo/
├── bot/                # application package
│   ├── __init__.py
│   ├── main.py         # entrypoint
│   ├── config.py       # pydantic‑based settings
│   ├── bot_core.py     # Telegram router
│   ├── dialog_manager.py
│   ├── agent_client.py
│   ├── sheets_adapter.py
│   ├── keyboards.py    # inline/reply markup
│   └── prompts.py      # system + user templates
├── tests/
│   ├── unit/
│   └── integration/
├── Dockerfile
├── docker‑compose.yml
├── pyproject.toml
└── README.md
```

### 5. Key Modules & Interfaces

```python
# config.py
class Settings(BaseSettings):
    google_sa_json: Path = Field(..., env="GOOGLE_SERVICE_ACCOUNT_JSON")
    sheet_id: str = Field(..., env="SHEET_ID")
    openai_api_key: SecretStr = Field(..., env="OPENAI_API_KEY")
    telegram_token: SecretStr = Field(..., env="BOT_TOKEN")

settings = Settings()
```

```python
# sheets_adapter.py
async def read_sheet(range_: str = "A1:Z1000") -> list[list[str]]: ...
async def overwrite_sheet(values: list[list[str]], sheet_name: str = "Sheet1") -> None: ...
async def write_output(values: list[list[str]], sheet_name: str = "AI_Output") -> None: ...
```

```python
# agent_client.py
AGENT_MODEL = "gpt-4o-mini"
SYSTEM_PROMPT = (
    "You are a financial assistant. Answer in Russian.\n"
    "The sheet data follows as JSON: {sheet_json}."
)

async def ask_agent(user_prompt: str, sheet_json: dict) -> str: ...
```

```python
# dialog_manager.py
COMMANDS = {
    "/start": handle_start,
    "/menu": handle_menu,
    "/purchase_proposal": handle_purchase,
    "/custom_query": handle_custom,
}
```

### 6. Prompts & Agent Contract

### 7. Testing Checklist Testing Checklist

1. Unit: Mock Google API and OpenAI.
    
2. Integration: Launch via `docker-compose`; send scripted Telegram updates.
    
3. Load: 100 sequential `/purchase_proposal` on 500‑row sheet ⟶ ≤ 200 s total.
    

### 8. CI/CD

- **Lint**: `ruff check . && black --check .`.
    
- **Test**: `pytest -q`.
    
- **Compose build & up** on `main` branch push.
    

### 9. Environment Variables (.env)

```
GOOGLE_SERVICE_ACCOUNT_JSON=sa.json
SHEET_ID=1AbcD...
OPENAI_API_KEY=sk‑...
BOT_TOKEN=123456:ABC‑DEF...
```

### 10. Acceptance Criteria

- Running `python -m bot.main` launches polling bot.
    
- `/purchase_proposal` creates/overwrites `AI_Output` and replies with a 3‑paragraph summary.
    
- **Editing flow**: when the Agent returns `{ "modified_sheet": ... }`, the bot overwrites the target worksheet with the received data and confirms the update in chat.
    
- Graceful fallback on external API errors (HTTP 4xx/5xx) with a single retry cycle.
    

---

_End of specification_

https://chatgpt.com/codex/tasks/task_e_684942ec5f388333b011cdaf72702d2c