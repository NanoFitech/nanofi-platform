# 🎯 Setup Verdana Protocol MVP dengan Cursor IDE

Panduan lengkap setup project untuk hackathon Solana menggunakan Cursor.

---

## 📋 Prerequisites

Sebelum mulai, pastikan Anda punya:
- ✅ **Cursor IDE** installed (https://cursor.sh)
- ✅ **Git** installed
- ✅ **Terminal access** (sudah built-in di Cursor)

---

## 🚀 STEP 1: Open Project di Cursor

### Option A: Dari File Explorer
1. Buka Cursor IDE
2. File → Open Folder
3. Navigate ke folder `verdana-mvp`
4. Click "Open"

### Option B: Dari Terminal
```bash
# Navigate ke folder project
cd path/to/verdana-mvp

# Open dengan Cursor
cursor .
```

---

## 💻 STEP 2: Open Terminal di Cursor

Ada 3 cara:
1. **Menu**: Terminal → New Terminal
2. **Keyboard**: `Ctrl + ` (backtick)` atau `Cmd + `` (Mac)
3. **Shortcut**: `Ctrl + Shift + ` (Windows/Linux)

Terminal akan muncul di bagian bawah Cursor.

---

## 🔧 STEP 3: Install Prerequisites

Copy-paste command ini satu per satu di terminal Cursor:

### 3.1 Install Rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
- Pilih option 1 (default installation)
- Tunggu sampai selesai (~5 menit)
- Restart terminal: `source $HOME/.cargo/env`

**Verify:**
```bash
rustc --version
cargo --version
```

### 3.2 Install Solana CLI
```bash
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
```
- Tunggu download selesai
- Add to PATH (ikuti instruksi yang muncul)

**Verify:**
```bash
solana --version
```

### 3.3 Install Anchor
```bash
# Install Anchor Version Manager
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force

# Install Anchor 0.29.0
avm install 0.29.0
avm use 0.29.0
```

**Verify:**
```bash
anchor --version
# Should show: anchor-cli 0.29.0
```

### 3.4 Install Node.js (jika belum ada)
```bash
# Check apakah sudah ada
node --version

# Jika belum ada, install via nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc  # or ~/.zshrc for macOS
nvm install 18
nvm use 18
```

**Verify:**
```bash
node --version  # Should be v18.x.x
npm --version
```

---

## 💳 STEP 4: Setup Solana Wallet

Di terminal Cursor:

### 4.1 Generate Keypair
```bash
# Create config directory
mkdir -p ~/.config/solana

# Generate new keypair
solana-keygen new --outfile ~/.config/solana/id.json
```

**PENTING**: Simpan seed phrase yang muncul! ✍️

### 4.2 Set ke Devnet
```bash
solana config set --url devnet
```

### 4.3 Check Balance
```bash
solana balance
```

### 4.4 Airdrop Test SOL
```bash
# Airdrop 2 SOL
solana airdrop 2

# Check balance lagi
solana balance
```

**Troubleshooting Airdrop:**
Jika gagal, coba:
```bash
# Method 1: Retry
solana airdrop 2

# Method 2: Use web faucet
# Visit: https://faucet.solana.com
# Paste address dari: solana address
```

---

## 🏗️ STEP 5: Build & Deploy Smart Contract

### 5.1 Build Program
Di terminal Cursor (pastikan Anda di folder `verdana-mvp`):

```bash
# Build Anchor program
anchor build
```

**Output yang benar:**
```
✔ Built target/deploy/verdana_protocol.so
✔ Generated IDL at target/idl/verdana_protocol.json
```

**Jika error "anchor: command not found":**
```bash
# Add to PATH dan restart terminal
export PATH="$HOME/.avm/bin:$PATH"
source ~/.bashrc  # or ~/.zshrc
```

### 5.2 Get Program ID
```bash
solana address -k target/deploy/verdana_protocol-keypair.json
```

Copy output ini (format: `XXXX...XXXX`)

### 5.3 Update Program ID

Buka 3 files ini di Cursor dan update program ID:

**File 1: `Anchor.toml`** (Line 5)
```toml
[programs.devnet]
verdana_protocol = "PASTE_YOUR_PROGRAM_ID_HERE"
```

**File 2: `programs/verdana-protocol/src/lib.rs`** (Line 8)
```rust
declare_id!("PASTE_YOUR_PROGRAM_ID_HERE");
```

**File 3: `app/src/idl/verdana_protocol.json`** (Last line)
```json
"metadata": {
  "address": "PASTE_YOUR_PROGRAM_ID_HERE"
}
```

💡 **Cursor Pro Tip**: 
- Use `Cmd/Ctrl + P` untuk quick file search
- Type nama file untuk jump langsung

### 5.4 Rebuild & Deploy
```bash
# Rebuild dengan program ID yang baru
anchor build

# Deploy ke Solana Devnet
anchor deploy --provider.cluster devnet
```

**Output sukses:**
```
Deploying cluster: https://api.devnet.solana.com
Upgrade authority: ~/.config/solana/id.json
Deploying program "verdana_protocol"...
Program Id: XXXX...XXXX

Deploy success
```

### 5.5 Copy IDL to Frontend
```bash
cp target/idl/verdana_protocol.json app/src/idl/
```

---

## 🎨 STEP 6: Setup & Run Frontend

### 6.1 Open New Terminal
Di Cursor: Click `+` icon di terminal panel

### 6.2 Navigate ke App
```bash
cd app
```

### 6.3 Install Dependencies
```bash
npm install
```

Tunggu ~2-3 menit

### 6.4 Start Development Server
```bash
npm start
```

**Output sukses:**
```
Compiled successfully!

You can now view verdana-protocol-app in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://192.168.x.x:3000
```

Browser akan otomatis buka ke `http://localhost:3000`

---

## 🦊 STEP 7: Setup Phantom Wallet

### 7.1 Install Extension
1. Chrome/Brave: https://phantom.app/download
2. Click "Add to Chrome"
3. Create new wallet atau import existing

### 7.2 Switch to Devnet
1. Open Phantom wallet
2. Click ⚙️ (Settings)
3. Scroll ke "Developer Settings"
4. Enable "Testnet Mode"
5. Select "Devnet"

### 7.3 Airdrop SOL ke Wallet
```bash
# Get wallet address dari Phantom (copy)
# Then:
solana airdrop 2 YOUR_PHANTOM_ADDRESS
```

---

## 🎮 STEP 8: Test MVP

Di browser (`http://localhost:3000`):

### 8.1 Connect Wallet
1. Click "Select Wallet" button
2. Choose "Phantom"
3. Approve connection

### 8.2 Initialize Protocol
1. Click "Initialize Protocol" button
2. Approve transaction di Phantom
3. Wait for confirmation

**Output**: "Protocol initialized! Transaction: xxx..."

### 8.3 Create Project
1. Click "Create Project"
2. Fill form:
   - Name: "Test Ocean Cleanup"
   - Description: "Remove plastic from beaches"
   - Category: Plastic Recycling
   - Goal: 10 SOL
3. Click "Create Project"
4. Approve transaction

**Output**: "Project created successfully! 🎉"

### 8.4 Browse & Fund
1. Click "Browse Projects"
2. See your project listed
3. Click on it
4. Enter amount: 1 SOL
5. Click "Fund Project"
6. Approve transaction
7. See progress bar update!

---

## 🤖 STEP 9: Using Cursor AI Features

Cursor punya AI yang sangat powerful! Gunakan untuk:

### 9.1 Cmd+K (Code Generation)
1. Select code yang mau dimodify
2. Press `Cmd+K` (atau `Ctrl+K` di Windows)
3. Describe perubahan yang diinginkan
4. AI akan generate code

**Example**:
- Select fungsi `fundProject`
- Press Cmd+K
- Type: "Add validation for minimum funding amount of 0.1 SOL"
- Accept changes

### 9.2 Cmd+L (Chat with AI)
1. Press `Cmd+L` (atau `Ctrl+L`)
2. Chat panel muncul di kanan
3. Ask questions tentang code

**Example Questions**:
- "How does the fundProject function work?"
- "What's the difference between publicKey and keypair?"
- "How to add a new field to Project struct?"

### 9.3 @ Commands in Chat
- `@codebase`: Reference entire codebase
- `@file`: Reference specific file
- `@docs`: Search documentation

**Example**:
```
@codebase How does milestone tracking work in this project?
```

### 9.4 Cmd+I (Inline Edit)
- Click where you want to add code
- Press `Cmd+I`
- Describe what you want
- AI inserts code directly

---

## 📁 STEP 10: Project Structure di Cursor

Cursor's sidebar akan show:

```
verdana-mvp/
├── 📁 programs/
│   └── verdana-protocol/
│       └── src/
│           └── 📄 lib.rs          ← Smart contract (open this!)
├── 📁 app/
│   ├── 📁 src/
│   │   ├── 📁 components/
│   │   │   ├── 📄 Dashboard.js
│   │   │   ├── 📄 CreateProject.js
│   │   │   ├── 📄 ProjectList.js
│   │   │   └── 📄 FundProject.js
│   │   ├── 📄 App.js             ← Main app
│   │   └── 📄 App.css            ← Styling
│   └── 📄 package.json
├── 📁 target/                     ← Build output
├── 📄 Anchor.toml
├── 📄 README.md
└── 📄 QUICKSTART.md
```

**Files penting untuk edit:**
1. `programs/verdana-protocol/src/lib.rs` - Smart contract
2. `app/src/components/` - Frontend components
3. `app/src/App.css` - Styling

---

## 🎨 STEP 11: Customize Project (Optional)

### 11.1 Change Colors
Buka `app/src/App.css` di Cursor:

```css
/* Line 8-15 */
:root {
  --primary-green: #2E7D32;      /* Ubah warna utama */
  --secondary-green: #388E3C;    /* Ubah warna sekunder */
  --light-green: #4CAF50;        /* Ubah accent color */
}
```

**Cursor Tip**: Use `Cmd+D` untuk multi-cursor select and edit

### 11.2 Update Project Name
Search & Replace di Cursor:
- Press `Cmd+Shift+F` (global search)
- Search: "Verdana Protocol"
- Replace: "Your Project Name"
- Click "Replace All"

### 11.3 Add Features
Gunakan Cursor AI:
1. Open `programs/verdana-protocol/src/lib.rs`
2. Press `Cmd+K`
3. Type: "Add a function to vote on projects"
4. Review & accept code

---

## 🐛 Troubleshooting di Cursor

### Issue 1: "anchor: command not found"
**Solution:**
```bash
# Di terminal Cursor
export PATH="$HOME/.avm/bin:$PATH"
source ~/.bashrc
```

### Issue 2: "solana-keygen: command not found"
**Solution:**
```bash
export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"
source ~/.bashrc
```

### Issue 3: Terminal hang saat npm install
**Solution:**
- Click 🗑️ (trash icon) untuk kill terminal
- Open new terminal
- Coba lagi: `npm install --legacy-peer-deps`

### Issue 4: Port 3000 sudah dipakai
**Solution:**
```bash
# Kill process di port 3000
lsof -ti:3000 | xargs kill -9

# Or use different port
PORT=3001 npm start
```

### Issue 5: Hot reload tidak work
**Solution:**
1. Stop server (`Ctrl+C`)
2. Delete `node_modules` dan `package-lock.json`
3. `npm install`
4. `npm start`

---

## ✅ Verification Checklist

Mark each setelah berhasil:

**Environment:**
- [ ] Rust installed (`rustc --version`)
- [ ] Solana CLI installed (`solana --version`)
- [ ] Anchor installed (`anchor --version`)
- [ ] Node.js v18+ (`node --version`)

**Wallet:**
- [ ] Keypair generated
- [ ] Config set to devnet
- [ ] Balance > 0 SOL
- [ ] Phantom wallet installed & connected

**Smart Contract:**
- [ ] Program built successfully
- [ ] Program ID updated in 3 files
- [ ] Program deployed to devnet
- [ ] IDL copied to frontend

**Frontend:**
- [ ] Dependencies installed
- [ ] Server running on port 3000
- [ ] Can connect wallet
- [ ] Can initialize protocol
- [ ] Can create project
- [ ] Can fund project

---

## 🎬 Next Steps: Record Demo

### Setup di Cursor:
1. **Split Screen**:
   - View → Editor Layout → Split Right
   - Left: Code
   - Right: Browser

2. **Record with OBS**:
   - Show code di Cursor
   - Show app di browser
   - Explain while recording

3. **Use Cursor AI for Script**:
   ```
   Press Cmd+L
   Type: "Generate a 3-minute demo script for a Web3 sustainability platform"
   ```

---

## 💡 Pro Tips untuk Development di Cursor

### 1. Multi-cursor Editing
- `Cmd+D`: Select next occurrence
- `Cmd+Shift+L`: Select all occurrences
- `Alt+Click`: Add cursor

### 2. Quick Navigation
- `Cmd+P`: Go to file
- `Cmd+Shift+O`: Go to symbol
- `Cmd+T`: Go to symbol in workspace
- `Cmd+Click`: Go to definition

### 3. Terminal Tips
- `Cmd+` `: Toggle terminal
- `Cmd+Shift+` `: Create new terminal
- Split terminal: Click split icon

### 4. AI Integration
- Ask questions dengan `@codebase`
- Generate tests dengan AI
- Explain error messages
- Refactor code automatically

### 5. Git Integration
- View changes: Source Control panel (Cmd+Shift+G)
- Stage & commit langsung dari Cursor
- Push to GitHub

---

## 📹 Recording Setup di Cursor

### For Best Demo Recording:

1. **Clean Interface**:
   - Hide minimap: View → Show Minimap (uncheck)
   - Hide sidebar: `Cmd+B`
   - Zen mode: `Cmd+K Z`

2. **Font Size untuk Recording**:
   - `Cmd+,` (Settings)
   - Search "font size"
   - Set to 16-18

3. **Theme** (pilih yang jelas):
   - `Cmd+K Cmd+T`
   - Choose "Dark+ (default dark)" atau "Light+ (default light)"

4. **Layout for Demo**:
   - Terminal di bawah: Toggle dengan `Cmd+` `
   - Browser: Separate window di samping

---

## 🚀 Deploy Frontend (Bonus)

Setelah testing lokal, deploy ke Vercel:

### Di Terminal Cursor:
```bash
cd app
npm run build

# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Follow prompts:
# - Set up and deploy? Y
# - Which scope? (your account)
# - Link to existing project? N
# - Project name? verdana-protocol
# - Directory? ./
# - Override settings? N
```

Get public URL! 🎉

---

## 🎯 Summary - What You Did

1. ✅ Opened project di Cursor
2. ✅ Installed Rust, Solana, Anchor, Node.js
3. ✅ Generated Solana wallet
4. ✅ Built & deployed smart contract
5. ✅ Ran frontend development server
6. ✅ Connected Phantom wallet
7. ✅ Tested full flow (initialize → create → fund)

---

## 📞 Need Help?

**In Cursor:**
1. Press `Cmd+L`
2. Ask AI: "@codebase Why is my transaction failing?"

**External Resources:**
- Solana Discord: https://discord.gg/solana
- Anchor Docs: https://www.anchor-lang.com
- Cursor Docs: https://docs.cursor.sh

---

## 🏆 You're Ready!

Project setup complete! Sekarang Anda bisa:
- ✅ Develop & test locally
- ✅ Modify smart contracts
- ✅ Update frontend
- ✅ Use Cursor AI untuk help
- ✅ Record demo
- ✅ Submit to hackathon

**Total setup time**: ~1-2 jam (termasuk instalasi)

---

**Happy Hacking di Cursor! 🚀🌿**
