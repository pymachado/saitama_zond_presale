# SAITAMA QUANTUM RESISTANT PRE-SALE PROTOCOL
The first Quantum-Resistant Decentralized Pre-sale Protocol, empowering liquidity within the QRL ecosystem.

## Simple Summary
The first Quantum-Resistant Decentralized Pre-sale Protocol, powering all liquidity around the QRL ecosystem.

## Abstract
This protocol serves as the first quantum-resistant decentralized presale platform within the QRL ecosystem. It facilitates liquidity addition in the form of QRL tokens while enabling developers, creators, and artists to fund their projects by selling their assets on the only quantum-resistant layer in the market.

With the QRL Foundation currently developing the new Zond Beta-Testnet V1 EVM, the network is transitioning from PoW to PoS, introducing the [Zond Virtual Machine (ZVM)](https://test-zond.theqrl.org/) to support smart contracts and DeFi protocols. This upgrade strengthens the ecosystem's quantum resistance, addressing the imminent threat posed by advancements in quantum computing, such as [Microsoft's Majorana 1 chip with a power of quantum computation](https://news.microsoft.com/source/features/innovation/microsofts-majorana-1-chip-carves-new-path-for-quantum-computing/) of 8 qubits.

As quantum computers advance, the security of traditional ECDSA-based wallets becomes increasingly vulnerable. Our project aims to be the first quantum-resistant decentralized pre-sale protocol, launching alongside the Zond Mainnet with an address space that is post-quantum secure based on XMSS. This will provide a secure and future-proof platform for developers, creators, and artists to raise funds by selling their ZRC-20 tokens on the only quantum-resistant blockchain, ensuring the longevity and security of their assets.

## Motivation
The necessity of a quantum-resistant decentralized presale protocol is paramount in ensuring secure and sustainable fundraising mechanisms. By leveraging the QRL ecosystem and its forthcoming Zond upgrade, this protocol enhances decentralization and provides a trustless funding avenue for innovators.

## Project Outline
The Quantum-Resistant Decentralized Pre-sale Protocol aims to provide a secure and decentralized platform for fundraising within the QRL ecosystem. The project is designed to facilitate the sale of ERC-20 tokens in a quantum-resistant environment, ensuring that investors and project creators can participate without the risk posed by evolving quantum computing threats.

- **Purpose:**
  - Enable developers, creators, and artists to raise funds by selling their ZRC20 assets in a quantum-secure ecosystem.
  - Strengthen the QRL ecosystem by increasing liquidity and participation in decentralized finance (DeFi).
  - Establish a sustainable, transparent, and permissionless presale model resistant to quantum attacks.

- **Scope:**
  - Development of a smart contract-based presale platform utilizing the Zond Virtual Machine (ZVM).
  - Integration of [zond-web3-wallet](https://github.com/theQRL/zond-web3-wallet) for execute the Presale features. 
  - Integration with QRL’s quantum-resistant blockchain to ensure the security of raised funds and investor contributions.
  - Creation of a user-friendly interface for seamless participation in presales.

- **Expected Outcomes:**
  - A fully functional quantum-resistant presale protocol ready for deployment upon Zond Mainnet launch.
  - Increased adoption of QRL-based smart contracts and DeFi applications.
  - Enhanced security and trust for blockchain fundraising by eliminating vulnerabilities associated with traditional cryptographic models.
  - A flourishing ecosystem where developers, artists, and creators can confidently launch their projects knowing that the infrastructure is built to withstand future quantum advancements.

---

## Presale Mechanism

### Tier-Based Presale without Deadline

- **Overview**  
  The tier-based presale mechanism is designed to incentivize early participation and ensure a structured token distribution process. The model supports predictable and scalable token sales while maintaining market-driven flexibility.

- **Data-Tech**
  - **Number of Tiers:** Defined by the user (admin role).
  - **Allocation per Tier:** Defined by the user (admin role).
  - **Rate per Sale Increase:** Defined by the user (admin role).

### Smart Contract Core

- **Factory.sol**
- **TierPresale.sol**
- **Vesting.sol**

### Mechanism Explanation
The presale operates on a tier-based structure:

- Each tier concludes when all tokens allocated to that tier are sold.
- The token price increases by a user-defined rate upon the completion of each tier, encouraging prompt investment and early participation.
- The allocation and pricing mechanism ensures fair and consistent token distribution across all tiers, adapting to market interest and demand.

### Roles
- **Contributor:** A user who reserves an amount of the base token through the $QRL.
- **Admin:** A user who manages the protocol features, including defining the presale parameters (e.g., number of tiers, allocation per tier, rate of increase).

### Advantages
- Creates urgency for investors as prices increase once a tier is completed.
- The time per tier depends on market interest and demand.

### Disadvantages
- Potential for slower sales if initial interest is low.

---

## Raising a Tier
When the presale is in progress and a tier is about to be raised, contributors can participate by making contributions within the allowed minimum and maximum limits. The presale smart contract enforces these limits and ensures the contribution does not exceed the remaining allocation for the current tier.

**Process:**
1. **Contributions are accepted:** As the current tier progresses, the smart contract ensures that the contribution amount does not exceed the remaining allocation for that tier.
2. **Exceeding allocation:** If a contributor attempts to contribute more than the remaining allocation, the contract automatically adjusts the contribution to match the exact remaining amount.

---

## Tier Raised

Once the **currentAllocation** reaches the **allocationPerTier**, the current tier is considered raised. At this point:
- The contract transfers the collected funds to the **vestingAddress**, which is managed by the presale team.
- The contract then resets the **currentAllocation**, increments the **tierNumber**, and updates the **tokensPerBit**.

**Process:**
1. **Vesting update:** Contributions are safely transferred to a **vestingAddress** managed by the presale team.
2. **Tier transition:** The smart contract prepares for the next tier by adjusting its internal variables:
   - Reset **currentAllocation** to 0.
   - Increase **tierNumber** by 1.
   - Update **tokensPerBit** to reflect the new pricing structure.

---

## Increasing the Tier Price
When a new tier is raised, the token price is adjusted by a user-defined increasing factor (e.g., 3.98%). This ensures that the token price rises with each tier, rewarding early participants with a lower price.

**Process:**
1. **Token price adjustment:** Upon transitioning to a new tier, the price increases by the set rate (e.g., 3.98%).
2. **Scaling the token allocation:** The contract adjusts the **tokensPerBit** accordingly, ensuring contributors receive fewer tokens for the same minimum unit of QRL as the presale progresses.

### Formula

To calculate the new **tokensPerBit**, the formula is as follows:

```math
tokensPerBitScaled = tokensPerBit * SCALE
```

Where the scaling factor **SCALE** is a constant typically defined as 10**6:

```math
tokensPerBit = \frac{tokensPerBitScaled}{increasingFactor}
```

---

## Raising the Presale
The presale progresses through each tier until it reaches the maximum number of tiers set by the admin. The process of raising a tier, updating allocations, and adjusting prices repeats with each new tier until the presale concludes.

1. **Repeat process:** The smart contract follows the same mechanism through all tiers, ensuring fair token distribution and price increases after each tier is raised.
2. **Final tier reached:** When the presale reaches the last tier (e.g., tier number 60), no further contributions will be accepted.

---

## Presale Raised
When the presale reaches its final tier (e.g., tier 60), no more contributions are allowed, and the presale is considered "raised."

**Process:**
1. **No further contributions:** After reaching the last tier, the contract disables the ability to accept new contributions or emergency withdrawals.
2. **Token claim:** Contributors can still interact with the contract to claim their purchased tokens using the **claimTokens** function.
3. **Vesting status:** All funds from contributions have been transferred to the **vestingAddress**, and the presale state is complete.

---

## Fee Mechanism
The protocol uses a fee mechanism to self-maintain its operations. All transactions on the protocol have a cost associated of 2%.

---

## Roadmap, Timeline, and Budget
A structured plan outlining the project's milestones, estimated completion dates, and associated budget requirements.

---

## Ecosystem Benefits
This protocol aligns with the Foundation's aims by reinforcing the QRL ecosystem, promoting decentralization, and serving the public good. It fosters innovation by offering a secure and quantum-resistant fundraising platform for various stakeholders.
```
