# 🚀 Quick Setup Guide

## ⚠️ IMPORTANT: Set Up Supabase Database FIRST

Your game **will not work** until you create the database tables in Supabase!

### Step 1: Create Supabase Project

1. Go to [https://supabase.com](https://supabase.com)
2. Sign up / Log in
3. Click "New Project"
4. Choose a name and password
5. Wait for project to be ready (~2 minutes)

### Step 2: Run This SQL Script

1. In your Supabase dashboard, go to **SQL Editor**
2. Click **New Query**
3. Copy and paste this **ENTIRE SCRIPT**:

```sql
-- =====================================================
-- CRYPTO WORLD - COMPLETE DATABASE SETUP
-- =====================================================

-- 1. CREATE PLAYERS TABLE
CREATE TABLE IF NOT EXISTS players (
    id TEXT PRIMARY KEY,
    username TEXT NOT NULL,
    wallet_address TEXT,
    wallet_type TEXT,
    pos_x REAL NOT NULL DEFAULT 0,
    pos_y REAL NOT NULL DEFAULT 1.6,
    pos_z REAL NOT NULL DEFAULT 0,
    rot_y REAL NOT NULL DEFAULT 0,
    animation TEXT DEFAULT 'Idle',
    color TEXT DEFAULT '0x00ccff',
    last_seen TIMESTAMPTZ DEFAULT NOW(),
    joined_at TIMESTAMPTZ DEFAULT NOW()
);

-- 2. CREATE MESSAGES TABLE
CREATE TABLE IF NOT EXISTS messages (
    id BIGSERIAL PRIMARY KEY,
    player_id TEXT NOT NULL,
    username TEXT NOT NULL,
    message TEXT NOT NULL,
    pos_x REAL NOT NULL,
    pos_y REAL NOT NULL,
    pos_z REAL NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 3. CREATE INDEXES
CREATE INDEX IF NOT EXISTS idx_players_position ON players(pos_x, pos_z);
CREATE INDEX IF NOT EXISTS idx_players_last_seen ON players(last_seen DESC);
CREATE INDEX IF NOT EXISTS idx_messages_position ON messages(pos_x, pos_z);
CREATE INDEX IF NOT EXISTS idx_messages_created ON messages(created_at DESC);

-- 4. ENABLE ROW LEVEL SECURITY
ALTER TABLE players ENABLE ROW LEVEL SECURITY;
ALTER TABLE messages ENABLE ROW LEVEL SECURITY;

-- 5. CREATE RLS POLICIES FOR PLAYERS
DROP POLICY IF EXISTS "Enable read access for all users" ON players;
CREATE POLICY "Enable read access for all users" ON players
    FOR SELECT USING (true);

DROP POLICY IF EXISTS "Enable insert for all users" ON players;
CREATE POLICY "Enable insert for all users" ON players
    FOR INSERT WITH CHECK (true);

DROP POLICY IF EXISTS "Enable update for all users" ON players;
CREATE POLICY "Enable update for all users" ON players
    FOR UPDATE USING (true);

DROP POLICY IF EXISTS "Enable delete for all users" ON players;
CREATE POLICY "Enable delete for all users" ON players
    FOR DELETE USING (true);

-- 6. CREATE RLS POLICIES FOR MESSAGES
DROP POLICY IF EXISTS "Enable read access for all users" ON messages;
CREATE POLICY "Enable read access for all users" ON messages
    FOR SELECT USING (true);

DROP POLICY IF EXISTS "Enable insert for all users" ON messages;
CREATE POLICY "Enable insert for all users" ON messages
    FOR INSERT WITH CHECK (true);

-- 7. ENABLE REALTIME
ALTER TABLE players REPLICA IDENTITY FULL;
ALTER TABLE messages REPLICA IDENTITY FULL;

-- 8. ADD TO REALTIME PUBLICATION
ALTER PUBLICATION supabase_realtime ADD TABLE players;
ALTER PUBLICATION supabase_realtime ADD TABLE messages;

-- 9. CREATE CLEANUP FUNCTIONS
CREATE OR REPLACE FUNCTION cleanup_offline_players()
RETURNS void AS $$
BEGIN
    DELETE FROM players
    WHERE last_seen < NOW() - INTERVAL '10 seconds';
END;
$$ LANGUAGE plpgsql;

CREATE OR REPLACE FUNCTION cleanup_old_messages()
RETURNS void AS $$
BEGIN
    DELETE FROM messages
    WHERE created_at < NOW() - INTERVAL '10 minutes';
END;
$$ LANGUAGE plpgsql;


-- SUCCESS! Your database issssss ready.
```

4. Click **Run** button

### Step 3: Enable Realtime Replication (CRITICAL!)

1. Go to **Database > Replication** in Supabase Dashboard
2. Find `players` table → **Enable replication**
3. Find `messages` table → **Enable replication**

### Step 4: Get Your Credentials

1. Go to **Settings > API** in Supabase
2. Copy your **Project URL** (looks like: `https://xxx.supabase.co`)
3. Copy your **anon public** key (starts with `eyJ...`)

### Step 5: Update index.html

Open `index.html` and find line ~2430, update:

```javascript
const CONFIG = {
    SUPABASE_URL: 'YOUR_PROJECT_URL_HERE',
    SUPABASE_ANON_KEY: 'YOUR_ANON_KEY_HERE',
    // ... rest of config
};
```

---

## ✅ Verification

After setup, open the browser console. You should see:
- ✅ "Player initialized in Supabase"
- ✅ "Loaded existing players"
- ❌ **NOT** "Error initializing player" or "404"

If you see errors, go back to Step 3 and make sure Realtime Replication is enabled!

---

## 🎮 Ready to Play!

Your multiplayer metaverse is now working:
- Real-time player synchronization
- Proximity chat
- Persistent wallet login
- Switch wallet functionality

Enjoy! 🚀
