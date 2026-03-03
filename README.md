# Project Description

**Deployed Frontend URL:** https://splitersol.vercel.app

**Solana Program ID:** `sPitpiqrhcuAgu8Ss9Bv2YEpkYhnKdDQeSYW65DSMcd`

## Project Overview

### Description
Spliter is a decentralized application built on Solana that enables transparent and automated payment splitting among multiple contributors. Users can create payment splits for shared expenses like group trips, events, or joint purchases. The app allows one wallet (the split authority) to define contributors with specific percentage shares, and contributors can pay their owed amounts directly into the split account. Once all contributions are collected, the split authority can release the funds to the designated receiver, bringing transparency and automation to shared financial arrangements.

### Key Features
- **Create Split**: Initialize payment splits with multiple contributors and percentage shares
- **Contribute to Split**: Contributors pay their exact share with real-time status tracking
- **Release Payment**: Automatic fund release to receiver once all contributions are collected
- **History & Transparency**: Complete visibility into split status, contributions, and payment releases
- **Percentage-based Splitting**: Flexible contribution system based on percentage shares

### How to Use the dApp
1. **Connect Wallet** - Use Phantom, Solflare, or any supported Solana wallet to connect
2. **Create a Split** (Split Authority) - Enter split name, receiver's wallet, total amount, and contributors with percentage shares
3. **Contribute to Split** (Contributor) - View splits you're part of and pay your owed share to change status to "Cleared"
4. **Release Split** (Authority) - When all contributions are collected, click "Release" to transfer funds to the receiver
5. **Track Progress** - Monitor contribution status and payment history for transparency

## Program Architecture
The Spliter dApp uses a sophisticated architecture centered around Program Derived Addresses (PDAs) to manage split accounts and ensure secure, transparent payment processing. The program handles complex state management for multiple contributors while maintaining data integrity and preventing unauthorized access.

### PDA Usage
The program uses Program Derived Addresses to create unique, deterministic split accounts that provide security and prevent conflicts.

**PDAs Used:**
- **Split PDA**: Derived from seeds `["SPLIT_SEED", receiver_pubkey, hash(split_name)]` - creates unique split accounts that store all split state including contributors, amounts, and payment status

### Program Instructions
**Instructions Implemented:**
- **Create**: Initializes a new split PDA with contributors, validates percentage shares sum to 100%, and stores receiver and total amount information
- **Contribute**: Allows contributors to pay their owed share, marks them as cleared with timestamp, and updates received amount
- **Release**: Enables split authority to transfer collected funds to receiver once all contributions are completed, with release timestamp tracking

### Account Structure
```rust
#[account]
pub struct Split {
   pub split_authority: Pubkey,    // Wallet that created and manages the split
   pub split_name: String,         // Unique identifier for the split
   pub receiver: Pubkey,           // Wallet that receives the final collected funds
   pub split_amount: u64,          // Total target amount to be collected
   pub contributors: Vec<Spliter>, // List of contributors with their details
   pub received_amount: u64,       // Current amount collected from contributors
   pub is_released: bool,          // Whether funds have been released to receiver
   pub released_at: i64,           // Unix timestamp when funds were released
   pub bump: u8,                   // PDA bump seed for account derivation
}

#[derive(AnchorSerialize, AnchorDeserialize, Clone, Copy)]
pub struct Spliter {
   pub contributor: Pubkey,        // Contributor's wallet address
   pub percent: u8,                // Percentage share this contributor owes
   pub has_cleared: bool,          // Whether contributor has paid their share
   pub cleared_at: i64,           // Unix timestamp when contribution was made
}

impl Space for Split {
   const INIT_SPACE: usize = 32 + 32 + 8 + 8 + 1 + 1 + 8;
}

```

## Testing

### Test Coverage
Comprehensive test suite covering all instructions with both successful operations and error conditions to ensure program security and reliability across complex multi-contributor scenarios.

**Happy Path Tests:**
- **Create Split**: Successfully creates splits with valid contributors and percentage allocations
- **Contribute**: Allows valid contributors to pay their exact share and updates status correctly
- **Release Payment**: Enables split authority to release funds to receiver after all contributions are complete

**Unhappy Path Tests:**
- **Create Validation**: Fails when contributor percentages don't sum to 100%, with duplicate contributors, no contributors, or 0% contributions
- **Contribute Authorization**: Prevents invalid contributors from contributing and stops contributors from paying multiple times
- **Release Authorization**: Fails when non-authority tries to release, wrong receiver account is used, or attempting to release multiple times

### Running Tests
```bash
yarn install    # install dependencies
anchor test     # run comprehensive test suite
```

### Additional Notes for Evaluators

This was my first Solana smart contract and also the first time I built something end-to-end with proper thought about state design, PDA handling, and testing. Here are some key reflections from the development journey:

**Understanding PDAs**: Initially confused about when and why to use PDAs, I started by storing contributors directly in a vector, then learned about scalability considerations. For this MVP, I kept contributors inline for simplicity, but gained valuable insights about when independent PDAs would be beneficial.

**State Design Evolution**: My `Split` struct went through multiple iterations - adding `is_released`, `released_at`, and contributor fields like `has_cleared` and `cleared_at` taught me that on-chain history tracking is crucial, not just basic functionality.

**Technical Challenges**: Encountered the "account with data cannot transfer lamports" issue when attempting CPI transfers from PDAs. Solving this taught me proper lamport balance handling and signer seed management - a painful but essential learning experience. Also learned about memcmp for efficiently fetching specific PDA accounts.

**Testing Philosophy**: Writing comprehensive tests forced me to consider both happy path scenarios and edge cases. Organizing tests by instruction (`create`, `contribute`, `release`) clarified potential failure points and made debugging systematic and rewarding.

**Development Insights**: This project demonstrated that Solana development requires careful state design, thorough edge case consideration, and thoughtful user experience planning. I learned to balance "scalable architecture" goals with practical MVP delivery, gaining valuable experience in real-world blockchain development constraints and best practices.