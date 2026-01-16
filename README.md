# 🏮 Festa delle Lanterne 2026 - Supabase Version
# 🏮 元宵节 2026 - Supabase 版本

## 📋 Overview | 概述

Complete bilingual event management system powered by **Supabase** (PostgreSQL).

基于 **Supabase**（PostgreSQL）的完整双语活动管理系统。

---

## 📁 Files | 文件

| File | Description | 描述 |
|------|-------------|------|
| `index.html` | Registration page | 注册页面 |
| `verify.html` | QR Scanner for entry | 入场二维码扫描 |
| `admin.html` | Admin dashboard | 管理后台 |
| `SUPABASE_SETUP.md` | Setup guide | 设置指南 |

---

## ✨ Features | 功能

### Registration | 注册
- 🌐 Bilingual (Italian/Chinese) | 双语
- 👤 Role selection (Visitor/Staff) | 角色选择
- 📧 Contact validation | 联系方式验证
- 🎯 Lottery number with duplicate check | 抽奖号码重复检查
- 📱 QR code generation | 二维码生成
- 📥 Downloadable ticket | 可下载门票

### Verification | 验证
- 📷 Real-time QR scanner | 实时二维码扫描
- ⌨️ Manual ID lookup | 手动ID查询
- ✅ Entry confirmation | 入场确认
- ⚠️ Duplicate detection | 重复检测
- 🔊 Audio feedback | 声音反馈

### Admin | 管理
- 📊 Live statistics | 实时统计
- 📄 CSV export | CSV 导出
- 📊 Excel export | Excel 导出
- 🎯 Lottery draw | 抽奖
- 🔍 Search & filter | 搜索筛选

---

## 🚀 Quick Start | 快速开始

### 1. Setup Supabase (15 min) | 设置 Supabase（15分钟）

```
1. Go to supabase.com
2. Create new project
3. Run SQL schema (see SUPABASE_SETUP.md)
4. Copy URL + anon key
5. Update all HTML files
```

### 2. Deploy | 部署

**Netlify (Easiest):**
```
Drag folder to netlify.com/drop
```

**GitHub Pages:**
```
Upload to repo → Enable Pages
```

---

## 📅 Event Details | 活动详情

| | |
|---|---|
| **Event** | Festa delle Lanterne 2026 |
| **Date** | 03/02/2026 |
| **Location** | Piazzale Michelangelo, Firenze |
| **Staff Entry** | 14:00 |
| **Visitor Entry** | 15:00 |

---

## 🗄️ Database Schema | 数据库结构

```sql
registrations (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    role TEXT NOT NULL,
    email TEXT,
    phone TEXT,
    lottery_number TEXT UNIQUE,
    entry_time TEXT,
    entered BOOLEAN,
    entered_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ
)
```

---

## 🆚 Why Supabase? | 为什么选择 Supabase？

| Advantage | 优势 |
|-----------|------|
| Open source | 开源 |
| PostgreSQL (powerful) | PostgreSQL（强大） |
| 500MB free storage | 500MB 免费存储 |
| Real-time built-in | 内置实时功能 |
| SQL queries | SQL 查询 |
| Self-host option | 可自托管 |

---

## 💻 Technical Stack | 技术栈

- **Database:** Supabase (PostgreSQL)
- **Frontend:** Vanilla HTML/CSS/JS
- **QR Scanner:** html5-qrcode
- **QR Generator:** qrcode.js
- **Excel Export:** SheetJS

---

**🏮 Buon Anno del Serpente! 蛇年大吉！🐍**
