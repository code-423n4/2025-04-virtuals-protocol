# Virtuals Protocol audit details

- Total Prize Pool: $60,000 in USDT
  - HM awards: $47,800 in USDT
    - If no valid Highs or Mediums are found, the HM pool is $0
  - QA awards: $2,000 in USDT
  - Judge awards: $5,800 in USDT
  - Validator awards: $3,900 in USDT
  - Scout awards: $500 in USDT
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts April 17, 2025 20:00 UTC
- Ends May 7, 2025 20:00 UTC

### ❗️ Important notes
1. A coded, runnable PoC is **required** for all High/Medium submissions to this audit. 
   - Exception: PoC is optional (though recommended) for wardens with signal ≥ 0.68
   - The audit repo includes a basic template to run the test suite.
   - Your submission will be marked as Insufficient if the POC is not runnable and working.
2. Since this audit includes live/deployed code, **all submissions will be treated as sensitive:**
    - Wardens are encouraged to use the [sensitive disclosure process](https://docs.code4rena.com/competitions/submission-guidelines#how-to-submit-zero-day-or-otherwise-highly-sensitive-bugs) for High-risk submissions affecting live code, to ensure timely disclosure of such vulnerabilities to the sponsor and guarantee payout in the case where [a sponsor patches a live critical during the audit](https://docs.code4rena.com/awarding#the-live-criticals-exception).
    - Submissions will be hidden from *all* wardens (SR and non-SR alike) by default, to ensure that no sensitive issues are erroneously shared. 
    - If the submissions include findings affecting live code, there will be no post-judging QA phase. This ensures that awards can be distributed in a timely fashion, without compromising the security of the project. (Senior members of C4 staff will review the judges’ decisions per usual.)
    - By default, submissions will not be made public until the report is published.
    - **Exception:** if the sponsor indicates that no submissions affect live code, then we’ll make submissions visible to all authenticated wardens, and open PJQA to SR wardens per the usual C4 process.
3. Judging phase risk adjustments:
    - High- or Medium-risk submissions downgraded to Low-risk (QA) will be ineligible for awards.
    - Upgrading a Low-risk finding from a QA report to a Medium- or High-risk finding is not supported.
    - As such, wardens are encouraged to select the appropriate risk level carefully during the submission phase.

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2025-02-virtuals-protocol/blob/main/4naly3er-report.md).

