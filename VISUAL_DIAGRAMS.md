# 📊 Verdana Protocol - Visual Architecture & Flow

ASCII diagrams untuk memahami struktur dan flow project.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         USERS                                │
│  (Project Owners, Funders, Verifiers, Community)           │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│                    FRONTEND (React)                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │Dashboard │  │  Create  │  │  Browse  │  │   Fund   │   │
│  │          │  │  Project │  │ Projects │  │  Project │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │         Wallet Adapter (Phantom/Solflare)            │  │
│  └──────────────────────────────────────────────────────┘  │
└────────────────────┬────────────────────────────────────────┘
                     │ (Web3.js / Anchor)
                     ▼
┌─────────────────────────────────────────────────────────────┐
│                   SOLANA BLOCKCHAIN                          │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │            Verdana Protocol Program                   │  │
│  │                                                       │  │
│  │  ┌─────────────────┐    ┌─────────────────┐        │  │
│  │  │ Protocol State  │    │     Projects    │        │  │
│  │  │  - Total Stats  │    │  - Metadata     │        │  │
│  │  │  - Admin        │    │  - Funding      │        │  │
│  │  └─────────────────┘    │  - Milestones   │        │  │
│  │                          └─────────────────┘        │  │
│  │                                                       │  │
│  │  ┌─────────────────┐    ┌─────────────────┐        │  │
│  │  │   Milestones    │    │  Impact Tokens  │        │  │
│  │  │  - Progress     │    │  - CCT, RPT     │        │  │
│  │  │  - Verification │    │  - Tradeable    │        │  │
│  │  └─────────────────┘    └─────────────────┘        │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔄 Transaction Flow

### 1️⃣ Initialize Protocol

```
┌─────────┐         ┌──────────┐         ┌────────────┐
│  Admin  │────────▶│ Frontend │────────▶│   Solana   │
└─────────┘         └──────────┘         └────────────┘
    │                     │                      │
    │ Click              │ Call                 │
    │ "Initialize"       │ initialize()         │
    │                     │                      │
    │                     │                      ▼
    │                     │              ┌────────────┐
    │                     │              │   Create   │
    │                     │              │  Protocol  │
    │                     │              │   State    │
    │                     │              └────────────┘
    │                     │                      │
    │◀────────────────────│◀─────────────────────┘
    │ Transaction         │ Success
    │ Confirmed          │
```

### 2️⃣ Create Project

```
┌────────────┐     ┌──────────┐     ┌────────────┐
│Project     │────▶│ Frontend │────▶│   Solana   │
│Owner       │     └──────────┘     └────────────┘
└────────────┘           │                 │
    │                    │                 │
    │ Fill Form          │ Call            │
    │ (name, desc,       │ create_project()│
    │  category, goal)   │                 │
    │                    │                 ▼
    │                    │          ┌────────────┐
    │                    │          │   Create   │
    │                    │          │  Project   │
    │                    │          │  Account   │
    │                    │          └────────────┘
    │                    │                 │
    │                    │                 │ Store:
    │                    │                 │ - Owner
    │                    │                 │ - Details
    │                    │                 │ - Goal
    │                    │                 │
    │◀───────────────────│◀────────────────┘
    │ Project Created!   │ Return PDA
```

### 3️⃣ Fund Project

```
┌────────────┐     ┌──────────┐     ┌────────────┐
│  Funder    │────▶│ Frontend │────▶│   Solana   │
└────────────┘     └──────────┘     └────────────┘
    │ (has SOL)          │                 │
    │                    │                 │
    │ Select Project     │ Call            │
    │ Enter Amount       │ fund_project()  │
    │                    │                 │
    │                    │                 ▼
    │                    │          ┌────────────┐
    │                    │          │  Transfer  │
    │                    │          │    SOL     │
    │                    │          └────────────┘
    │                    │                 │
    │                    │                 ▼
    │                    │          ┌────────────┐
    │                    │          │   Update   │
    │                    │          │  Project   │
    │                    │          │  Funding   │
    │                    │          └────────────┘
    │                    │                 │
    │◀───────────────────│◀────────────────┘
    │ Transaction        │ Success
    │ Confirmed          │
    │                    │
    │ View Updated       │
    │ Progress ──────────│
```

---

## 🎯 Project Lifecycle

```
START
  │
  ▼
┌─────────────────┐
│  1. CREATE      │  Project Owner registers project
│     PROJECT     │  - Name, description, category
└────────┬────────┘  - Funding goal, metadata
         │
         ▼
┌─────────────────┐
│  2. RECEIVE     │  Anyone can fund the project
│     FUNDING     │  - SOL transferred to escrow
└────────┬────────┘  - Progress tracked on-chain
         │
         ▼
┌─────────────────┐
│  3. COMPLETE    │  Project owner reports progress
│   MILESTONES    │  - Verifiers check completion
└────────┬────────┘  - Funds released gradually
         │
         ▼
┌─────────────────┐
│  4. MINT        │  Environmental impact verified
│  IMPACT TOKENS  │  - Tokens minted as proof
└────────┬────────┘  - Can be traded/sold
         │
         ▼
┌─────────────────┐
│  5. WITHDRAW    │  Project owner withdraws funds
│     FUNDS       │  - Based on milestones
└────────┬────────┘  - Transparent tracking
         │
         ▼
       END
```

---

## 💾 Data Structure

