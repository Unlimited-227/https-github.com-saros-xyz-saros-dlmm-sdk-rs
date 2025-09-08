# Saros SDK Developer Documentation

## 1. Introduction
Saros is a Solana-native DeFi super-app offering:
- AMM-based swaps  
- Liquidity & farming  
- Staking DLMM (Dynamic Liquidity Market Maker) pools  

The Saros SDKs give you everything you need to integrate DeFi primitives into your dApp with minimal boilerplate.

**SDKs available:**
- `@saros-finance/sdk` – Core AMM functionality: swaps, liquidity, farms, staking.  
- `@saros-finance/dlmm-sdk` – DLMM engine: concentrated liquidity pools and advanced trading strategies.  

---

## 2. Quickstart Guide: AMM SDK
Clone the [SarosSwap SDK](https://github.com/saros-xyz/saros-sdk.git) into your desired environment.

**Installation:**
```bash
yarn add @saros-finance/sdk
# OR
npm install @saros-finance/sdk
````

**Example: Token Swap**

```ts
import { genConnectionSolana, swapSaros } from "@saros-finance/sdk";
import { PublicKey } from "@solana/web3.js";

// 1. Connect to Solana
const connection = genConnectionSolana();

// 2. Perform a swap
async function main() {
  const result = await swapSaros(
    connection,
    fromTokenAccount,
    toTokenAccount,
    100_000,
    0,
    null,
    new PublicKey("POOL_ADDRESS"),
    "SAROS_SWAP_PROGRAM_ADDRESS_V1",
    accountPubKey,
    fromMint,
    toMint
  );
  console.log("Swap complete:", result);
}
main();
```

---

## 3. Integration Tutorials

### A. Swapping Tokens

1. Install the SDK:

```bash
npm install @saros-finance/sdk
```

2. Generate a Solana connection:

```ts
const connection = genConnectionSolana("devnet");
```

3. Call `swapSaros()` with pool address, mints and wallet.
4. Submit the transaction.
5. View result on Explorer.

---

### B. Providing Liquidity

```ts
import { addLiquidity } from "@saros-finance/sdk"; 

// Pass pool details and token accounts
// Confirm liquidity addition
// Receive LP tokens
```

---

### C. Staking LP Tokens

```ts
import { stakeSaros } from "@saros-finance/sdk";

// Pass LP token account and farm ID/public key
// Track rewards with getStakingInfo()
```

---

## 4. Code Examples

### A. Swap on Devnet

```ts
// Import and call the Saros AMM swap function
const result = await swapSaros(
  connection, fromAcc, toAcc, 100_000, 0, null,
  pool, programId, wallet, fromMint, toMint
);

// Print transaction result
console.log("Devnet swap result:", result);
```

---

### B. Add Liquidity

```ts
// Add liquidity to a Saros pool (earn LP tokens)
const lpResult = await addLiquidity(
  connection, Pool, userAccount, mintA, mintB, amountA, amountB
);

// Output LP token result (transaction + balance update)
console.log("Liquidity provided:", lpResult);
```

---

### C. DLMM Quote & Swap

```ts
import { LiquidityBookServices, MODE } from "@saros-finance/dlmm-sdk";
import { PublicKey } from "@solana/web3.js";

// Initialize DLMM services on Devnet
const services = new LiquidityBookServices({ mode: MODE.DEVNET });

// Request a price quote from a DLMM pool
const quote = await services.getQuote({
  amount: BigInt(1_000_000),
  isExactInput: true,
  swapForY: true,
  pair: new PublicKey(pool),
  tokenBase: baseMint,
  tokenQuote: quoteMint,
  tokenBaseDecimal: 6,
  tokenQuoteDecimal: 6,
  slippage: 0.5,
});

// Print the quote
console.log("DLMM quote:", quote);

// (Next step: execute `services.swap()` with this quote to finalize the trade)
```

---

## 5. API References

**AMM SDK Methods:**

* `swapSaros(wallet, connection, toMint, toAcc, fromWallet, fromAcc, amountIn, amountOutMin, referral, pool, programId)`
* `addLiquidity(connection, pool, userAccount, mintA, mintB, amountA, amountB)`
* `stakeSaros(connection, farm, lpTokenAccount, wallet)`

**DLMM SDK Methods:**

* `LiquidityBookServices.getQuote()`
* `LiquidityBookServices.swap()`
* `LiquidityBookServices.getPoolState()`

---

## 6. Developer Experience Analysis

**Pros:**

* Strongly typed TypeScript APIs that can be easily imported without errors
* Strong technical foundation for both basic and advanced workability under Solana ecosystem
* One-of-a-kind on Solana for DLMM with liquidity SDK (backed by Coin98)
* DLMM abstraction similar to Uniswap v3
* Seamless Devnet & Mainnet support

**Cons:**

* No unified starter templates for Hackathon onboarding
* Documentation gaps and lack of uniformity
* Limited example coverage and error handling for failed swaps/high slippage
* No playground/test UI for live experiments
* DLMM docs assume prior AMM knowledge (lack of diagrams for beginners)

**Suggestions:**

* Publish React/Next.js boilerplates
* Add more copy-paste tutorials for staking and farming
* Provide troubleshooting for Solana RPC issues
* Include visual diagrams for DLMM

---

## 7. Troubleshooting & FAQ

**Q:** My transaction fails on Devnet.

**A:** Ensure you are using Devnet pool addresses and have SOL for fees.

**Q:** I get *Blockhash not found.*

**A:** Refresh blockhash with `connection.getLatestBlockhash()`.

**Q:** How do I test DLMM without capital?

**A:** Use Solana Devnet faucets and demo pools.

---

## 8. SDK Comparison Guide

| Feature         | @saros-finance/sdk | @saros-finance/dlmm-sdk |
| --------------- | ------------------ | ----------------------- |
| Token Swaps     | ✅                  | ✅                       |
| Liquidity       | ✅                  | ✅                       |
| Farming/Staking | ✅                  | ❌                       |
| Pool Creation   | ❌                  | ✅                       |
| Ideal For       | MVPs & Hackathons  | Advanced DeFi Protocols |

---

## 9. Visual Aids

* Diagram: AMM vs DLMM liquidity distribution
* Screenshot: Devnet Explorer with Saros transactions
* GIF: Quickstart swap demo in React app

---

## 10. References & Documentation

* GitHub: [https://github.com/saros-xyz/saros-sdk](https://github.com/saros-xyz/saros-sdk)
* Docs: [https://docs.saros.xyz](https://docs.saros.xyz)
* NPM: [https://www.npmjs.com/package/@saros-finance/sdk](https://www.npmjs.com/package/@saros-finance/sdk)
