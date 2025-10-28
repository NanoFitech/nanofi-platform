# 🌿 Verdana Protocol MVP

A Web3 sustainability funding platform built on Solana.

## 📋 Project Overview

Verdana Protocol is a decentralized platform that connects sustainability projects with backers on the Solana blockchain. Projects can raise funds, track milestones, and receive impact tokens for their environmental contributions.

## ✨ Features

- **Project Creation**: Create and manage sustainability projects across 6 categories
- **Funding System**: Transparent funding with real-time progress tracking
- **Milestone Tracking**: Add and complete project milestones
- **Impact Tokens**: Earn tokens for contributions
- **Low Fees**: Ultra-low transaction costs on Solana
- **Fast Confirmations**: Sub-second transaction confirmation

## 🏗️ Project Structure

```
verdana-mvp/
├── programs/
│   └── verdana-protocol/
│       └── src/
│           └── lib.rs              # Solana program
├── app/
│   ├── src/
│   │   ├── App.js                  # Main React app
│   │   ├── App.css                 # Styling
│   │   └── idl/
│   │       └── verdana_protocol.json
│   └── package.json
├── tests/
│   └── verdana-protocol.ts         # Tests
├── Anchor.toml                     # Anchor config
└── README.md                       # This file
```

## 🚀 Quick Start

### Prerequisites

- Rust (latest stable)
- Solana CLI 2.0.0+
- Anchor 0.29.0+
- Node.js v18+

### Installation

1. **Install Rust** (if not already installed):
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

2. **Install Solana CLI**:
```bash
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"
```

3. **Install Anchor**:
```bash
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
avm install 0.29.0
avm use 0.29.0
```

4. **Install Node.js dependencies**:
```bash
cd app
npm install
cd ..
```

### Configuration

1. **Generate a wallet**:
```bash
solana-keygen new --outfile ~/.config/solana/id.json
```

2. **Set to devnet**:
```bash
solana config set --url devnet
```

3. **Airdrop SOL** (for testing):
```bash
solana airdrop 2
```

### Build & Deploy

1. **Build the program**:
```bash
anchor build
```

2. **Get program ID**:
```bash
solana address -k target/deploy/verdana_protocol-keypair.json
```

3. **Update program ID** in 3 files:
   - `Anchor.toml` (line 5)
   - `programs/verdana-protocol/src/lib.rs` (line 8)
   - `app/src/idl/verdana_protocol.json` (last line)

4. **Rebuild and deploy**:
```bash
anchor build
anchor deploy --provider.cluster devnet
```

5. **Copy IDL to frontend**:
```bash
cp target/idl/verdana_protocol.json app/src/idl/
```

### Run Frontend

```bash
cd app
npm start
```

Open http://localhost:3000 in your browser.

## 📖 Usage

### Creating a Project

1. Connect your Phantom wallet
2. Navigate to "Create Project"
3. Fill in the form:
   - Project name
   - Description
   - Category (Plastic Recycling, Carbon Offset, etc.)
   - Goal amount in SOL
   - Optional image URL
4. Click "Create Project" and approve the transaction

### Funding a Project

1. Browse projects from the home page
2. Click "Fund Project" on any project
3. Enter the amount you want to contribute
4. Approve the transaction in your wallet

### Managing Milestones

1. Project creators can add milestones
2. Track progress as milestones are completed
3. Impact tokens are distributed based on contributions

## 🎯 Categories

- ♻️ Plastic Recycling
- 🌳 Carbon Offset
- ⚡ Renewable Energy
- 🌲 Reforestation
- 💧 Water Conservation
- 🔹 Other

## 🛠️ Development

### Testing

```bash
anchor test
```

### Building

```bash
anchor build
```

### Deployment

Deploy to devnet:
```bash
anchor deploy --provider.cluster devnet
```

Deploy to mainnet:
```bash
anchor deploy --provider.cluster mainnet
```

## 📝 Documentation

- [Command Cheatsheet](COMMAND_CHEATSHEET.md)
- [Setup Guide](CURSOR_SETUP_GUIDE.md)
- [Project Overview](VERDANA_MVP_OVERVIEW.md)
- [Visual Diagrams](VISUAL_DIAGRAMS.md)

## 🔧 Troubleshooting

### "solana: command not found"
```bash
export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"
```

### "anchor: command not found"
```bash
export PATH="$HOME/.avm/bin:$PATH"
```

### Port 3000 already in use
```bash
lsof -ti:3000 | xargs kill -9
```

### Airdrop failed
Visit https://faucet.solana.com or use:
```bash
solana airdrop 2 YOUR_ADDRESS --url devnet
```

## 📄 License

This project is part of a hackathon submission. All rights reserved.

## 🤝 Contributing

This is an MVP project. Contributions and improvements are welcome!

## 📞 Support

- Solana Discord: https://discord.gg/solana
- Anchor Discord: https://discord.gg/PDeRXyVURd

---

**Built with 🌍 for a sustainable future on Solana**

