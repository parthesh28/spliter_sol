# Spliter dApp (Frontend)

Live Demo: [splitersol.vercel.app](https://splitersol.vercel.app)  

---

## 📖 Overview
Spliter is a Solana dApp frontend built with **Next.js**, **create-solana-dapp**, and **Anchor**.  
It connects to the Spliter Anchor program on devnet and allows users to manage group payment splits transparently.

---

## ✨ Features
- **Create Splits** → as an authority, define contributors, receiver, and target amount.  
- **Contribute to Splits** → pay your share if you’re part of a split.  
- **Release Splits** → authority can release funds to the receiver once all contributions are made.  
- **Browse All Splits** on the homepage, with:  
  - 🔍 **Search** → search splits by `split_name`.  
  - 🎛 **Filters**:  
    - By **payment status**: Paid / Unpaid.  
    - By **release status**: Released / Unreleased.  
    - By **role**: Creator / Contributor / Both.  

This makes it easy for a user to quickly find the exact split they’re involved in.

---

## 📦 Prerequisites
- Node.js (LTS recommended)  
- Yarn or NPM  
- A Solana wallet (Phantom, Solflare, Backpack, etc.)  

---

## ⚙️ Setup & Installation
Clone the repo and install dependencies:
```bash
git clone https://github.com/<your-repo>/spliter-frontend.git
cd spliter-frontend
yarn install

```

## Run the test server
```bash
yarn dev
or 
npm run dev
```