```
ProtocolState
├── admin: Pubkey
├── total_projects: u64
├── total_funding: u64
└── bump: u8

Project
├── owner: Pubkey
├── name: String (max 50)
├── description: String (max 200)
├── category: Enum
│   ├── PlasticRecycling
│   ├── CarbonOffset
│   ├── RenewableEnergy
│   ├── Reforestation
│   ├── WaterConservation
│   └── Other
├── funding_goal: u64
├── current_funding: u64
├── milestone_count: u32
├── is_active: bool
├── metadata_uri: String
├── created_at: i64
└── bump: u8

Milestone
├── project: Pubkey
├── title: String (max 50)
├── target_amount: u64
├── is_completed: bool
├── completed_at: i64
└── bump: u8
```

---

## 🔐 Security Model

```
┌─────────────────────────────────────────────┐
│          PDA (Program Derived Address)      │
│                                             │
│  Seeds-based deterministic addresses        │
│  - No private key needed                    │
│  - Only program can sign                    │
│  - Collision-resistant                      │
└─────────────────────────────────────────────┘
                    │
        ┌───────────┼───────────┐
        │           │           │
        ▼           ▼           ▼
┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│  Protocol   │ │   Project   │ │  Milestone  │
│    State    │ │             │ │             │
│             │ │             │ │             │
│ Seeds:      │ │ Seeds:      │ │ Seeds:      │
│ ["protocol"]│ │ ["project"] │ │ ["milestone"│
│             │ │ [owner]     │ │  project]   │
│             │ │ [name]      │ │  [index]    │
└─────────────┘ └─────────────┘ └─────────────┘
```

---

## 📱 Frontend Component Tree

```
App
├── AnchorProvider (Context)
│   └── Provides: program, wallet, connection
│
├── Header
│   ├── Logo
│   ├── Navigation
│   └── WalletMultiButton
│
└── Main Content (Route-based)
    │
    ├── Dashboard
    │   ├── Stats Cards
    │   │   ├── Total Projects
    │   │   ├── Total Funding
    │   │   └── User Projects
    │   ├── Action Cards
    │   │   ├── Create Project
    │   │   └── Browse Projects
    │   └── Features Grid
    │
    ├── CreateProject
    │   ├── Form
    │   │   ├── Name Input
    │   │   ├── Description Textarea
    │   │   ├── Category Select
    │   │   ├── Funding Goal Input
    │   │   └── Metadata URI Input
    │   └── Submit Button
    │
    ├── ProjectList
    │   ├── Filters
    │   │   └── Category Buttons
    │   └── Project Cards
    │       ├── Header (Category + Status)
    │       ├── Name & Description
    │       ├── Stats (Goal + Raised)
    │       ├── Progress Bar
    │       └── Fund Button
    │
    └── FundProject
        ├── Project Details Card
        │   ├── Info
        │   ├── Funding Stats
        │   └── Progress Bar
        └── Funding Form Card
            ├── Amount Input
            ├── Quick Amounts
            └── Fund Button
```

---

## ⚡ Transaction Cost Breakdown

```
Transaction Type       │ Compute Units │ Lamports │ USD (est)
──────────────────────────────────────────────────────────
Initialize Protocol    │   ~5,000      │  ~5,000  │ $0.001
Create Project         │  ~20,000      │ ~20,000  │ $0.004
Fund Project          │  ~10,000      │ ~10,000  │ $0.002
Add Milestone         │  ~15,000      │ ~15,000  │ $0.003
Complete Milestone    │   ~8,000      │  ~8,000  │ $0.002
Mint Impact Tokens    │  ~12,000      │ ~12,000  │ $0.002
Withdraw Funds        │  ~10,000      │ ~10,000  │ $0.002
──────────────────────────────────────────────────────────
TOTAL (Full Flow)     │  ~80,000      │ ~80,000  │ $0.016

vs Ethereum Gas: ~$20-50 per transaction 💰
```

---

## 🌐 Network Comparison

```
Feature              │  Solana    │  Ethereum  │  Winner
─────────────────────────────────────────────────────────
Transaction Speed    │  400ms     │  15-60s    │  Solana ⚡
Transaction Cost     │  $0.00025  │  $2-50     │  Solana 💰
Throughput (TPS)     │  65,000    │  15-30     │  Solana 🚀
Block Time           │  400ms     │  12s       │  Solana ⏱️
Finality            │  <1s       │  ~13min    │  Solana ✅
Energy Efficiency   │  High      │  Medium    │  Solana 🌱
──────────────────────────────────────────────────────────
Best for:           │ High-freq  │ Complex    │
                    │ micro-tx   │ DeFi       │
```

**Verdict**: Solana perfect untuk sustainability platform! 🌿

---

## 📊 Project Categories & Use Cases

```
┌─────────────────────────────────────────────────────┐
│              VERDANA PROTOCOL                       │
└─────────────────────────────────────────────────────┘
            │
            ├─── ♻️  Plastic Recycling
            │     ├─ Ocean cleanup
            │     ├─ Beach cleanups
            │     └─ Recycling facilities
            │
            ├─── 🌳  Carbon Offset
            │     ├─ Reforestation
            │     ├─ Mangrove planting
            │     └─ Carbon capture
            │
            ├─── ⚡  Renewable Energy
            │     ├─ Solar installations
            │     ├─ Wind farms
            │     └─ Hydro projects
            │
            ├─── 🌲  Reforestation
            │     ├─ Tree planting
            │     ├─ Forest conservation
            │     └─ Habitat restoration
            │
            ├─── 💧  Water Conservation
            │     ├─ Clean water access
            │     ├─ Irrigation systems
            │     └─ Water treatment
            │
            └─── 🔹  Other
                  ├─ Biodiversity
                  ├─ Education
                  └─ Research
```

---

**Use these diagrams in your presentation! 📊**
