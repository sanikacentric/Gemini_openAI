Gemini LLM Client (Production-Ready Wrapper)

A lightweight, production-friendly Python wrapper for calling Google Gemini models using the OpenAI-compatible API endpoint.

This project provides:

✅ Environment-based configuration

✅ Retry logic with exponential backoff

✅ Structured logging

✅ Timeout controls

✅ Clean service abstraction for LLM calls

📦 Features

Uses OpenAI SDK pointed to Gemini endpoint

Configurable model (default: gemini-2.5-flash)

Automatic retries on failure

Centralized LLM service wrapper

Production-style logging

Clean separation of config and runtime logic

🏗️ Architecture Overview
Settings (Config)
        ↓
build_client()
        ↓
LLMService.chat()
        ↓
Gemini OpenAI-Compatible Endpoint
🔧 Installation
1️⃣ Install Dependencies
pip install openai
2️⃣ Set Environment Variables
Windows (PowerShell)
setx GEMINI_API_KEY "your_api_key_here"
macOS / Linux
export GEMINI_API_KEY="your_api_key_here"
⚙️ Configuration

The Settings dataclass manages all configuration via environment variables.

Variable	Default	Description
GEMINI_API_KEY	❌ Required	Your Gemini API key
MODEL	gemini-2.5-flash	Model name
TIMEOUT_SECONDS	30	Request timeout
MAX_RETRIES	3	Retry attempts
RETRY_BACKOFF_SECONDS	1.5	Backoff multiplier
LOG_LEVEL	INFO	Logging level
🧠 How It Works
1️⃣ Settings Class
@dataclass(frozen=True)
class Settings:

Centralizes configuration

Pulls values from environment variables

Controls reliability parameters

2️⃣ Client Builder
def build_client(settings: Settings) -> OpenAI:

Validates API key

Points OpenAI SDK to Gemini endpoint:

https://generativelanguage.googleapis.com/v1beta/openai/
3️⃣ LLMService Wrapper
class LLMService:

Provides a single method:

chat(user_text: str, system_text: str)

Features:

Structured retries

Backoff logic

Clean error handling

Returns model text output

🔁 Retry Logic

If a request fails:

Logs a warning

Waits using exponential backoff

Retries up to MAX_RETRIES

Raises a clean error if all retries fail

Example backoff:

Attempt 1 → wait 1.5s
Attempt 2 → wait 3.0s
Attempt 3 → fail
▶️ Example Usage
if __name__ == "__main__":
    settings = Settings()
    llm = LLMService(settings)

    prompt = "Explain to me how AI works in simple terms."
    answer = llm.chat(prompt)

    print("\n--- MODEL ANSWER ---\n")
    print(answer)
🛡️ Production Readiness

This wrapper is suitable for:

Backend APIs

Microservices

Streamlit apps

FastAPI integrations

Internal AI tools

Enterprise systems

Because it includes:

Logging

Retries

Timeout control

Config isolation

Clean abstraction layer

🧩 Extending the Service

You can easily add:

Streaming support

JSON mode

Tool calling

Structured outputs

Observability hooks

Metrics (Prometheus, etc.)

Circuit breaker pattern

📌 Why Use This Wrapper?

Instead of calling the model directly everywhere:

❌ Repeated retry logic
❌ Repeated client instantiation
❌ Hardcoded configs
❌ Scattered error handling

You get:

✅ One clean LLM service
✅ Centralized reliability logic
✅ Easy model swapping
✅ Environment-driven config

🧪 Sample Output
--- MODEL ANSWER ---

AI works by learning patterns from data...
🏁 Summary

This project provides a clean, production-grade Gemini client wrapper using the OpenAI-compatible API, making it easy to:

Build reliable AI applications

Scale safely

Maintain clean architecture

Integrate Gemini models into enterprise systems
