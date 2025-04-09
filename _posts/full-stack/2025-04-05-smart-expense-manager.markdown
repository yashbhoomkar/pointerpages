---
layout: single
title: "Smart Expense Manager & Auto-Budgeting with Telegram + Gemini AI"
date: 2025-04-05 11:00:00 +0530
categories:
  - full-stack
tags:
  - telegram bot
  - gemini
  - razorpay
  - mongodb
  - ai
  - expense tracker
  - budgeting
toc: true
classes: wide
---

## 💡 Introduction

Managing expenses shouldn't feel like managing a spreadsheet. That’s what drove the idea behind **Smart Expense Manager** — a conversational, intelligent solution for personal and group expense tracking, built using **Telegram**, **Razorpay**, **Gemini AI**, and **MongoDB**.

This was my hackathon entry for a software innovation challenge — and it turned into one of the most fun and useful projects I’ve built!

---

## 🧠 What the System Does

> “A Telegram message is all it takes.”

This smart assistant helps users:
- 📥 Log expenses easily from chat
- 👥 Split bills with friends in real-time
- 🤖 Use AI to suggest budgets based on spending
- 💰 Handle payments using Razorpay
- 📲 Get alerts via SMS

All without ever leaving Telegram.

---

## 🛠️ Tech Stack

| Layer       | Stack Used           |
|-------------|----------------------|
| **Frontend**| Telegram Chat UI     |
| **Backend** | Node.js + Express    |
| **AI Engine** | Gemini API (Google AI) |
| **Database**| MongoDB Atlas        |
| **Payments**| Razorpay Integration |
| **Alerts**  | Twilio SMS / Bot API |

---

## 🧱 Architecture Overview

1. **User sends command** to the Telegram bot (`/add`, `/split`, `/view`, `/suggest`)
2. Bot sends the data to a **Node.js backend**
3. User data, group metadata, and expenses are stored in **MongoDB**
4. The backend:
   - Calls **Gemini AI** to suggest budgeting categories and limits
   - Calls **Razorpay API** to send payment links if needed
   - Sends SMS reminders through **Twilio**
5. User receives updates — budget suggestions, summaries, and payment confirmations — right inside Telegram.

---

## 🔑 Core Features

### 1. 💬 Telegram Chat Bot
- Intuitive chat-first interface
- Expense logging in seconds
- Group creation & invite support

### 2. 🧠 Gemini AI Budgeting
- Analyzes past spend data
- Recommends monthly budget goals
- Adjusts based on user behavior

### 3. 💸 Razorpay Integration
- Real-time request-to-pay system
- Pings payer via UPI or card
- Confirms payments instantly

### 4. 📢 Alerts via SMS / Telegram
- Sends transaction confirmations
- Daily/weekly budget digests
- Payment reminders

---

## 💼 Example Use Case

You enter:
```bash
/split Pizza 1200 with @rohan @raj @meena
➡️ The bot:

Splits amount ₹300 each

Sends Razorpay requests

Logs in MongoDB

Notifies users via SMS + bot chat

🔮 Future Scope
Voice-command support with Google Assistant

Expense forecasting with ML

Integration with bank APIs

Android/iOS native companion app

Leaderboard of budgeters (gamification)

✨ Takeaway
This project helped me explore:

Real-time APIs and integrations

Multi-user concurrency via chatbots

Payment flow design

AI-in-the-loop systems

It’s the perfect example of how GenAI + full-stack dev can create delightful user experiences — that are actually useful.

