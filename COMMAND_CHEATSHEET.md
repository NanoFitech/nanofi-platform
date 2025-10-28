# 📋 Command Cheat Sheet - Verdana Protocol Setup

Copy-paste commands ini di terminal Cursor secara berurutan.

---

## 🔧 PART 1: Install Prerequisites

### Install Rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
rustc --version
```

### Install Solana
```bash
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
export PATH="/home/$(whoami)/.local/share/solana/install/active_release/bin:$PATH"
solana --version
```

### Install Anchor
```bash
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
avm install 0.29.0
avm use 0.29.0
anchor --version
```

### Check Node.js (should be v18+)
```bash
node --version
npm --version
```

---

## 💳 PART 2: Setup Wallet

```bash
# Generate keypair
solana-keygen new --outfile ~/.config/solana/id.json

# Set devnet
solana config set --url devnet

# Airdrop SOL
solana airdrop 2

# Check balance
solana balance
```

---

## 🏗️ PART 3: Build & Deploy Program

```bash
# Build
anchor build

# Get program ID (SAVE THIS!)
solana address -k target/deploy/verdana_protocol-keypair.json

# Deploy
anchor deploy --provider.cluster devnet

# Copy IDL
cp target/idl/verdana_protocol.json app/src/idl/
```

**IMPORTANT**: Update program ID di 3 files:
1. `Anchor.toml` - line 5
2. `programs/verdana-protocol/src/lib.rs` - line 8
3. `app/src/idl/verdana_protocol.json` - last line

Then rebuild:
```bash
anchor build
anchor deploy --provider.cluster devnet
cp target/idl/verdana_protocol.json app/src/idl/
```

---

## 🎨 PART 4: Run Frontend

```bash
# Navigate to app
cd app

# Install dependencies
npm install

# Start server
npm start
```

Browser opens at: http://localhost:3000

---

## 🦊 PART 5: Phantom Wallet

1. Install: https://phantom.app/download
2. Switch to Devnet in settings
3. Copy wallet address
4. Airdrop to it:
```bash
solana airdrop 2 YOUR_PHANTOM_ADDRESS
```

---

## 🎮 PART 6: Test Flow

1. Connect wallet
2. Initialize protocol
3. Create project
4. Fund project
5. ✅ Done!

---

## 🐛 Quick Fixes

### "anchor not found"
```bash
export PATH="$HOME/.avm/bin:$PATH"
source ~/.bashrc
```

### "solana not found"
```bash
export PATH="/home/$(whoami)/.local/share/solana/install/active_release/bin:$PATH"
source ~/.bashrc
```

### "Port 3000 in use"
```bash
lsof -ti:3000 | xargs kill -9
npm start
```

### "Airdrop failed"
Visit: https://faucet.solana.com
Paste address from: `solana address`

---

## ⚡ All Commands in One Block

Copy entire block jika fresh install:

```bash
# Prerequisites
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh && \
source $HOME/.cargo/env && \
sh -c "$(curl -sSfL https://release.solana.com/stable/install)" && \
export PATH="/home/$(whoami)/.local/share/solana/install/active_release/bin:$PATH" && \
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force && \
avm install 0.29.0 && \
avm use 0.29.0

# Wallet setup
solana-keygen new --outfile ~/.config/solana/id.json && \
solana config set --url devnet && \
solana airdrop 2

# Build & deploy (run from verdana-mvp directory)
anchor build && \
echo "PROGRAM ID:" && \
solana address -k target/deploy/verdana_protocol-keypair.json

# After updating program ID in 3 files:
anchor build && \
anchor deploy --provider.cluster devnet && \
cp target/idl/verdana_protocol.json app/src/idl/

# Frontend (in new terminal)
cd app && \
npm install && \
npm start
```

---

## 📝 Files to Update After Getting Program ID

**File 1: Anchor.toml**
```toml
[programs.devnet]
verdana_protocol = "YOUR_PROGRAM_ID"
```

**File 2: programs/verdana-protocol/src/lib.rs**
```rust
declare_id!("YOUR_PROGRAM_ID");
```

**File 3: app/src/idl/verdana_protocol.json**
```json
"metadata": {
  "address": "YOUR_PROGRAM_ID"
}
```

---

## 🔍 Verification Commands

```bash
# Check installations
rustc --version
solana --version
anchor --version
node --version

# Check wallet
solana address
solana balance

# Check program
solana program show YOUR_PROGRAM_ID --url devnet

# Check frontend
curl http://localhost:3000
```

---

## 🚀 Deployment Commands

### Deploy Frontend to Vercel
```bash
cd app
npm run build
npm i -g vercel
vercel
```

### Update Program
```bash
anchor build
anchor upgrade target/deploy/verdana_protocol.so --program-id YOUR_PROGRAM_ID --provider.cluster devnet
```

---

## 💡 Cursor Shortcuts

```
Cmd+P          # Quick file open
Cmd+Shift+P    # Command palette
Cmd+`          # Toggle terminal
Cmd+K          # AI code edit
Cmd+L          # AI chat
Cmd+B          # Toggle sidebar
Cmd+D          # Multi-select
Cmd+/          # Comment line
```

---

**Print this cheat sheet or keep it open while working!** 📋
