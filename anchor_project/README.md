# Spliter Anchor Program

## 📦 Prerequisites
Make sure you have installed:
- [Rust](https://www.rust-lang.org/tools/install)
- [Solana CLI](https://docs.solana.com/cli/install-solana-cli)  
- [Anchor CLI](https://book.anchor-lang.com/getting_started/installation.html)  
- Node.js (LTS) + Yarn or NPM  

Check versions:
```bash
solana --version
anchor --version
rustc --version

```

## Installation
```bash
yarn install
or
npm install
```

## Building the program
```bash
anchor build
```

## Testing the program
```bash
anchor test
```

## NOTES: 
- Program ID is set in lib.rs via declare_id!().
- The generated IDL and types are what a frontend would use to interact with this program.