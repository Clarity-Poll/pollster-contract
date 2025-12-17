# Pollster

A simple voting and polling smart contract for Stacks blockchain built with Clarity. Create polls, cast votes, and track results on-chain.

## What It Does

Pollster allows you to:
- Create polls with multiple options
- Cast one vote per user (no duplicate voting)
- Count votes in real-time
- View poll results
- Close polls to prevent further voting.

Perfect for:
- Community decisions
- DAO governance
- Simple surveys
- Learning Clarity maps and duplicate prevention
- Understanding on-chain voting mechanics

## Features

- **One Vote Per User**: Each address can only vote once per poll
- **Multiple Options**: Create polls with as many options as you need
- **Real-Time Results**: Check vote counts at any time
- **Poll Management**: Create and close polls
- **Transparent**: All votes are recorded on-chain
- **Gas Efficient**: Optimized for minimal transaction costs

## Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) installed
- Basic understanding of Stacks blockchain
- A Stacks wallet for testnet deployment

## Installation

```bash
# Clone the repository
git clone https://github.com/Clarity-Poll/pollster-contract.git
cd pollster

# Check Clarinet installation
clarinet --version
```

## Project Structure

```
pollster/
├── contracts/
│   └── pollster.clar        # Main voting contract
├── tests/
│   └── pollster_test.ts     # Contract tests
├── Clarinet.toml            # Project configuration
└── README.md
```

## Usage

### Deploy Locally

```bash
# Start Clarinet console
clarinet console

# Create a poll
(contract-call? .pollster create-poll "Favorite Color?" (list "Red" "Blue" "Green"))

# Cast a vote (option index starts at 0)
(contract-call? .pollster vote u0 u1)  ;; Vote for option 1 (Blue) in poll 0

# Get results
(contract-call? .pollster get-poll-results u0)
```

### Contract Functions

**create-poll (title, options)**
```clarity
(contract-call? .pollster create-poll 
  "Best Programming Language?" 
  (list "Python" "JavaScript" "Rust" "Clarity")
)
```
Creates a new poll and returns the poll ID

**vote (poll-id, option-index)**
```clarity
(contract-call? .pollster vote u0 u2)
```
Cast your vote for option 2 in poll 0

**get-poll-info (poll-id)**
```clarity
(contract-call? .pollster get-poll-info u0)
```
Returns poll title, total votes, and status

**get-option-votes (poll-id, option-index)**
```clarity
(contract-call? .pollster get-option-votes u0 u1)
```
Returns vote count for a specific option

**has-voted (poll-id, user)**
```clarity
(contract-call? .pollster has-voted u0 tx-sender)
```
Check if a user has already voted

**close-poll (poll-id)**
```clarity
(contract-call? .pollster close-poll u0)
```
Close the poll (only creator can do this)

## How It Works

### Creating a Poll
1. User submits poll title and list of options
2. Contract assigns unique poll ID
3. Poll is set to "active" status
4. Creator is recorded

### Voting
1. User selects poll ID and option index
2. Contract checks if user has already voted
3. If not, vote is recorded and count is incremented
4. User is marked as "has voted" for this poll

### Preventing Duplicates
The contract uses a composite map key `{poll-id + voter-principal}` to track who has voted. This ensures:
- One vote per address per poll
- No double voting possible
- Transparent vote tracking

## Testing

```bash
# Run all tests
npm run test

# Check contract syntax
clarinet check
```

## Learning Goals

Building this contract teaches you:
- ✅ Creating and managing maps
- ✅ Preventing duplicate actions
- ✅ Working with lists
- ✅ Composite map keys
- ✅ Access control patterns
- ✅ Vote counting logic

## Example Use Cases

**Community Decision:**
```clarity
;; Should we add a new feature?
(contract-call? .pollster create-poll 
  "Add dark mode to our app?" 
  (list "Yes" "No" "Maybe later")
)
```

**DAO Proposal:**
```clarity
;; Vote on treasury allocation
(contract-call? .pollster create-poll 
  "Allocate 1000 STX to marketing?" 
  (list "Approve" "Reject" "Needs revision")
)
```

**Simple Survey:**
```clarity
;; Gather feedback
(contract-call? .pollster create-poll 
  "How satisfied are you?" 
  (list "Very satisfied" "Satisfied" "Neutral" "Dissatisfied")
)
```

## Deployment

### Testnet
```bash
clarinet deployments generate --testnet --low-cost
clarinet deployments apply -p deployments/default.testnet-plan.yaml
```

### Mainnet
```bash
clarinet deployments generate --mainnet
clarinet deployments apply -p deployments/default.mainnet-plan.yaml
```

## Roadmap

- [ ] Write the core contract
- [ ] Add comprehensive tests
- [ ] Deploy to testnet
- [ ] Add weighted voting option
- [ ] Support poll expiration by block height
- [ ] Add poll categories/tags
- [ ] Implement vote delegation

## Security Considerations

- Users can only vote once per poll
- Only poll creator can close their polls
- Votes cannot be changed once cast
- All votes are public and transparent
- No admin backdoors or override functions

## Contributing

This is a learning project! Feel free to:
- Open issues for questions
- Submit PRs for improvements
- Fork and experiment
- Share your polls

## License

MIT License - do whatever you want with it

## Resources

- [Clarity Language Reference](https://docs.stacks.co/clarity)
- [Clarinet Documentation](https://github.com/hirosystems/clarinet)
- [Stacks Blockchain](https://www.stacks.co/)
- [Voting Contract Examples](https://github.com/stacksgov)

---

Built while learning Clarity 🗳️
