# 🌍 Crypto World - P2P Multiplayer Metaverse

Production-ready crypto-integrated P2P multiplayer game with wallet authentication, in-game payments, and decentralized networking.

## 🚀 Features

### 💼 Wallet Integration
- **Phantom Wallet** (Solana) - Full integration with SOL balance display
- **MetaMask** (Base/Ethereum) - ETH/Base network support
- Wallet-based authentication and identity
- Real-time balance tracking

### 🌐 P2P Networking
- WebRTC-based peer-to-peer connections
- No central server required
- Decentralized server/room system
- Supports multiple players per server
- Real-time position synchronization (50ms updates)
- Automatic peer discovery and connection management

### 💬 Communication System
- **Global Chat** - Server-wide messaging
- **Direct Messages** - Private 1-on-1 chat
- **Voice Chat** - WebRTC voice communication (toggle on/off)
- **Audio Controls** - Mute/unmute capabilities
- Real-time message broadcasting

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
- Player list with online status
- Player name tags (wallet addresses)
- Proximity-based interactions
- Server browser with player counts
- Create and manage servers

## 🛠 Technology Stack

- **Frontend**: Three.js, WebRTC, PeerJS
- **Blockchain**: Solana Web3.js, Ethers.js
- **Wallets**: Phantom, MetaMask
- **Networking**: WebRTC P2P mesh network
- **Hosting**: Vercel (static deployment)

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
