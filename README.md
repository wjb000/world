# 🌍 Crypto World - Multiplayer Metaverse

Production-ready crypto-integrated multiplayer game with wallet authentication, Supabase real-time backend, and proximity-based social features.

## ⚡ Quick Start

1. **Clone or download** this repository
2. **Set up Supabase** (see [Supabase Setup](#supabase-backend-setup) below)
3. **Update credentials** in `index.html` (your Supabase URL and API key)
4. **Deploy** to Vercel, Netlify, or any static host
5. **Visit your site** → Connect wallet → Start playing!

**What works out of the box:**
- ✅ Wallet connection (Phantom + MetaMask)
- ✅ Real-time multiplayer (see other players move)
- ✅ Proximity chat (talk to nearby players)
- ✅ Portfolio showcase (interactive 3D world)
- ✅ Balance display (SOL/ETH)

## 🚀 Features

### 💼 Wallet Integration
- **Phantom Wallet** (Solana) - Full integration with SOL balance display
- **MetaMask** (Base/Ethereum) - ETH/Base network support
- Wallet-based authentication and identity
- Real-time balance tracking

### 🌐 Multiplayer Networking
- **Supabase Real-Time** - Centralized backend for reliable synchronization
- **Single World Architecture** - All players in one massive shared universe
- **Real-time Position Sync** - Player movements update every 50ms
- **Proximity Streaming** - Only load nearby players for performance
- **Automatic Cleanup** - Offline players removed automatically
- **Scalable** - Supports hundreds of concurrent players

### 💬 Proximity Chat System
- **Location-Based Chat** - Only see messages from nearby players (50 unit range)
- **Real-Time Messaging** - Instant message delivery via Supabase subscriptions
- **Player Discovery** - See how many players are nearby
- **Persistent History** - Messages stored for 10 minutes
- **Spam Protection** - 200 character message limit

### 🎮 Game Features
- **3D Environment** - Three.js powered world
- **Third-person Camera** - Smooth follow camera
- **WASD/Arrow Controls** - Standard FPS controls
- **Mobile Support** - Touch controls ready
- **Dropped Items** - Share links and items in-world
- **Portals** - Create teleports to other servers
- **In-game Payments** - Send crypto between players

### 💰 Economic System
- Wallet-to-wallet transfers
- In-game payment system
- Balance display and tracking
- Transaction handling for both Solana and Ethereum networks

### 🎯 Social Features
- **Wallet-Based Identity** - Your wallet address is your unique ID
- **Player Name Tags** - See wallet addresses above players (first 6 + last 4 chars)
- **Proximity Chat** - Talk to nearby players
- **Real-Time Player Count** - See total players online
- **Color Customization** - Choose from 12 vibrant colors
- **Interactive World** - Collectible loot items and portfolio showcase

## 🛠 Technology Stack

- **Frontend**: Three.js (3D rendering), JavaScript ES6+
- **Backend**: Supabase (PostgreSQL + Real-Time subscriptions)
- **Blockchain**: Solana Web3.js, Ethers.js
- **Wallets**: Phantom (Solana), MetaMask (Ethereum/Base)
- **Hosting**: Vercel (static deployment) or any static host

## 📦 Deployment

### Deploy to Vercel

1. Install Vercel CLI:
```bash
npm i -g vercel
```

2. Deploy:
```bash
cd world
vercel --prod
```

3. Follow the prompts to link your Vercel account

### Manual Deployment

1. Fork/upload the repository to GitHub
2. Connect to Vercel dashboard
3. Import the repository
4. Deploy (zero configuration needed)

## 🎮 How to Play

### Getting Started

1. **Connect Wallet**
   - Visit the deployed site
   - Choose Phantom (Solana) or MetaMask (Base/ETH)
   - Approve wallet connection

2. **Join or Create Server**
   - Browse available servers
   - Join existing server by clicking
   - Or create your own server

3. **Play**
   - Use WASD or Arrow keys to move
   - Chat with other players
   - Drop links and create portals
   - Send payments to other players

### Controls

- **Movement**: WASD / Arrow Keys
- **Chat**: Type in chat box and press Enter
- **Voice**: Click microphone button to toggle
- **Actions**: Use action menu on bottom right

## 🔧 Configuration

### RPC Endpoint Setup (Recommended for Production)

The game uses Solana RPC endpoints for balance fetching. For better performance and reliability, set up a dedicated RPC:

#### Free RPC Providers:

**Option 1: Helius (Recommended)**
1. Visit [https://helius.dev](https://helius.dev)
2. Sign up for free account (100k requests/day)
3. Create a new project
4. Copy your RPC endpoint
5. Update `CONFIG.SOLANA_RPC_PRIMARY` in `index.html` (line ~1023)

**Option 2: QuickNode**
1. Visit [https://quicknode.com](https://quicknode.com)
2. Sign up for free tier (100k requests/day)
3. Create Solana Mainnet endpoint
4. Copy your endpoint URL
5. Update `CONFIG.SOLANA_RPC_PRIMARY` in `index.html` (line ~1023)

**Option 3: Alchemy**
1. Visit [https://alchemy.com](https://alchemy.com)
2. Sign up for free tier (300M compute units/month)
3. Create Solana Mainnet app
4. Copy your HTTPS endpoint
5. Update `CONFIG.SOLANA_RPC_PRIMARY` in `index.html` (line ~1023)

#### Configuration Location:
```javascript
// In index.html around line 1023
const CONFIG = {
    SOLANA_RPC_PRIMARY: 'YOUR_RPC_ENDPOINT_HERE',
    SOLANA_RPC_FALLBACK: 'https://rpc.ankr.com/solana'
};
```

### Supabase Backend Setup

**SINGLE WORLD ARCHITECTURE**: The game uses Supabase for real-time multiplayer in ONE massive shared world. All players exist in the same universe with proximity-based streaming.

#### Setting Up Your Supabase Database:

1. **Create a Supabase Project**
   - Visit [https://supabase.com](https://supabase.com)
   - Sign up for free account
   - Create a new project

2. **Run the Complete Setup SQL**

   Copy and paste this **ENTIRE SQL SCRIPT** into your Supabase SQL Editor (run it all at once):

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

   -- SUCCESS! Your database is ready.
   ```

3. **Enable Realtime Replication** (CRITICAL - Do this in Supabase Dashboard)

   After running the SQL above:
   - Go to **Database > Replication** in Supabase Dashboard
   - Find `players` table → click to enable replication
   - Find `messages` table → click to enable replication

   This step is REQUIRED for real-time multiplayer to work!

4. **Get Your Supabase Credentials**

   From your Supabase project dashboard:
   - Go to **Settings > API**
   - Copy your **Project URL** (looks like: `https://xxx.supabase.co`)
   - Copy your **anon/public** key (starts with `eyJ...`)

5. **Update Configuration in index.html**

   Open `index.html` and find the CONFIG object (around line 2430), then update:
   ```javascript
   const CONFIG = {
       SUPABASE_URL: 'https://YOUR_PROJECT_ID.supabase.co',
       SUPABASE_ANON_KEY: 'your_anon_key_here',

       // Optional: Add your custom Solana RPC for better performance
       SOLANA_RPC_PRIMARY: 'https://api.mainnet-beta.solana.com',
       SOLANA_RPC_FALLBACK: 'https://rpc.ankr.com/solana',
   };
   ```

   **Get a Free Custom RPC (Recommended):**
   - [Helius](https://helius.dev) - 100k requests/day free
   - [QuickNode](https://quicknode.com) - 100k requests/day free
   - [Alchemy](https://alchemy.com) - 300M compute units/month free

### Wallet Networks

**Solana (Phantom)**:
- Mainnet-beta by default
- Can be changed in code for devnet/testnet

**Ethereum (MetaMask)**:
- Supports all EVM-compatible chains
- Base network ready
- Configure network in MetaMask

### Server Settings

Servers are stored in localStorage by default. For production:
- Implement IPFS/Arweave for decentralized server registry
- Use blockchain for permanent server records
- Add ENS/SNS for server naming

## 🚀 Production Optimizations

- ✅ Minified dependencies (CDN)
- ✅ Efficient P2P mesh networking
- ✅ Optimized 3D rendering
- ✅ Mobile responsive design
- ✅ Error handling and fallbacks
- ✅ Wallet connection security
- ✅ CORS headers configured
- ✅ Static deployment ready

## 🔒 Security Features

- Client-side wallet signatures
- No private key exposure
- Secure P2P connections
- Transaction confirmation prompts
- Connection timeout handling

## 📱 Browser Support

- Chrome/Brave (Recommended)
- Firefox
- Safari (Limited WebRTC support)
- Edge

**Requirements**:
- Phantom or MetaMask extension installed
- WebRTC support
- WebGL support

## 🐛 Troubleshooting

### Wallet Won't Connect
- Ensure extension is installed and unlocked
- Check browser console for errors
- Try refreshing the page

### Can't Join Server
- Verify wallet is connected
- Check peer connection in console
- Ensure WebRTC is not blocked by firewall

### Voice Chat Not Working
- Allow microphone permissions
- Check browser audio settings
- Ensure other players have audio enabled

## 📈 Scaling

Current implementation supports:
- **Players per server**: 10-20 (WebRTC limit)
- **Concurrent servers**: Unlimited
- **Messages per second**: 100+
- **Position updates**: 20/second

For larger scale:
- Implement TURN servers for better connectivity
- Add mesh optimization for 50+ players
- Use SFU for voice chat scaling

## 🔜 Future Enhancements

- NFT avatars and items
- On-chain achievements
- DAO governance for servers
- Token-gated servers
- Marketplace for in-game items
- Quest system with rewards
- Clan/Guild system
- Tournament modes

## 📄 License

MIT License - Feel free to use and modify

## 🤝 Contributing

Contributions welcome! Please open issues or PRs.

## 💬 Support

For issues or questions, open a GitHub issue.

---

**Built with ❤️ for the decentralized metaverse**
