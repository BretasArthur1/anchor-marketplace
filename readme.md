# Anchor Marketplace

An open NFT marketplace built on Solana using Anchor.

## Overview

Anchor Marketplace allows users to:
- List their NFTs for sale by specifying a price 
- Purchase NFTs that others have listed
- Cancel their active listings

Under the hood, it uses Metaplex's Token Metadata program to validate NFT ownership and ensure only verified creators can list their NFTs.

When an NFT is listed, it is transferred into an escrow account owned by the marketplace until it is either purchased or the listing is cancelled. The marketplace takes a small fee on each sale.

## Setup

### Prerequisites

- Solana CLI
- Anchor CLI
- Yarn 
- NodeJS v14+

### Install 

1. Clone the repo:
```
git clone https://github.com/username/anchor-marketplace.git
```

2. Install JavaScript dependencies:
```
yarn install
```

3. Install Rust dependencies:
```
anchor build
```

## Usage

### Deployment

To deploy the marketplace program:

```
anchor deploy
```

### Initialization

To initialize a new marketplace named "My Marketplace" with a 5% fee:

```typescript
await program.methods.initialize("My Marketplace", 500).rpc();
```

### Listing

To list an NFT for 1 SOL:

```typescript
await program.methods.list(1000000000).accounts({
  maker: wallet.publicKey,
  makerMint: nftMint,
  makerAta: nftAta,
  vault: vaultPda,
  listing: listingPda,
  metadata: metadataPda,
  masterEdition: masterEditionPda,
}).rpc();
```

### Purchasing

To purchase a listed NFT:

```typescript
await program.methods.purchase().accounts({
  taker: wallet.publicKey,
  takerAta: takerAtaPda,
  maker: listing.maker,
  makerMint: listing.mint,
  vault: vaultPda,
  listing: listingPda,
  treasury: treasuryPda,
}).rpc();  
```

### Cancelling Listing

To cancel an active listing and reclaim the NFT:

```typescript
await program.methods.delist().accounts({
  maker: wallet.publicKey,
  makerMint: listing.mint,
  makerAta: makerAta,
  vault: vaultPda,
  listing: listingPda,
}).rpc();
```

## Testing

Unit tests are located in the `tests/` directory. To run them:

```
anchor test
```

## License

This project is licensed under the ISC License.
