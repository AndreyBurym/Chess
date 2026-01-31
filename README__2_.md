# Real Multiplayer Chess Setup Guide

## 📋 Overview
This guide will help you add real WebSocket-based multiplayer to your chess game so actual players can join your lobbies and play together in real-time.

## 🚀 Quick Start

### Step 1: Install Server Dependencies
```bash
npm install
```

### Step 2: Start the Server
```bash
npm start
```

The server will run on `http://localhost:8080` by default.

### Step 3: Update Your HTML File

Add this button to your HTML (in the game tab section):
```html
<button class="btn btn-warning btn-block" onclick="manualConnectServer()" style="margin-top: 12px;">
    🔌 Connect to Server
</button>
```

Add the WebSocket client script BEFORE the closing `</script>` tag in your HTML:
```html
<!-- Add all the code from chess-multiplayer-client.js here -->
```

### Step 4: Test Locally
1. Open your chess HTML file in TWO different browsers or browser windows
2. Click "Connect to Server" in both windows
3. In window 1: Click "Create Lobby" and get your lobby code
4. In window 2: Click "Join Lobby" and enter the code
5. Play chess together in real-time!

## 🌐 Deploying to Production

### Option 1: Deploy Server to Render.com (Free)
1. Create account at https://render.com
2. Create new "Web Service"
3. Connect your GitHub repo (upload chess-server.js and package.json)
4. Set build command: `npm install`
5. Set start command: `npm start`
6. Deploy!
7. Copy your server URL (e.g., `https://your-app.onrender.com`)
8. Update `wsUrl` in your HTML to: `wss://your-app.onrender.com`

### Option 2: Deploy Server to Railway.app (Free)
1. Create account at https://railway.app
2. Click "New Project" → "Deploy from GitHub"
3. Upload chess-server.js and package.json
4. Railway auto-detects Node.js and deploys
5. Copy your public URL
6. Update `wsUrl` in your HTML

### Option 3: Deploy Server to Glitch.com
1. Go to https://glitch.com
2. Create new project
3. Upload chess-server.js and package.json
4. Your app runs automatically
5. Copy the live app URL
6. Update `wsUrl` in your HTML

### Option 4: Deploy to Your Own VPS
```bash
# SSH into your server
ssh user@yourserver.com

# Upload files
scp chess-server.js package.json user@yourserver.com:~/chess/

# Install and run
cd ~/chess
npm install
node chess-server.js

# Or use PM2 for production
npm install -g pm2
pm2 start chess-server.js --name chess-server
pm2 save
```

## 🎮 How It Works

### Client → Server Messages:
- `CREATE_LOBBY` - Create a new game lobby
- `JOIN_LOBBY` - Join an existing lobby with code
- `MAKE_MOVE` - Send chess move to opponent
- `CANCEL_LOBBY` - Cancel/leave lobby
- `DISCONNECT` - Clean disconnect

### Server → Client Messages:
- `LOBBY_CREATED` - Lobby successfully created (returns code)
- `LOBBY_JOINED` - Successfully joined lobby
- `OPPONENT_JOINED` - Someone joined your lobby
- `START_GAME` - Game starting (includes your color)
- `OPPONENT_MOVE` - Opponent made a move
- `OPPONENT_DISCONNECTED` - Opponent left
- `LOBBY_CANCELLED` - Lobby was cancelled
- `ERROR` - Something went wrong

## 🔧 Configuration

### Change Server Port
In `chess-server.js`, modify:
```javascript
const PORT = process.env.PORT || 8080; // Change 8080 to your port
```

### Change Server URL
In your HTML file, modify:
```javascript
var wsUrl = 'ws://localhost:8080'; // For local testing
// or
var wsUrl = 'wss://your-server.com'; // For production (use wss for HTTPS)
```

### Enable Password Protection
Already built in! Just check the "Require Password" checkbox when creating a lobby.

## 🐛 Troubleshooting

### Can't connect to server?
- Make sure server is running: `npm start`
- Check console for errors (F12 in browser)
- Verify `wsUrl` matches your server address
- If using HTTPS, use `wss://` instead of `ws://`

### Opponent can't join?
- Verify lobby code is correct (6 characters)
- Check if password is required
- Make sure lobby hasn't timed out
- Ensure server is publicly accessible (not just localhost)

### Moves not syncing?
- Check browser console for errors
- Verify WebSocket connection is active (green dot)
- Make sure both players are in the same lobby

### Server crashes?
```bash
# View server logs
pm2 logs chess-server

# Restart server
pm2 restart chess-server
```

## 📊 Server Monitoring

Add this to track active lobbies:
```javascript
// In chess-server.js, add endpoint:
server.on('request', (req, res) => {
    if (req.url === '/status') {
        res.writeHead(200, {'Content-Type': 'application/json'});
        res.end(JSON.stringify({
            activeLobbies: lobbies.size,
            lobbies: Array.from(lobbies.keys())
        }));
    }
});
```

Visit `http://localhost:8080/status` to see active lobbies.

## 🎯 Features Included

✅ Real-time multiplayer with WebSocket
✅ Lobby system with 6-digit codes
✅ Password-protected lobbies
✅ Automatic reconnection
✅ Move synchronization
✅ Disconnect handling
✅ Premoves work in multiplayer
✅ Connection status indicator

## 🔐 Security Notes

For production:
- Add rate limiting to prevent spam
- Validate all moves server-side
- Add authentication if needed
- Use HTTPS (wss://) instead of HTTP (ws://)
- Set up CORS if hosting separately

## 📱 Next Steps

Want to add more features?
- Chat system between players
- Match history
- ELO ranking system
- Spectator mode
- Game replay
- Time controls

## 💡 Testing Tips

1. Use two browsers (Chrome + Firefox) for local testing
2. Use browser incognito mode for second player
3. Use ngrok to test with friends: `ngrok http 8080`
4. Check Network tab in DevTools to see WebSocket traffic

## 🎉 You're Done!

Your chess game now supports real multiplayer! Players anywhere in the world can join your lobbies and play together in real-time.

Need help? Check the server logs and browser console for error messages.
