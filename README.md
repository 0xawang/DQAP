![Domain Auction Banner](https://raw.githubusercontent.com/0xawang/DomaAuction/main/domain-auction-banner.png)

# DomaAuction - Hybrid Dutch Auction Protocol

A next-generation auction system for domain NFTs featuring batch auctions, gamified bidding, and dynamic royalties.

## Features

### 1. Batch Dutch Auctions
- Auction multiple domain NFTs as a portfolio
- Fractional ownership support (buy 10%, 25%, etc.)
- Linear price decay over time
- Reserve price protection

### 2. Gamified Bidding System
- **Soft Bids**: Intent-based bidding with auto-conversion
- **Bonds**: 0.2% refundable deposit prevents spam
- **Loyalty Rewards**: Time-weighted points for early engagement
- **Sale-Gated**: Rewards only distributed on successful auctions

### 3. Reverse Royalty Engine
- Dynamic royalties starting at 0%
- Increases per block to incentivize quick trades
- Optional feature for secondary sales
- Automatic distribution to original creators

## Repositories
- Smart Contracts [https://github.com/0xawang/DomaAuctoin]
- UI/Frontend [https://github.com/0xawang/doma-auction-frontend]

## Deployed Contracts & Websites (On Doma Testnet)

Website: [https://doma-auction-frontend.vercel.app/](https://doma-auction-frontend.vercel.app/)

| Contract Name          | Address                                      |
|------------------------|----------------------------------------------|
| HybridDutchAuction.sol | `0xcf60D335E3aAd5a58c924d43Ce9F65e5557c7716` |
| LoyaltyNFT.sol         | `0xa2E424DBdF826D5B80301B7F4C91cC0a2ff1a8E5` |


## Examples

### Example 1: Batch Portfolio Auction with Gamification

**Setup:**
- Item: 100-domain bundle
- Start price: 1,000 USDC (Dutch, linearly down)
- Reserve floor: 700 USDC
- Reward budget: 1% of final sale, only if cleared
- Bond: 0.2% of intended spend

**Early Phase:**
- Alice: soft bid for 10% of bundle, threshold = 900 → bond posted
- Bob: soft bid 5%, threshold = 860 → bond posted
- Carol: soft bid 40%, threshold = 820 → bond posted
- Dana: soft bid 50%, threshold = 780 → bond posted

**Price Progression:**
- At 900: Alice auto-converts (10%). Cumulative = 10% — continue
- At 860: Bob auto-converts (5%). Cumulative = 15% — continue
- At 820: Carol auto-converts (40%). Cumulative = 55% — continue
- At 780: Dana auto-converts (50%). Cumulative = 105% ≥ 100% → auction clears at 780

**Settlement:**
- Pro-rata fill at clearing price (if over-subscribed)
- Bonds returned
- Rewards minted (since sale cleared):
  - Alice (earliest, highest price distance) gets largest share of points
  - Dana gets less (later threshold), even though she cleared the auction

### Example 2: Single Domain Auction

**Setup:**
- Single premium domain
- First threshold hit auto-converts and ends auction immediately
- Only converting bidders get rewards

### Example 3: Secondary Sale with Reverse Royalty

**Setup:**
- Reselling acquired domains
- No loyalty rewards (rewardBudgetBps = 0)
- Reverse royalty starts at 0%, increases 0.1% per block
- Creates urgency for buyers


### Example 4: Failed Auction (Bond Refunds)

**Setup:**
- Auction fails to reach 100% conversion
- Price hits reserve but insufficient demand
- Bonds are refunded

## Deployment

### Prerequisites
```bash
npm install
```

### Compile
```bash
npx hardhat compile
```

### Deploy to Doma Testnet
```bash
# Set PRIVATE_KEY in .env
cp .env.example .env

# Deploy contracts
npx hardhat run scripts/deploy.js --network doma
```

### Test
```bash
npx hardhat test
```

## Configuration

### Network Settings (hardhat.config.js)
```javascript
networks: {
  doma: {
    url: "https://fraa-dancebox-3043-rpc.a.dancebox.tanssi.network",
    chainId: 3043
  }
}
```

### Contract Addresses
- **Doma OwnershipToken**: `0x424bDf2E8a6F52Bd2c1C81D9437b0DC0309DF90f`

## Key Parameters

- **Bond Rate**: 0.2% of intended spend
- **Max Reward Budget**: 5% of sale proceeds
- **Price Multipliers**: Early bids get 1.5x, late bids get 1.0x
- **Royalty Cap**: No hardcoded limit (market-driven)

## Security Features

- **ReentrancyGuard**: Prevents reentrancy attacks
- **IERC721Receiver**: Safe NFT handling
- **Bond System**: Spam prevention
- **Reserve Price**: Seller protection
- **Overflow Protection**: SafeMath patterns