The Slither report can be found [here](https://github.com/code-423n4/2025-02-virtuals-protocol/blob/main/slither.txt).

_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

Many protocol contracts have some form of Admin or role-related execution functions. All of these higher access functions are separate multi-sig wallets such as Fireblocks, that require at least 2 of 3 approval to run

# Overview

| Contract | Purpose | Access Control | Upgradable |
| ------ | ------ | ------ | ------ |
| veVirtualToken | This is a non-transferrable voting token to be used to vote on Virtual Protocol DAO and Virtual Genesis DAO  | Ownable | N |
| VirtualProtocolDAO | Regular DAO to maintain the VIRTUAL ecosystem | - | N |
| VirtualGenesisDAO | Used to vote for instantiation of a VIRTUAL. This DAO allows early execution of proposal as soon as quorum (10k votes) is reached. | - | N |
| AgentFactory | Handles the application & instantiation of a new VIRTUAL. References to TBA registry, VIRTUAL DAO/Token implementation and Persona NFT vault contracts are stored here. | Roles : DEFAULT_ADMIN_ROLE, WITHDRAW_ROLE | Y |
| AgentNft | This is the main registry for Persona, Core and Validator. Used to generate ICV wallet address.  | Roles: DEFAULT_ADMIN_ROLE, VALIDATOR_ADMIN_ROLE, MINTER_ROLE | Y |
| ContributionNft | Each contribution will mint a new ContributionNft. Anyone can propose a new contribution at the VIRTUAL DAO and mint token using the proposal Id.  | - | Y |
| ServiceNft | Accepted contribution will mint a ServiceNft, restricted to only VIRTUAL DAO can mint a ServiceNft. User can query the latest service NFT for a VIRTUAL CORE. | - | Y |
| AgentToken | This is implementation contract for VIRTUAL staking. AgentFactory will clone this during VIRTUAL instantiation. Staked token is non-transferable. | - | N |
| AgentDAO | This is implementation contract for VIRTUAL specific DAO. AgentFactory will clone this during VIRTUAL instantiation. It holds the maturity score for each core service. | - | N |
| AgentReward | This is reward distribution center. | Roles: GOV_ROLE, TOKEN_SAVER_ROLE | Y |
| TimeLockStaking | Allows user to stake their VIRTUAL in exchange for sVIRTUAL | Roles: GOV_ROLE, TOKEN_SAVER_ROLE | N |
| Virtual | VIRTUAL token | Ownable | N |
| Airdrop | Airdrop token to holders | - | N |

## Main Activities

### VIRTUAL Genesis

1. Submit a new application at **AgentFactory**
 a. It will transfer VIRTUAL to AgentFactory
2. Propose at **VirtualGenesisDAO** (action = ```VirtualFactory.executeApplication``` )
3. Start voting at **VirtualGenesisDAO**
4. Execute proposal at  **VirtualGenesisDAO**  , it will do following:
 a. Clone **AgentToken**
 b. Clone **AgentDAO**
 c. Mint **AgentNft**
 d. Stake VIRTUAL -> &#36;`PERSONA (depending on the symbol sent to application)
 e. Create **TBA** with **AgentNft**

### Submit Contribution

1. Create proposal at **AgentDAO** (action = ServiceNft.mint)
2. Mint **ContributionNft** , it will authenticate by checking whether sender is the proposal's proposer.

### Upgrading Core

1. Validator vote for contribution proposal at **AgentDAO**
2. Execute proposal at **AgentDAO**, it will mint a **ServiceNft**, and trigger following actions:
 a. Update maturity score
 b. Update VIRTUAL core service id.

### Distribute Reward

1. On daily basis, protocol backend will conclude daily profits into a single amount.
2. Protocol backend calls **AgentReward**.distributeRewards , triggering following:
 a. Transfer VIRTUAL into **AgentReward**
 b. Account & update claimable amounts for: Protocol, Stakers, Validators, Dataset Contributors, Model Contributors

### Claim Reward

1. Protocol calls **AgentReward**.withdrawProtocolRewards
2. Stakers, Validators, Dataset Contributors, Model Contributors calls **AgentReward**.claimAllRewards

### Staking VIRTUAL

1. Call **AgentToken**.stake , pass in the validator that you would like to delegate your voting power to. It will take in sVIRTUAL and mint &#36;`_PERSONA_ to you.
2. Call **AgentToken**.withdraw to withdraw , will burn your &#36;`_PERSONA_ and return sVIRTUAL to you.

## Links

- **Previous audits:**
  - <https://whitepaper.virtuals.io/info-hub/important-links-and-resources/security-audits>
- **Documentation:** <https://whitepaper.virtuals.io/>
- **Website:** <https://app.virtuals.io/>
- **X/Twitter:** <https://x.com/virtuals_io>
- **Discord:** <https://discord.gg/virtualsio>

---

# Scope

_See [scope.txt](https://github.com/code-423n4/2025-02-virtuals-protocol/blob/main/scope.txt)_

### Files in scope

| File   | Logic Contracts | Interfaces | nSLOC | Purpose | Libraries used |
| ------ | --------------- | ---------- | ----- | -----   | ------------ |
| /contracts/AgentInference.sol | 1| **** | 63 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/utils/math/Math.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/contribution/ContributionNft.sol | 1| **** | 113 | |@openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol, @openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol, @openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol, @openzeppelin/contracts/access/AccessControl.sol, @openzeppelin/contracts/interfaces/IERC5805.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/contribution/IContributionNft.sol | ****| 1 | 4 | |@openzeppelin/contracts/governance/IGovernor.sol|
| /contracts/contribution/IServiceNft.sol | ****| 1 | 3 | ||
| /contracts/contribution/ServiceNft.sol | 1| **** | 124 | |@openzeppelin/contracts/governance/IGovernor.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol, @openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol, @openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol, @openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol, @openzeppelin/contracts/interfaces/IERC5805.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/fun/Bonding.sol | 1| **** | 300 | |@openzeppelin/contracts/utils/ReentrancyGuard.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol|
| /contracts/fun/FERC20.sol | 1| **** | 94 | |@openzeppelin/contracts/access/Ownable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol|
| /contracts/fun/FFactory.sol | 1| **** | 58 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol, @openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol|
| /contracts/fun/FPair.sol | 1| **** | 78 | |@openzeppelin/contracts/utils/ReentrancyGuard.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol|
| /contracts/fun/FRouter.sol | 1| **** | 108 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol|
| /contracts/fun/IFPair.sol | ****| 1 | 3 | ||
| /contracts/genesis/FGenesis.sol | 1| **** | 115 | |@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/genesis/Genesis.sol | 1| **** | 353 | |@openzeppelin/contracts/utils/ReentrancyGuard.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol|
| /contracts/genesis/GenesisLib.sol | 1| **** | 35 | ||
| /contracts/genesis/GenesisTypes.sol | ****| **** | 34 | ||
| /contracts/genesis/MockAgentFactoryV3.sol | 1| **** | 40 | |@openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol, @openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol|
| /contracts/genesis/MockERC20.sol | 1| **** | 12 | |@openzeppelin/contracts/token/ERC20/ERC20.sol|
| /contracts/governance/GovernorCountingSimple.sol | 1| **** | 54 | |@openzeppelin/contracts/governance/Governor.sol|
| /contracts/governance/VirtualGenesisDAO.sol | 1| **** | 95 | |@openzeppelin/contracts/governance/Governor.sol, @openzeppelin/contracts/governance/extensions/GovernorSettings.sol, @openzeppelin/contracts/governance/extensions/GovernorStorage.sol, @openzeppelin/contracts/governance/extensions/GovernorVotes.sol, @openzeppelin/contracts/governance/extensions/GovernorVotesQuorumFraction.sol, @openzeppelin/contracts/utils/structs/Checkpoints.sol, @openzeppelin/contracts/access/AccessControl.sol|
| /contracts/governance/VirtualProtocolDAO.sol | 1| **** | 49 | |@openzeppelin/contracts/governance/Governor.sol, @openzeppelin/contracts/governance/extensions/GovernorSettings.sol, @openzeppelin/contracts/governance/extensions/GovernorStorage.sol, @openzeppelin/contracts/governance/extensions/GovernorVotes.sol, @openzeppelin/contracts/governance/extensions/GovernorVotesQuorumFraction.sol|
| /contracts/governance/veVirtualToken.sol | 1| **** | 32 | |@openzeppelin/contracts/token/ERC20/ERC20.sol, @openzeppelin/contracts/access/Ownable.sol, @openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol, @openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol|
| /contracts/libs/AddressCheckpoints.sol | 1| **** | 61 | |@openzeppelin/contracts/utils/math/Math.sol|
| /contracts/libs/Elo.sol | 1| **** | 32 | ||
| /contracts/libs/FixedPointMathLib.sol | 1| **** | 131 | ||
| /contracts/libs/IERC6551Registry.sol | ****| 1 | 11 | ||
| /contracts/libs/RewardSettingsCheckpoints.sol | 1| **** | 68 | |@openzeppelin/contracts/utils/math/Math.sol|
| /contracts/libs/RewardSettingsCheckpointsV2.sol | 1| **** | 65 | |@openzeppelin/contracts/utils/math/Math.sol|
| /contracts/libs/TokenSaver.sol | 1| **** | 17 | |@openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts/access/AccessControl.sol|
| /contracts/pool/AeroAdaptor.sol | 1| 1 | 36 | |@openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol|
| /contracts/pool/IRouter.sol | ****| 1 | 3 | ||
| /contracts/pool/IUniswapV2Factory.sol | ****| 1 | 4 | ||
| /contracts/pool/IUniswapV2Pair.sol | ****| 1 | 5 | ||
| /contracts/pool/IUniswapV2Router01.sol | ****| 1 | 3 | ||
| /contracts/pool/IUniswapV2Router02.sol | ****| 1 | 4 | ||
| /contracts/tax/AgentTax.sol | 1| **** | 219 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/utils/math/Math.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/tax/BondingTax.sol | 1| **** | 112 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/utils/math/Math.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/tax/IBondingTax.sol | ****| 1 | 3 | ||
| /contracts/tax/ITBABonus.sol | ****| 1 | 3 | ||
| /contracts/tax/LPRefund.sol | 1| **** | 45 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/tax/TBABonus.sol | 1| **** | 61 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/token/Airdrop.sol | 1| **** | 45 | |@openzeppelin/contracts/token/ERC20/ERC20.sol|
| /contracts/token/Virtual.sol | 1| **** | 15 | |@openzeppelin/contracts/token/ERC20/ERC20.sol, @openzeppelin/contracts/token/ERC20/extensions/ERC20Capped.sol, @openzeppelin/contracts/access/Ownable.sol|
| /contracts/virtualPersona/AgentDAO.sol | 1| **** | 150 | |@openzeppelin/contracts-upgradeable/governance/extensions/GovernorSettingsUpgradeable.sol, @openzeppelin/contracts-upgradeable/governance/extensions/GovernorStorageUpgradeable.sol, @openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesQuorumFractionUpgradeable.sol, @openzeppelin/contracts/utils/structs/Checkpoints.sol, @openzeppelin/contracts/token/ERC721/IERC721.sol, @openzeppelin/contracts/utils/Strings.sol|
| /contracts/virtualPersona/AgentFactory.sol | 1| **** | 275 | |@openzeppelin/contracts/proxy/Clones.sol, @openzeppelin/contracts/governance/IGovernor.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts/access/AccessControl.sol, @openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol|
| /contracts/virtualPersona/AgentFactoryV3.sol | 1| **** | 327 | |@openzeppelin/contracts/proxy/Clones.sol, @openzeppelin/contracts/governance/IGovernor.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts/access/AccessControl.sol, @openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol|
| /contracts/virtualPersona/AgentFactoryV4.sol | 1| **** | 369 | |@openzeppelin/contracts/proxy/Clones.sol, @openzeppelin/contracts/governance/IGovernor.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts/access/AccessControl.sol, @openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol|
| /contracts/virtualPersona/AgentMigrator.sol | 1| **** | 129 | |@openzeppelin/contracts/proxy/Clones.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/governance/IGovernor.sol, @openzeppelin/contracts/access/Ownable.sol, @openzeppelin/contracts/utils/Pausable.sol|
| /contracts/virtualPersona/AgentNftV2.sol | 1| **** | 192 | |@openzeppelin/contracts/governance/extensions/GovernorVotes.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/interfaces/IERC5805.sol, @openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol, @openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol, @openzeppelin/contracts/token/ERC721/extensions/IERC721Enumerable.sol, @openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/virtualPersona/AgentToken.sol | 1| **** | 422 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol, @openzeppelin/contracts-upgradeable/access/Ownable2StepUpgradeable.sol, @openzeppelin/contracts/utils/structs/EnumerableSet.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol|
| /contracts/virtualPersona/AgentVeToken.sol | 1| **** | 98 | |@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol, @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol, @openzeppelin/contracts/utils/structs/Checkpoints.sol, @openzeppelin/contracts/access/IAccessControl.sol|
| /contracts/virtualPersona/CoreRegistry.sol | 1| **** | 18 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/virtualPersona/ERC20Votes.sol | 1| **** | 19 | |@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol, @openzeppelin/contracts/utils/math/SafeCast.sol|
| /contracts/virtualPersona/EloCalculator.sol | 1| **** | 49 | |@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol, @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol, @openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol|
| /contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol | 1| **** | 55 | |@openzeppelin/contracts-upgradeable/governance/GovernorUpgradeable.sol|
| /contracts/virtualPersona/IAgentDAO.sol | ****| 1 | 5 | |@openzeppelin/contracts/governance/IGovernor.sol, @openzeppelin/contracts/governance/utils/IVotes.sol|
| /contracts/virtualPersona/IAgentFactory.sol | ****| 1 | 4 | |@openzeppelin/contracts/governance/IGovernor.sol|
| /contracts/virtualPersona/IAgentFactoryV3.sol | ****| 1 | 4 | |@openzeppelin/contracts/governance/IGovernor.sol|
| /contracts/virtualPersona/IAgentFactoryV4.sol | ****| 1 | 4 | |@openzeppelin/contracts/governance/IGovernor.sol|
| /contracts/virtualPersona/IAgentNft.sol | ****| 1 | 16 | ||
| /contracts/virtualPersona/IAgentToken.sol | ****| 1 | 144 | |@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol, @openzeppelin/contracts/token/ERC20/IERC20.sol|
| /contracts/virtualPersona/IAgentVeToken.sol | ****| 1 | 3 | ||
| /contracts/virtualPersona/IERC20Config.sol | ****| 1 | 28 | ||
| /contracts/virtualPersona/IEloCalculator.sol | ****| 1 | 3 | ||
| /contracts/virtualPersona/IErrors.sol | ****| 1 | 156 | ||
| /contracts/virtualPersona/IExecutionInterface.sol | ****| 1 | 3 | ||
| /contracts/virtualPersona/IValidatorRegistry.sol | ****| 1 | 4 | ||
| /contracts/virtualPersona/ValidatorRegistry.sol | 1| **** | 51 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| **Totals** | **43** | **24** | **5238** | | |

### Files out of scope

_See [out_of_scope.txt](https://github.com/code-423n4/2025-02-virtuals-protocol/blob/main/out_of_scope.txt)_

| File         |
| ------------ |
| ./contracts/AgentRewardV2.sol |
| ./contracts/AgentRewardV3.sol |
| ./contracts/IAgentReward.sol |
| ./contracts/IAgentRewardV3.sol |
| ./contracts/dev/BMWToken.sol |
| ./contracts/dev/BMWTokenChild.sol |
| ./contracts/dev/ERC6551BytecodeLib.sol |
| ./contracts/dev/ERC6551Registry.sol |
| ./contracts/dev/FxERC20ChildTunnel.sol |
| ./contracts/dev/FxERC20RootTunnel.sol |
| ./contracts/dev/ProxyAdmin.sol |
| ./contracts/dev/tba/lib/ERC6551AccountLib.sol |
| ./contracts/dev/tba/lib/ERC6551BytecodeLib.sol |
| ./contracts/token/IMinter.sol |
| ./contracts/token/Minter.sol |
| Totals: 15 |

## Scoping Q &amp; A

### General questions

| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |      VIRTUAL Creating new agent tokens            |
| ERC721 used  by the protocol            |         Creating new ERC721 tokens as proof of agentic work           |
| ERC777 used by the protocol             |         None           |
| ERC1155 used by the protocol            |        None      |
| Chains the protocol will be deployed on | Base |

### External integrations (e.g., Uniswap) behavior in scope

| Question                                                  | Answer |
| --------------------------------------------------------- | ------ |
| Enabling/disabling fees (e.g. Blur disables/enables fees) | Yes   |
| Pausability (e.g. Uniswap pool gets paused)               |  No   |
| Upgradeability (e.g. Uniswap gets upgraded)               |   Yes  |

### EIP compliance checklist

N/A

# Additional context

## Main invariants

N/A

## Attack ideas (where to focus for bugs)

Bonding pool

Is there a way for users to pull liquidity from the bonding pool

Any risk of loss of funds

Access control related

Are all privileged actions guarded by access controls

## All trusted roles in the protocol

| Role                                | Description                       |
| --------------------------------------- | ---------------------------- |
| Admin                          | controls fees and other higher-order functions              |
| Executor                             | Runs execution functions like fee/tax distribution                     |
| Deployer                             | Deployer for contracts                   |

## Describe any novel or unique curve logic or mathematical models implemented in the contracts

Simple bonding curve with a `y = kx`

## Running tests

```bash
git clone --recurse https://github.com/code-423n4/2025-04-virtuals-protocol.git
cd 2025-04-virtuals-protocol
yarn
npx hardhat test
```

To run code coverage

```bash
npx hardhat coverage
```

## Miscellaneous

Employees of Virtuals and employees' family members are ineligible to participate in this audit.

Code4rena's rules cannot be overridden by the contents of this README. In case of doubt, please check with C4 staff.
