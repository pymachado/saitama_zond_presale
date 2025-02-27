# SAITAMA POST QUANTUM RESISTANT PRE-SALE PROTOCOL
The first Quantum-Resistant Decentralized Pre-sale Protocol, empowering liquidity within the QRL ecosystem.

## Simple Summary
The first Quantum-Resistant Decentralized Pre-sale Protocol, powering all liquidity around the QRL ecosystem.

## Author

Pedro Machado

**[Github](https://github.com/pymachado)**

**[Linkedln](https://www.linkedin.com/in/petermacblockchain/?locale=en_US)**

**[x](https://x.com/machado_leiva)**

## Abstract
This protocol serves as the first quantum-resistant decentralized presale platform within the QRL ecosystem. It facilitates liquidity addition in the form of QRL tokens while enabling developers, creators, and artists to fund their projects by selling their assets on the only quantum-resistant layer in the market.

With the QRL Foundation currently developing the new Zond Beta-Testnet V1 EVM, the network is transitioning from PoW to PoS, introducing the [Zond Virtual Machine (ZVM)](https://test-zond.theqrl.org/) to support smart contracts and DeFi protocols. This upgrade strengthens the ecosystem's quantum resistance, addressing the imminent threat posed by advancements in quantum computing, such as [Microsoft's Majorana 1 chip with a power of quantum computation](https://news.microsoft.com/source/features/innovation/microsofts-majorana-1-chip-carves-new-path-for-quantum-computing/) of 8 qubits.

As quantum computers advance, the security of traditional ECDSA-based wallets becomes increasingly vulnerable. Our project aims to be the first quantum-resistant decentralized pre-sale protocol, launching alongside the Zond Mainnet with an address space that is post-quantum secure based on [XMSS](https://datatracker.ietf.org/doc/html/rfc8391). This will provide a secure and future-proof platform for developers, creators, and artists to raise funds by selling their ZRC-20 tokens on the only quantum-resistant blockchain, ensuring the longevity and security of their assets.

## Motivation
The necessity of a quantum-resistant decentralized presale protocol is paramount in ensuring secure and sustainable fundraising mechanisms. By leveraging the QRL ecosystem and its forthcoming Zond upgrade, this protocol enhances decentralization and provides a trustless funding avenue for innovators.

## Project Outline
The Saitama Post Quantum-Resistant Decentralized Pre-sale Protocol aims to provide a secure and decentralized platform for fundraising within the QRL ecosystem. The project is designed to facilitate the sale of ZRC-20 tokens in a quantum-resistant environment, ensuring that investors and project creators can participate without the risk posed by evolving quantum computing threats.

- **Purpose:**
  - Enable developers, creators, and artists to raise funds by selling their ZRC20 assets in a quantum-secure ecosystem.
  - Strengthen the QRL ecosystem by increasing liquidity and participation in decentralized finance (DeFi).
  - Establish a sustainable, transparent, and permissionless presale model resistant to quantum attacks.

- **Scope:**
  - Development of a smart contract-based presale platform running on the Zond Virtual Machine (ZVM).
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
- **Escrow.sol**

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

## **Initialize Pre-sale**  
In this phase, the admin must deploy and set up a new **TierPresale** contract through the **Factory** contract. Additionally, the admin needs to fund the **TierPresale** contract with the total amount of base tokens allocated for sale. This ensures that once the presale starts, the contract can distribute tokens accordingly as contributors participate.  

**Process:**  
1. **Contract deployment:** The admin deploys a new **TierPresale** contract using the **Factory** contract.  
2. **Configuration setup:** The admin configures the initial settings, including allocation per tier, contribution limits, and vesting parameters.  
3. **Funding the contract:** The admin transfers the total amount of base tokens required for the presale into the **TierPresale** contract.  
4. **Presale activation:** Once the setup is complete, the presale is ready to accept contributions as soon as it is officially launched.  

## Raising a Tier
When the presale is in progress and a tier is about to be raised, contributors can participate by making contributions within the allowed minimum and maximum limits. The presale smart contract enforces these limits and ensures the contribution does not exceed the remaining allocation for the current tier.

**Process:**
1. **Contributions are accepted:** As the current tier progresses, the smart contract ensures that the contribution amount does not exceed the remaining allocation for that tier.
2. **Exceeding allocation:** If a contributor attempts to contribute more than the remaining allocation, the contract automatically adjusts the contribution to match the exact remaining amount.

---

## Tier Raised

Once the **currentAllocation** reaches the **allocationPerTier**, the current tier is considered raised. At this point:
- The contract transfers the collected funds to the **escrowAddress**, which is managed by the team creators.
- The contract then resets the **currentAllocation**, increments the **tierNumber**, and updates the **tokensPerBit**.

**Process:**
1. **Escrow update:** Contributions are safely transferred to a **escrowAddress** managed by the team creators.
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

### Total QRL Collected

The total amount of QRL collected by selling the base token during each presale is calculated by multiplying the last tier raised by the contribution per tier:

```math
Total\ QRL\ Collected = Tier\ Raised \times Contribution\ per\ Tier
```

**Process:**
1. **No further contributions:** After reaching the last tier, the contract disables the ability to accept new contributions or emergency withdrawals.
2. **Token claim:** Contributors can still interact with the contract to claim their purchased tokens using the **claimTokens** function.
3. **Vesting contract deposit:** Claimed tokens will be deposited into the Vesting Contract and released over time according to the initial settings. This ensures a structured distribution and prevents immediate large withdrawals.
4. **Escrow status:** All funds from contributions have been transferred to the **escrowAddress**, and the presale state is complete.


---

## Fee Mechanism

The protocol uses a fee mechanism to self-maintain its operations. All transactions on the protocol have a cost associated of 2%.

50% of the total fees collected will be allocated to the staking mechanism of the Saitama protocol. This will help remove a portion of the QRL tokens from circulation, contributing to the scarcity of the token and empowering its use within the ecosystem.

---

## **Project Roadmap, Timeline & Budget**  


| **#** | **Milestone** | **Tasks** | **Timeline** | **Estimated Hours** | **Budget Allocation (USD)** |
|------|-------------|--------|------------|-----------------|---------------------|
| **1** | **Research & Documentation** | - Define architecture & scope  <br> - **Training on QRL and ZondVM ecosystem & performance**  <br> - Create technical documentation | **Week 1** | **40 hours** | **$1,200** |
| **2** | **Node & Wallet Setup** | - Deploy a node using Zond Beta-Testnet  <br> - Set up wallet for development & testing with **zond-web3**  <br> - Ensure network connectivity & basic interactions | **Week 2** | **40 hours** | **$1,200** |
| **3** | **TierPresale.sol Development** | - Develop **TierPresale.sol**  <br> - Implement contribution and tier logic  <br> - Test with TDD | **Week 3 & 4** | **80 hours** | **$2,400** |
| **4** | **Vesting.sol Development** | - Develop **Vesting.sol**  <br> - Implement vesting logic  <br> - Test with TDD | **Week 5 & 6** | **80 hours** | **$2,400** |
| **5** | **Escrow.sol Development** | - Develop **Escrow.sol**  <br> - Implement escrow management logic  <br> - Test with TDD | **Week 7** | **40 hours** | **$1,200** |
| **6** | **Factory.sol Development** | - Develop **Factory.sol**  <br> - Implement core logic  <br> - Test with TDD | **Week 8** | **40 hours** | **$1,200** |
| **7** | **Zond-Web3-Wallet Integration** | - Implement **wallet connection** using Zond-Web3-Wallet <br> - Enable **transaction signing** and authentication <br> - Display **events & errors** triggered by smart contracts | **Week 9** | **40 hours** | **$1,200** |
| **8** | **Web3 API Library Development** | - Develop a **custom Web3 API** to connect the UI with smart contracts <br> - Implement **contract interaction functions** (read/write ops) <br> - Optimize **data fetching & event handling** | **Week 10 & 11** | **80 hours** | **$2,400** |
| **9** | **User Interface Development** | - **Training on front-end** (40h) <br> - Design UI/UX (40h) <br> - Develop front-end (80h) <br> - Test & refine (40h) | **Week 12-17** (if self-developed) <br> **OR** Week 12-14 (if hiring a dev, with $3,500 budget) | **160 hours** | **$4,800** (self) OR **$3,500** (hired) |
| **10** | **Documentation Refinement & Web3 API Doc** | - Refactor and update existing technical documentation  <br> - Create **Web3 API Connectivity** documentation for the front-end integration | **Week 18** | **40 hours** | **$1,200** |
| **11** | **Deployment & Beta Testing** | - Deploy contracts on **Zond Testnet BUIDL** (locally and sync with network nodes) <br> - Simulate **transactions & interactions** in the test environment <br> - Gather **feedback & identify improvements** before mainnet launch | **Week 14-15** | **40 hours** | **$1,200** |
| **12** | **Community Building, Education & Mainnet Preparation** | - **Hire a Web3 Community Manager** to manage social media (Discord, X, Telegram, IG, LinkedIn) <br> - Launch **community engagement & feedback sessions** <br> - Establish an **official YouTube channel** for education & awareness <br> - Create a **structured video course on Saitama Protocol & QRL-ZOND Blockchain** <br> - Course will cover: **Smart Contract Development, Web3 API, and Zond Ecosystem** <br> - Content will include **tutorials, live Q&A, and interactive learning** <br> - Promote through **social media campaigns, dev hackathons, and AMA sessions** <br> - Prepare **user guides & onboarding materials** for mainnet launch | **Week 16-24 (2 Months)** | **160 hours (Community Manager) + 80 hours (Content Creation) = 240 hours** | **$8,000** (Includes $6,000 for Community Manager + $2,000 for Video Production & Content Creation) |


### **Final Total Budget (Rounded Up)**  
✅ **$33,000** (includes a 15% contingency buffer for unexpected costs) with a total of 6 months to create a fully functional MVP.

This ensures flexibility for additional development, bug fixes, marketing, and unforeseen expenses. 🚀

### **Future Recommendations**
- **Saitama NFT Marketplace:** Expand ecosystem utility by integrating with Saitama’s NFT marketplace.
- **Saitama Swap DEX:** Enable seamless token swaps and liquidity management via Saitama Swap DEX.
- **CCIP Integration:** Implement Chainlink’s Cross-Chain Interoperability Protocol (CCIP) once QRL-ZOND is supported for enhanced security and functionality.

---

## Ecosystem Benefits
This protocol aligns with the Foundation's aims by reinforcing the QRL ecosystem, promoting decentralization, and serving the public good. It fosters innovation by offering a secure and quantum-resistant fundraising platform for various stakeholders.

### Some Ecosystem Benefits

1. **First Presale Platform on QRL-ZOND**: Introduces the first presale platform within the QRL-ZOND ecosystem, enabling efficient token launches and liquidity aggregation for new projects.
   
2. **Liquidity Aggregation**: Enhances liquidity across projects, ensuring efficient trading and boosting market participation for $QRL and other tokens.
   
3. **Boosting User Adoption**: Encourages broader adoption of the QRL-ZOND ecosystem by attracting users to the presale platform, increasing the value and utility of $QRL.
   
4. **Community Growth**: Supports community expansion by providing an accessible, transparent fundraising platform for a diverse range of stakeholders, including developers and creators.
   
5. **Enhancing $QRL Usability**: Promotes practical use of $QRL in fundraising activities, driving deeper integration into the ecosystem.
   
6. **Increasing $QRL Scarcity**: Utilizes $QRL in presales and funding events, potentially increasing its scarcity and long-term value.
   
7. **ZRC20 Presale Mechanism**: Enables ZRC20 presales, offering new projects a secure and decentralized way to launch while adhering to QRL’s standards.
   
8. **Funding Platform for Creators**: Provides a secure platform for creators, artists, and developers to fund and launch projects, from digital art to blockchain solutions.
   
9. **Empowering New Projects**: Serves as a launchpad for new projects, including meme coins and utility tokens, offering them the exposure and funding needed to thrive in the QRL-ZOND ecosystem.
   
10. **First Funding Door on QRL**: Opens a comprehensive funding avenue within the QRL ecosystem, allowing individuals and teams to fund blockchain-related projects securely and efficiently.
