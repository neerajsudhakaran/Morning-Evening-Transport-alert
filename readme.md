# NZ Bus & Weather Telegram Bot

This AWS Lambda project fetches **bus arrival predictions** and **weather updates** for Wellington, NZ, and sends them as a message to a **Telegram bot** every morning at 7:30 AM NZST.

---

## Features

- Fetch **bus arrivals** for stop 6224 (buses 4 and 38x, top 4 departures)
- Fetch **current weather**, **next 24h forecast**, and **weather alerts**
- Send **combined info** via Telegram
- Runs automatically every day at 7:30 AM NZST using **EventBridge cron**

---

## Architecture

EventBridge (7:30 AM NZST)
│
▼
AWS Lambda
┌──────────────┐
│ Bus API │
│ Weather API │
└──────────────┘
│
▼
Telegram Bot


---

## Setup

### 1. AWS Lambda

- Runtime: **Python 3.11**
- Timeout: **10–15 seconds**
- Memory: 128 MB is sufficient

### 2. Environment Variables

Set these in Lambda console:

| Variable            | Value                                |
|--------------------|--------------------------------------|
| `TELEGRAM_TOKEN`     | Telegram Bot API token                |
| `TELEGRAM_CHAT_ID`   | Your chat ID (e.g., `7132393434`)   |
| `METLINK_API_KEY`    | Metlink API key                       |
| `METEO_API_KEY`      | Meteosource API key                   |

### 3. EventBridge Schedule

- Cron expression: `cron(30 19 * * ? *)`  
  - Triggers Lambda at **7:30 AM NZST** daily

---

## Usage

1. Deploy Lambda with the provided Python code.
2. Attach environment variables.
3. Test manually to ensure Telegram message is sent.
4. EventBridge will automatically trigger daily.

---

## Python Standard Library Only

This project uses only the **Python standard library (`urllib`)**—no external dependencies are required.

---

## Telegram Bot Setup

1. Create a bot with [@BotFather](https://t.me/BotFather)
2. Get the **bot token**
3. Send a message to your bot and get your **chat ID**
4. Add token and chat ID as environment variables in Lambda

---

## Example Output

🚌 Bus Schedule (Stop 6224):
Bus 4 at Kilbirnie-A
Arrival: 2025-08-26T06:50:00+12:00
Delay: On time
🌤️ Current Weather in Wellington:
Temperature: 7.8°C
Condition: Partly clear
📅 Forecast for 2025-08-26:
Fog in the morning, sunny later. Temperature 7/12 °C.
High: 12.2°C, Low: 7.0°C

✅ No active weather alerts.
