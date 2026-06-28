# Multi-Tool AI Agent

A production-inspired conversational AI agent built using the ReAct 
(Reasoning + Acting) pattern — the same architecture used in enterprise 
AI systems and production chatbots. The agent accepts natural language 
input, reasons about which tool to use, executes real-time API calls, 
and returns structured, accurate responses — all within a persistent 
memory-enabled conversation loop.

Built to demonstrate practical LLM agent engineering: tool schema design,
multi-API orchestration, error handling, and modular tool registration —
skills directly applicable to AI/ML and backend engineering roles.

---

## Features at a Glance

  14 tools across 7 domains — all driven by a single agent loop.

  ┌──────────────────────┬───────────────────────────────────────────────┐
  │ Domain               │ Tools                                         │
  ├──────────────────────┼───────────────────────────────────────────────┤
  │ Math                 │ calculator                                    │
  │ Health               │ bmi_calculator, age_calculator                │
  │ Academics            │ grade_calculator                              │
  │ Weather              │ get_weather, get_weather_by_coordinates       │
  │ Currency             │ convert_currency, list_currencies,            │
  │                      │ compare_currency                              │
  │ Country Information  │ get_country_info, search_countries_by_region  │
  │ Book Search          │ search_books_by_title, search_books_by_author,│
  │                      │ get_book_by_isbn                              │
  └──────────────────────┴───────────────────────────────────────────────┘

---

## Agent Capabilities

### Math & Utilities
  Handles arithmetic operations with guaranteed accuracy — bypassing
  LLM hallucination on large number computation entirely.
  → "What is 1,847,293 × 6,492?"
  → "Divide 99 by 7"

### Health Calculators
  Computes BMI with WHO classification and exact age in years,
  months, and days from date of birth — using Python datetime,
  not LLM guessing.
  → "My weight is 70kg and height is 175cm"
  → "I was born on 15 March 2004, how old am I?"

### Academic Grade Calculator
  Calculates average percentage and letter grade across multiple
  subjects with remarks, supporting custom max scores.
  → "My scores are 88, 76, 91 and 84 out of 100"

### Live Weather
  Fetches real-time meteorological data for any city worldwide
  or by GPS coordinates. Returns temperature, feels-like,
  humidity, wind speed, and sky conditions.
  → "What is the weather in Chennai?"
  → "Weather at coordinates 11.0168, 76.9558"
  → "Temperature in London in Fahrenheit?"

### Live Currency Exchange
  Converts amounts between 160+ currencies using live forex rates.
  Supports multi-currency comparison in a single query and maps
  natural language currency names to ISO codes automatically.
  → "Convert 500 INR to USD"
  → "Compare USD against INR, EUR, GBP and JPY"
  → "How much is 100 dollars in Japanese yen?"

### Country Information
  Retrieves structured profiles for any country — capital city,
  population, area, official languages, currencies, timezones,
  calling codes, and regional flag. Also lists all countries
  within any world region sorted by name.
  → "Tell me about Japan"
  → "What currency does Switzerland use?"
  → "List all countries in Asia"

### Book Search
  Searches millions of books by title, author, or ISBN using
  the Open Library database. Returns title, author, publisher,
  year, language, subjects, edition count, and description.
  → "Search books named Atomic Habits"
  → "Show me books by APJ Abdul Kalam"
  → "Find book with ISBN 9780735224292"

---

## Architecture

  User Input
      │
      ▼
  Agent Loop  ──→  LLM (OpenRouter / Nemotron 120B)
      │                    │
      │         Decides which tool to call
      │                    │
      ▼                    ▼
  Tool Executor  ──→  Python Function
      │                    │
      │         Calls external API / computes result
      │                    │
      ▼                    ▼
  Result fed back to LLM  ──→  Final natural language response
      │
      ▼
  Memory saved to JSON (persists across sessions)

  Core pattern: ReAct (Reasoning + Acting)
  Max tool loop steps: 5 (prevents infinite loops)
  Memory: JSON file, full conversation history

---

## Project Structure

  multi-tool-ai-agent/
  ├── main.py        ← Agent loop, tool schemas, LLM calls
  ├── tools.py       ← All 14 tool implementations
  ├── prompts.py     ← System prompt and tool usage rules
  ├── memory.py      ← Conversation memory (load/save JSON)
  ├── memory.json    ← Auto-generated conversation history
  └── .env           ← API keys 

---

## Tech Stack

  Language   : Python 3.10+
  LLM        : Nemotron 3 Super 120B via OpenRouter
  Agent Type : ReAct (tool-calling loop)
  APIs Used  :
    • OpenRouter API     — LLM inference         (openrouter.ai)
    • OpenWeatherMap API — Live weather data      (openweathermap.org)
    • ExchangeRate API   — Live forex rates       (exchangerate-api.com)
    • RestCountries API  — Country data           (restcountries.com)  [no key needed]
    • Open Library API   — Book search            (openlibrary.org)    [no key needed]

---

## Setup

  1. Clone the repository
       git clone https://github.com/yourusername/ai-agents.git
       cd ai-agents

  2. Install dependencies
       pip install requests python-dotenv

  3. Create a .env file in the root directory
       OPENROUTER_API_KEY=your_openrouter_key_here
       OPENWEATHER_API_KEY=your_openweather_key_here
       EXCHANGERATE_API_KEY=your_exchangerate_key_here

  4. Run the agent
       python main.py

  API keys for RestCountries and Open Library are NOT required.

---

## Sample Interactions

  You: What is the weather in Coimbatore?
  Agent: It's currently 33°C and partly cloudy in Coimbatore, IN.
         Humidity is 72% with winds at 3.5 m/s. Stay hydrated!

  You: Convert 1000 INR to USD
  Agent: 1000 INR = 11.98 USD
         Exchange Rate: 1 INR = 0.01198 USD (live rate)

  You: Tell me about Germany
  Agent: 🇩🇪 Germany | Capital: Berlin | Population: 83,240,525
         Region: Europe | Languages: German | Currency: Euro (€)

  You: Books by APJ Abdul Kalam
  Agent: Found 5 books by APJ Abdul Kalam:
         [1] Wings of Fire — 1999 | Publisher: Universities Press
         [2] Ignited Minds — 2002 | Publisher: Penguin Books ...

---
