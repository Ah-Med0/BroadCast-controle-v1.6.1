<div align="center">

# 📢 BroadCast Controle

### **Advanced Multi-Bot Smart Broadcast System for Discord**

[![Version](https://img.shields.io/badge/version-1.6.1-blue.svg?style=for-the-badge)](https://github.com/Ah-Med0/BroadCast-controle-v1.6.1)
[![Discord.js](https://img.shields.io/badge/discord.js-v14-5865F2.svg?style=for-the-badge&logo=discord&logoColor=white)](https://discord.js.org)
[![Node](https://img.shields.io/badge/node-%3E%3D22.0.0-339933.svg?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![License](https://img.shields.io/badge/license-MIT-green.svg?style=for-the-badge)](LICENSE.md)
[![Status](https://img.shields.io/badge/status-Production-brightgreen.svg?style=for-the-badge)]()

---

[![Discord](https://img.shields.io/badge/Developer-Ahmed%20Amine%20(2.7m)-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discord.com/users/580414690472230962)
[![Support Server](https://img.shields.io/badge/Join-Support%20Server-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discord.gg/x2QqPMP9WB)

</div>

---

## Looking for Arabic Version?
[Download it fron here](https://www.mediafire.com/file/6lqjltzaj9spn0y/BroadcastControle_V1.6_Encrypted.zip/file)


---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [How It Works](#-how-it-works)
- [Commands](#-commands)
  - [Bot Management](#-bot-management)
  - [Broadcast](#-broadcast)
  - [General](#-general)
- [Setup Guide](#-setup-guide)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
- [Project Structure](#-project-structure)
- [Safety & Rate Limits](#-safety--rate-limits)
- [FAQ](#-faq)
- [License](#-license)

---

## 📌 Overview

**BroadCast Controle** is a production-grade Discord bot system designed for **secure, large-scale direct messaging** using a **multi-bot architecture**. Instead of relying on a single bot (which hits Discord's rate limits quickly), this system distributes the workload across multiple bot accounts to achieve high throughput while staying within Discord's API limits.

> **⚠️ Important:** This bot is intended for legitimate use cases such as server announcements, event notifications, and community engagement. Always comply with [Discord's Terms of Service](https://discord.com/terms) and [Developer Policy](https://discord.com/developers/docs/policies-and-agreements).

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **🤖 Multi-Bot Engine** | Uses multiple bot tokens to distribute DM broadcasts, bypassing single-bot rate limits |
| **🛡️ Smart Safety** | Automatic rate-limit handling, error streak detection, and safety cooldowns |
| **📊 Live Dashboard** | Real-time broadcast panel with per-bot stats (sent, failed, skipped) |
| **🟢 Online-Only Mode** | Option to broadcast only to members who are currently online (`+obc`) |
| **🛑 Stop Button** | Interactive stop button on the broadcast panel |
| **🔧 Full Token Management** | Add, list, delete, edit, and verify tokens via commands or interactive menus |
| **💬 DM Support** | Commands work via DM even if the bot is banned from the server |
| **🏥 Auto Health Checks** | Periodic bot health monitoring with automatic reconnection |
| **🔌 Graceful Shutdown** | Clean disconnection of all bots on SIGINT/SIGTERM |
| **🧹 Session Cleanup** | Automatic cleanup of temporary data after broadcasts |

---

## ⚙️ How It Works

### Architecture

```
┌──────────────────────────────────────────────────┐
│                   Main Bot                        │
│  (Command Handler, Orchestrator, Dashboard)       │
└──────────┬──────────┬──────────┬─────────────────┘
           │          │          │
     ┌─────▼──┐ ┌─────▼──┐ ┌─────▼──┐
     │ Bot #1 │ │ Bot #2 │ │ Bot #N │  ← Helper Bots
     └────┬───┘ └────┬───┘ └────┬───┘
          │          │          │
          ▼          ▼          ▼
     ┌──────────────────────────────────┐
     │       Server Members (DMs)        │
     │  (Each bot sends to 500 members)  │
     └──────────────────────────────────┘
```

### Broadcast Flow

1. **Command Received** — User runs `+bc <message>` in the server
2. **Bot Selection** — Interactive menu lets you choose which helper bots to use
3. **Member Fetching** — The system fetches all non-bot members (with smart retry for rate limits)
4. **Work Distribution** — Members are shuffled and distributed evenly across selected bots (~500 members per bot)
5. **Execution** — Each bot sends DMs in batches of 15, with random delays (4-8s between messages, 20-30s between batches)
6. **Live Monitoring** — A control panel embed updates every 5 seconds with real-time stats
7. **Completion** — A final summary report is posted with success/failure rates

### Safety Mechanisms

- **Rate Limit Handling:** Automatic backoff when Discord returns 429 errors
- **Error Streak Detection:** After 5 consecutive errors, a 45-second safety cooldown activates
- **Batch Processing:** Messages are sent in batches of 15 with rest periods between batches
- **Bot Ban Detection:** If a helper bot gets banned, it stops immediately
- **Closed DM Detection:** Members with closed DMs are counted as "skipped" (not failed)

---

## 🎮 Commands

> **Prefix:** `+` (configurable in `config.json`)

### 🤖 Bot Management

| Command | Aliases | Permission | Description |
|---------|---------|-----------|-------------|
| `+set-tokens <token1> [token2...]` | `+إضافة-توكنات` | Owner | Add one or more helper bot tokens |
| `+list-tokens` | `+التوكنات` | Owner | Display all bots and their status |
| `+delete-token <ID/token>` | `+حذف-توكن` | Owner | Remove a bot by ID or token |
| `+edit-token <old/ID> <new>` | `+تعديل-توكن` | Owner | Replace an existing token |
| `+check-tokens` | `+فحص-التوكنات` | Owner | Validate all tokens and remove invalid ones |
| `+clear-tokens` | `+حذف-التوكنات` | Owner | Delete bots via interactive menu |
| `+set-name <name>` | `+تغيير-الاسم` | Owner | Change bot username(s) via menu |

### 📢 Broadcast

| Command | Aliases | Permission | Description |
|---------|---------|-----------|-------------|
| `+bc <message>` | `+بث` | Admin | Broadcast to all server members |
| `+obc <message>` | `+بث-اونلاين` | Admin | Broadcast only to online members |

### ℹ️ General

| Command | Aliases | Permission | Description |
|---------|---------|-----------|-------------|
| `+ping` | — | Admin | Check bot latency |
| `+stats` | `+الإحصائيات` | Owner | View bot statistics |
| `+help` | `+المساعدة` | Admin | Show the help menu |

> **💡 Tip:** All broadcast and management commands also work via DM to the bot owner, allowing operation even if the bot is restricted in the server.

---

## 🚀 Setup Guide

### Prerequisites

- **Node.js** v22.0.0 or higher
- **npm** (comes with Node.js)
- **Discord Bot Token** from [Discord Developer Portal](https://discord.com/developers/applications)
- One or more **helper bot tokens** (you can create additional bots via the Developer Portal)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Ah-Med0/BroadCast-controle-v1.6.1
cd broadcast-controle

# 2. Install dependencies
npm install

# 3. Set up environment variables
cp .env.example .env
# Edit .env and add your main bot token:
# BOT_TOKEN=your_main_bot_token_here

# 4. Configure the bot
# Edit config.json with your owner ID and server ID

# 5. Start the bot
npm start

# For development (with auto-reload):
npm run dev
```

### Configuration

Edit `config.json`:

```json
{
  "bot": {
    "prefix": "+",
    "status": "dnd",
    "activity": {
      "name": "Smart BroadCast Controle",
      "type": "PLAYING"
    }
  },
  "owner": {
    "id": "YOUR_DISCORD_USER_ID",
    "name": "Ahmed Amine(2.7m)"
  },
  "guild": {
    "id": "YOUR_SERVER_ID"
  },
  "permissions": {
    "required": "Administrator"
  }
}
```

### Environment Variables (`.env`)

```env
BOT_TOKEN=your_main_bot_token_here
```

### Helper Bots Setup

1. Create additional bot applications on the [Discord Developer Portal](https://discord.com/developers/applications)
2. Invite them to your server with Administrator permissions
3. Add their tokens using `+set-tokens <token1> <token2> ...`

> **⚠️ Security:** Never commit your `.env` file or `database.json` to GitHub. They are already in `.gitignore`.

---

## 📁 Project Structure

```
broadcast-controle/
├── index.js          # Main bot code (orchestrator, commands, broadcast engine)
├── config.json       # Bot configuration (prefix, owner, guild)
├── database.json     # Stored bot tokens (auto-managed by pro.db)
├── package.json      # Dependencies and scripts
├── .env.example      # Environment variables template
├── .gitignore        # Files ignored by Git
└── README.md         # This file
```

---

## 🛡️ Safety & Rate Limits

The bot uses carefully tuned constants to avoid hitting Discord's rate limits:

| Parameter | Value | Description |
|-----------|-------|-------------|
| Min Delay | 4,000ms | Minimum time between messages per bot |
| Max Delay | 8,000ms | Maximum time between messages per bot |
| Batch Size | 15 | Messages sent before taking a rest |
| Batch Rest | 20-30s | Rest period between batches |
| Members/Bot | 500 | Maximum members assigned per bot |
| Error Threshold | 5 | Consecutive errors before safety cooldown |
| Safety Rest | 45s | Cooldown duration after error streak |
| Panel Update | 5s | Dashboard refresh interval |
| Health Check | 30min | Periodic bot reconnection check |

---

## ❓ FAQ

**Q: Why use multiple bots?**  
A: Discord's API rate limits a single bot to ~5 messages per second. By distributing across multiple bots, you can achieve much higher throughput while staying within limits.

**Q: Is this against Discord ToS?**  
A: Mass DMing can be against Discord's Terms of Service if used for spam. Use responsibly for legitimate community announcements.

**Q: Can the main bot also send messages?**  
A: No. The main bot is the orchestrator only — it cannot be added as a helper bot. Helper bots are separate bot accounts.

**Q: What happens if a member has DMs closed?**  
A: They are counted as "skipped" and do not count as failures or errors.

**Q: How do I stop an ongoing broadcast?**  
A: Click the red "⏹ إيقاف البرودكاست" button on the live dashboard panel.

---

## 📄 License

Copyright © 2026 **Ahmed Amine (2.7m)**

This project is licensed under the **MIT License** — see the [LICENSE.md](LICENSE.md) file for details.

---

<div align="center">

**Built with ❤️ by [Ahmed Amine (2.7m)](https://discord.com/users/580414690472230962)**

[![Discord](https://img.shields.io/badge/Support-Server-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/x2QqPMP9WB)

</div>
