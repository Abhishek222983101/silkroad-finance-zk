![SilkRoad Architecture](https://raw.githubusercontent.com/Abhishek222983101/silkroad-finance-zk/main/public/arch.png)

<div align="center">
  <h1>SilkRoad Finance</h1>
  <h3>The First Stateless RWA Privacy Layer on Solana</h3>
  <p>
    <strong>Winner Target: Light Protocol Track (ZK Compression) & Helius Track</strong>
  </p>
  <p>
    <a href="https://silkroad-finance-zk.vercel.app">Live Demo</a> |
    <a href="https://www.youtube.com/watch?v=5LfyOQW3sJ8">Video Pitch</a> |
    <a href="#-technical-architecture">Architecture</a>
  </p>
</div>

---

## Executive Summary

**SilkRoad Finance** is a decentralized invoice factoring protocol built to bridge the gap between traditional supply chain finance and Web3 liquidity, **without compromising privacy**.

Global suppliers often wait **60-90 days** to get paid (the "Cash Trap"). While they want to factor these invoices on-chain for instant liquidity, they cannot afford **"Radical Transparency."** Revealing client names and deal sizes on a public ledger lets competitors underbid them.

**SilkRoad solves this.** We use **ZK Compression (Light Protocol)** to tokenize Real World Assets (RWA) as private, compressed state on Solana. This allows suppliers to access liquidity instantly while keeping their commercial data cryptographically concealed.

---

## Demo & Walkthrough

[![Watch the Video](https://img.youtube.com/vi/5LfyOQW3sJ8/0.jpg)](https://www.youtube.com/watch?v=5LfyOQW3sJ8)

> *Click the image above to watch the 4-minute technical walkthrough.*

---

## Problem & Solution

### The Problem: The Transparency Paradox

* **The Leak:** If a supplier mints an invoice for "Tesla" worth "$500,000" on a public chain, their competitors see it immediately.
* **The Cost:** Standard Solana Token Accounts cost **~0.02 SOL** in rent. For high-volume supply chains (thousands of invoices), this creates significant friction.
* **The Trust Gap:** Web2 solutions are private but centralized (banks). Web3 solutions are trustless but public.

### The Solution: Stateless ZK Privacy

SilkRoad provides a **Stateless** marketplace where assets are compressed and private by default.

* **Zero-Knowledge Privacy:** Invoice metadata is hashed locally (`SHA256`). The chain only sees a validity proof, never the raw data.
* **100x Lower Costs:** By using Light Protocol's **State Merkle Trees**, minting an asset costs **~0.001 SOL** (Rent-Exempt).
* **Atomic Settlement:** Liquidity swaps happen in a single transaction.

---

## Technical Architecture (The "Secret Sauce")

SilkRoad pushes the boundaries of Solana by moving state management to the client-side (Stateless Architecture).

### 1. ZK Compression Engine (Light Protocol)

Instead of standard Anchor accounts, we interact with the **Light System Program**.

* **Client-Side Hashing:** We generate a deterministic hash of the invoice data (Client + Amount + Salt) in the browser.
* **State Compression:** This hash is stored as a "leaf" in a Concurrent Merkle Tree on-chain.
* **Result:** The asset exists on Solana, but its contents are known only to the holder of the decryption key.

### 2. High-Performance RPC (Helius)

To enable a stateless frontend, we rely on **Helius RPCs** as our indexer.

* `rpc.getCompressedAccountProof`: Fetches the Merkle proof needed to modify or transfer the asset.
* `rpc.getStateTreeInfos`: Syncs the latest state root for valid ZK proofs.
* This removes the need for a centralized backend database.

### 3. Atomic Compliance Layer (Simulation)

We implement a "Check-then-Swap" flow. Before settlement, the protocol validates the Buyer's credentials (KYC/Accredited Status) via a simulation loop, ensuring regulatory compliance before the swap executes.

---

## Tech Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Privacy Engine** | **Light Protocol** | `Stateless.js` SDK & ZK Compression |
| **RPC Provider** | **Helius** | Compression-enabled RPC nodes |
| **Frontend** | **Next.js 16** | React Framework with App Router |
| **Styling** | **Tailwind CSS** | Responsive UI components |
| **Blockchain** | **Solana Devnet** | Network layer |

---

## How to Run Locally

Follow these steps to run the ZK Minting engine on your local machine.

### Prerequisites

- Node.js (v18+)
- Yarn
- A Helius API Key (Devnet)

### Installation

1. **Clone the Repo**
   ```bash
   git clone https://github.com/Abhishek222983101/silkroad-finance-zk.git
   cd silkroad-finance-zk
   ```

2. **Install Dependencies**
   ```bash
   yarn install
   ```

3. **Configure Environment**
   Create a `.env.local` file in the root directory:
   ```bash
   echo "NEXT_PUBLIC_HELIUS_RPC=https://devnet.helius-rpc.com/?api-key=YOUR_KEY" > .env.local
   ```

4. **Run the App**
   ```bash
   yarn dev
   ```

Open `http://localhost:3000` to start minting private assets.

---

## Roadmap: Road to Mainnet

The current version is a fully functional **Devnet Prototype** demonstrating the core ZK primitives. The path to Mainnet involves:

1. **Pay-to-Reveal Logic:** Implementing an atomic encryption scheme where the "Buy" transaction triggers the secure release of the PDF decryption key to the buyer.

2. **Fiat Oracle Integration:** Automating the exit liquidity flow by listening for off-chain bank wires (e.g., from the Debtor) to burn the asset and release stablecoins.

3. **On-Chain Identity:** Replacing the frontend compliance check with **Civic/Reclaim** Verifiable Credentials for strict institutional whitelisting.

---

<div align="center">
  <p>Built with mass mass mass mass by Abhishek Tiwari</p>
  <p>Solana Privacy Hackathon 2026</p>
</div>
