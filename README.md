# VoiceChain Protocol

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Stacks](https://img.shields.io/badge/Stacks-Clarity-orange.svg)
![Version](https://img.shields.io/badge/version-1.0.0-green.svg)

> **The Future of Decentralized Social Discourse**

VoiceChain is a next-generation social protocol that transforms how communities engage, create, and monetize content through blockchain-native incentive structures and democratic governance mechanisms.

## 🌟 Overview

VoiceChain reimagines social interaction by leveraging Bitcoin's security foundation to create an economic layer for authentic discourse. The protocol introduces novel mechanisms including reputation-weighted voting, creator monetization through premium content gates, peer-to-peer value streaming via tips, and community-driven content curation.

By requiring economic stake for participation, VoiceChain eliminates spam while ensuring quality discussions flourish through aligned incentives between creators, curators, and consumers of content.

## 🚀 Innovation Highlights

- **Economic Participation Model**: Stake-based entry barriers ensure serious engagement
- **Creator Economy Engine**: Direct monetization paths for premium discussions
- **Reputation-Driven Governance**: Community standing influences content visibility
- **Hierarchical Discussion Trees**: Structured conversations with nested replies
- **Value Distribution Layer**: Transparent tip economy for quality content rewards

## 🏗️ Architecture

### Core Components

#### 1. Thread Registry

Premium content management system that handles discussion topics with monetization capabilities.

```clarity
;; Thread structure with premium content support
{
  author: principal,
  title: (string-utf8 256),
  content: (string-utf8 2048),
  is-premium: bool,
  premium-price: uint,
  created-at: uint,
  upvotes: uint,
  downvotes: uint,
  tips-received: uint,
  is-locked: bool,
  reply-count: uint,
}
```

#### 2. Reply System

Hierarchical discussion architecture supporting nested conversations.

```clarity
;; Reply structure with parent-child relationships
{
  thread-id: uint,
  author: principal,
  content: (string-utf8 1024),
  created-at: uint,
  upvotes: uint,
  downvotes: uint,
  tips-received: uint,
  parent-reply-id: (optional uint),
}
```

#### 3. Reputation Engine

Community standing metrics that influence governance and content visibility.

```clarity
;; Reputation scoring system
{
  total-upvotes: uint,
  total-downvotes: uint,
  threads-created: uint,
  replies-created: uint,
  tips-sent: uint,
  tips-received: uint,
  staked-amount: uint,
  reputation-score: uint,
}
```

#### 4. Staking Mechanism

Economic participation requirement ensuring quality engagement.

```clarity
;; User stake requirements
{
  amount: uint,
  locked-until: uint,
}
```

## 📋 Requirements

- **Stacks Blockchain**: Running on Stacks 2.0+
- **Minimum Stake**: 1 STX (1,000,000 microSTX) for participation
- **Clarinet**: For local development and testing

## 🛠️ Installation

### Prerequisites

1. Install [Clarinet](https://github.com/hirosystems/clarinet)

```bash
curl --proto '=https' --tlsv1.2 -sSf https://run.clarinet.so | sh
```

2. Clone the repository

```bash
git clone https://github.com/aniefioke/voice-chain.git
cd voice-chain
```

### Local Development

1. Initialize the project

```bash
clarinet new voice-chain
cd voice-chain
```

2. Check contract syntax

```bash
clarinet check
```

3. Run tests

```bash
clarinet test
```

4. Start local development environment

```bash
clarinet integrate
```

## 🎯 Core Functions

### Thread Management

#### Create Thread

```clarity
(define-public (create-thread
  (title (string-utf8 256))
  (content (string-utf8 2048))
  (is-premium bool)
  (premium-price uint)
))
```

Creates a new discussion thread with optional premium content monetization.

**Parameters:**

- `title`: Thread title (max 256 UTF-8 characters)
- `content`: Thread content (max 2048 UTF-8 characters)
- `is-premium`: Whether the thread requires payment to access
- `premium-price`: Price in microSTX for premium access

**Requirements:**

- User must have minimum stake (1 STX)
- Title and content must not be empty
- Premium price must be > 0 if is-premium is true

#### Create Reply

```clarity
(define-public (create-reply
  (thread-id uint)
  (content (string-utf8 1024))
  (parent-reply-id (optional uint))
))
```

Creates a reply to a thread or another reply, supporting nested conversations.

**Parameters:**

- `thread-id`: ID of the target thread
- `content`: Reply content (max 1024 UTF-8 characters)
- `parent-reply-id`: Optional parent reply for nested discussions

**Requirements:**

- User must have minimum stake
- Thread must exist and not be locked
- Content must not be empty
- Premium threads require access purchase

### Premium Content

#### Purchase Premium Access

```clarity
(define-public (purchase-premium-access (thread-id uint))
```

Unlocks access to premium thread content by paying the author.

**Parameters:**

- `thread-id`: ID of the premium thread

**Process:**

1. Validates thread is premium and user hasn't already purchased
2. Transfers payment to thread author (minus platform fee)
3. Transfers platform fee to treasury
4. Grants permanent access to user

### Read-Only Functions

#### Get Thread Information

```clarity
(define-read-only (get-thread (thread-id uint)))
```

#### Get User Reputation

```clarity
(define-read-only (get-user-reputation (user principal)))
```

#### Check Premium Access

```clarity
(define-read-only (has-premium-access (thread-id uint) (user principal)))
```

## 🔧 Configuration

### Protocol Parameters

| Parameter | Default Value | Description |
|-----------|---------------|-------------|
| `min-stake-amount` | 1,000,000 µSTX | Minimum stake required for participation |
| `platform-fee-rate` | 250 (2.5%) | Platform fee percentage (basis points) |
| `platform-treasury` | Contract owner | Treasury address for platform fees |

### Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| 100 | `ERR_OWNER_ONLY` | Function restricted to contract owner |
| 101 | `ERR_NOT_FOUND` | Requested resource not found |
| 102 | `ERR_UNAUTHORIZED` | Insufficient permissions |
| 103 | `ERR_INSUFFICIENT_BALANCE` | Insufficient STX balance |
| 104 | `ERR_INVALID_AMOUNT` | Invalid amount provided |
| 105 | `ERR_THREAD_LOCKED` | Thread is locked for replies |
| 106 | `ERR_ALREADY_VOTED` | User has already voted |
| 107 | `ERR_INVALID_TIP` | Invalid tip amount |
| 108 | `ERR_SELF_TIP` | Cannot tip yourself |
| 109 | `ERR_THREAD_NOT_PREMIUM` | Thread is not premium |
| 110 | `ERR_INSUFFICIENT_STAKE` | User stake below minimum |
| 111 | `ERR_INVALID_PARENT_REPLY` | Invalid parent reply reference |

## 🧪 Testing

The protocol includes comprehensive test coverage for all core functions:

```bash
# Run all tests
clarinet test

# Run specific test file
clarinet test tests/voice-chain.test.ts

# Run with coverage
clarinet test --coverage
```

### Test Scenarios

- Thread creation with various parameters
- Reply creation with nested structures
- Premium access purchasing flow
- Reputation calculation accuracy
- Staking mechanism validation
- Error condition handling

## 🚀 Deployment

### Testnet Deployment

1. Configure your testnet environment in `settings/Testnet.toml`
2. Deploy the contract:

```bash
clarinet publish --testnet
```

### Mainnet Deployment

1. Configure your mainnet environment in `settings/Mainnet.toml`
2. Deploy the contract:

```bash
clarinet publish --mainnet
```

## 📊 Economic Model

### Staking Requirements

- **Minimum Stake**: 1 STX per user
- **Purpose**: Prevent spam and ensure quality participation
- **Mechanism**: Locked stake with time-based unlock

### Premium Content Monetization

- **Creator Revenue**: 97.5% of premium access fees
- **Platform Fee**: 2.5% of premium access fees
- **Payment Flow**: Direct STX transfers on purchase

### Reputation System

- **Base Score Calculation**:

  ```
  base_score = (upvotes × 10) + (threads × 5) + (replies × 2)
  ```

- **Downvote Penalty**:

  ```
  final_score = (base_score × 100) / (100 + downvotes × 5)
  ```

## 🔮 Future Enhancements

- [ ] NFT milestone rewards for top contributors
- [ ] Thread boosting with STX deposits
- [ ] Tipping system for quality content
- [ ] Advanced voting mechanisms
- [ ] Content moderation tools
- [ ] Mobile SDK development
- [ ] Web3 frontend integration

## 🤝 Contributing

We welcome contributions to VoiceChain! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Write comprehensive tests for new features
- Follow Clarity best practices and conventions
- Update documentation for API changes
- Ensure all tests pass before submitting PR

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Links

- [Stacks Documentation](https://docs.stacks.co/)
- [Clarity Language Reference](https://docs.stacks.co/clarity/)
- [Clarinet Documentation](https://github.com/hirosystems/clarinet)