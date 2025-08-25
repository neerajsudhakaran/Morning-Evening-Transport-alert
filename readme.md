# Wellington Bus & Weather Telegram Alerts using AWS Lambda

This project provides **automated Telegram alerts** for bus arrivals and daily weather in Wellington, NZ, using **AWS Lambda**. There are two types of alerts:

1. **Morning Alert (7:30 AM NZST)** – Provides both **bus schedule** and **daily weather forecast**.  
2. **Evening Alert (15 min prior to shift end)** – Provides **bus schedule only** (no weather) for convenient planning after work.

---

## Motivation

- During **weekdays**, buses run frequently every 15–30 minutes. Alerts are **helpful but not critical**.  
- During **weekends (Saturday & Sunday)**, buses run less frequently (every 45–60 minutes), and some may be **cancelled or arrive early**, which can be confusing.  
- The evening alert is designed to **notify 15 minutes prior to the end of the work shift**, helping avoid missing buses, especially when some arrive **5–10 minutes early**.

---

## Features

- **Morning Alert**  
  - Fetches top 4 buses (services 4 or 38x) for stop 6224  
  - Shows **aimed arrival times** and **delay**  
  - Fetches **current weather** and **next 24-hour forecast**  
  - Sends a **formatted Telegram message**

- **Evening Alert**  
  - Fetches top 4 buses (services 4 or 38x) for stop 6224  
  - Sends **bus arrival times only** to Telegram

---

## Architecture

EventBridge Schedule
│
▼
AWS Lambda
┌──────────────┐
│ Morning API │
│ Bus + Weather│
└──────────────┘
│
▼
Telegram Bot
┌──────────────┐
│ Evening API │
│ Bus Only │
└──────────────┘


---

## Setup

### 1. AWS Lambda

- Runtime: **Python 3.11**
- Timeout: **10–15 seconds**
- Memory: 128 MB

### 2. Environment Variables

Set these in Lambda for **both functions**:

| Variable            | Purpose                           |
|--------------------|----------------------------------|
| `TELEGRAM_TOKEN`     | Telegram Bot API token            |
| `TELEGRAM_CHAT_ID`   | Your chat ID                     |
| `METLINK_API_KEY`    | Metlink API key                  |
| `METEO_API_KEY`      | Meteosource API key (only morning Lambda) |

### 3. EventBridge Schedule

- **Morning alert** → 7:30 AM NZST  

cron(30 19 * * ? *)

- **Evening alert** → 15 minutes prior to end of work shift (adjust hours)  

> Remember: NZST = UTC +12 hours.

---

## Usage

1. Deploy each Lambda function separately.
2. Add the environment variables in the Lambda console.
3. Configure EventBridge rules for the appropriate time.
4. Ensure Telegram bot is created and your chat ID is correct.

---

## Example Morning Alert Output
    ![Alert](Image1.jpg)
