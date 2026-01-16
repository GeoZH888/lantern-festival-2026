# 🔧 Supabase Setup Guide | Supabase 设置指南

## Step 1: Create Supabase Account | 创建 Supabase 账户

1. Go to [https://supabase.com](https://supabase.com)
2. Click **"Start your project"** / 点击 **"Start your project"**
3. Sign up with GitHub or email / 使用 GitHub 或邮箱注册

---

## Step 2: Create New Project | 创建新项目

1. Click **"New Project"** / 点击 **"New Project"**
2. Fill in:
   - **Name / 名称:** `festa-lanterne-2026`
   - **Database Password:** (save this!) / （保存好！）
   - **Region:** Choose closest (EU for Italy) / 选择最近的区域
3. Click **"Create new project"** / 点击 **"Create new project"**
4. Wait 2-3 minutes for setup... / 等待 2-3 分钟...

---

## Step 3: Create Database Table | 创建数据库表

1. Go to **SQL Editor** (left menu) / 左侧菜单点击 **SQL Editor**
2. Click **"New query"** / 点击 **"New query"**
3. **Copy and paste this SQL:** / **复制粘贴以下 SQL：**

```sql
-- Create registrations table
CREATE TABLE registrations (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    role TEXT NOT NULL CHECK (role IN ('visitor', 'staff')),
    email TEXT,
    phone TEXT,
    lottery_number TEXT UNIQUE NOT NULL,
    entry_time TEXT NOT NULL,
    event_date DATE DEFAULT '2026-02-03',
    location TEXT DEFAULT 'Piazzale Michelangelo, Firenze',
    entered BOOLEAN DEFAULT FALSE,
    entered_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    
    -- At least one contact method required
    CONSTRAINT contact_required CHECK (email IS NOT NULL OR phone IS NOT NULL)
);

-- Create index for faster lottery number lookup
CREATE INDEX idx_lottery_number ON registrations(lottery_number);

-- Create index for entry status queries
CREATE INDEX idx_entered ON registrations(entered);

-- Create index for role filtering
CREATE INDEX idx_role ON registrations(role);

-- Enable Row Level Security (RLS)
ALTER TABLE registrations ENABLE ROW LEVEL SECURITY;

-- Policy: Allow anyone to read
CREATE POLICY "Allow public read" ON registrations
    FOR SELECT USING (true);

-- Policy: Allow anyone to insert (for registration)
CREATE POLICY "Allow public insert" ON registrations
    FOR INSERT WITH CHECK (true);

-- Policy: Allow updates only to entry status
CREATE POLICY "Allow entry updates" ON registrations
    FOR UPDATE USING (true)
    WITH CHECK (true);

-- Enable realtime for the table
ALTER PUBLICATION supabase_realtime ADD TABLE registrations;
```

4. Click **"Run"** (or Ctrl+Enter) / 点击 **"Run"**（或 Ctrl+Enter）
5. You should see "Success" / 应该看到 "Success"

---

## Step 4: Get API Keys | 获取 API 密钥

1. Go to **Settings** → **API** (left menu)
   
   左侧菜单 **Settings** → **API**

2. Find and copy:
   - **Project URL:** `https://xxxxx.supabase.co`
   - **anon public key:** `eyJhbGciOiJIUzI1NiIsInR5cCI6...`

---

## Step 5: Update Your Files | 更新您的文件

Open each HTML file and replace the config:

打开每个 HTML 文件并替换配置：

### Files to update / 需要更新的文件:
- `index.html`
- `verify.html`  
- `admin.html`

### Find this code / 找到这段代码:
```javascript
const SUPABASE_URL = 'https://YOUR_PROJECT.supabase.co';
const SUPABASE_ANON_KEY = 'YOUR_ANON_KEY';
```

### Replace with your values / 替换为您的值:
```javascript
const SUPABASE_URL = 'https://abcdefghijk.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

---

## Step 6: Test | 测试

1. Open `index.html` in browser / 在浏览器中打开 `index.html`
2. Check connection shows "🟢 Online" / 检查连接显示 "🟢 Online"
3. Try a test registration / 尝试测试注册
4. Check Supabase Dashboard → **Table Editor** to see data
   
   在 Supabase Dashboard → **Table Editor** 查看数据

---

## 🎉 Done! | 完成！

Your system is now connected to Supabase with:

您的系统现已连接到 Supabase，具有：

- ✅ PostgreSQL database / PostgreSQL 数据库
- ✅ Real-time subscriptions / 实时订阅
- ✅ Automatic timestamps / 自动时间戳
- ✅ Data validation / 数据验证
- ✅ Unique lottery numbers / 唯一抽奖号码

---

## 📊 Supabase Dashboard Features | Supabase 控制台功能

| Feature | Location | Use |
|---------|----------|-----|
| **Table Editor** | Left menu | View/edit all data |
| **SQL Editor** | Left menu | Run custom queries |
| **Authentication** | Left menu | User management (optional) |
| **API Docs** | Left menu | Auto-generated API docs |
| **Logs** | Left menu | Debug issues |

---

## 🔒 Security Tips | 安全提示

### For Production / 生产环境:

Update Row Level Security policies in SQL Editor:

```sql
-- More restrictive insert policy (prevent duplicate entries)
DROP POLICY "Allow public insert" ON registrations;
CREATE POLICY "Allow new registrations" ON registrations
    FOR INSERT WITH CHECK (
        NOT EXISTS (
            SELECT 1 FROM registrations 
            WHERE lottery_number = NEW.lottery_number
        )
    );

-- Restrict updates to only entered status
DROP POLICY "Allow entry updates" ON registrations;
CREATE POLICY "Allow entry confirmation" ON registrations
    FOR UPDATE USING (true)
    WITH CHECK (
        entered = true AND 
        entered_at IS NOT NULL
    );
```

---

## 📞 Troubleshooting | 故障排除

| Problem | Solution |
|---------|----------|
| "🔴 Offline" | Check SUPABASE_URL and SUPABASE_ANON_KEY |
| "Failed to fetch" | Verify project URL has no typos |
| Table not found | Run the SQL schema in Step 3 |
| Duplicate lottery error | Number already exists - choose another |
| Real-time not working | Check `ALTER PUBLICATION` command ran |

---

## 🆚 Supabase vs Firebase

| Feature | Supabase | Firebase |
|---------|----------|----------|
| Database | PostgreSQL (SQL) | NoSQL |
| Pricing | 500MB free | Spark plan free |
| Open Source | ✅ Yes | ❌ No |
| Self-host option | ✅ Yes | ❌ No |
| Real-time | ✅ Yes | ✅ Yes |
| Complexity | Medium | Easy |

---

## 📁 Database Schema | 数据库结构

```
registrations
├── id (TEXT, PRIMARY KEY) - e.g., "FDL2026-ABC123"
├── name (TEXT, NOT NULL) - Participant name
├── role (TEXT, NOT NULL) - "visitor" or "staff"
├── email (TEXT, NULLABLE) - Email address
├── phone (TEXT, NULLABLE) - Phone number
├── lottery_number (TEXT, UNIQUE) - 4-digit number
├── entry_time (TEXT) - "14:00" or "15:00"
├── event_date (DATE) - 2026-02-03
├── location (TEXT) - Piazzale Michelangelo
├── entered (BOOLEAN) - Entry status
├── entered_at (TIMESTAMPTZ) - Entry timestamp
└── created_at (TIMESTAMPTZ) - Registration time
```

---

**🏮 新年快乐！Buon Anno del Serpente! 🐍**
