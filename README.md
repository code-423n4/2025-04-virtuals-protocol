# ✨ So you want to run an audit

This `README.md` contains a set of checklists for our audit collaboration. This is your audit repo, which is used for scoping your audit and for providing information to wardens

Some of the checklists in this doc are for our scouts and some of them are for **you as the audit sponsor (⭐️)**.

---

# Repo setup

## ⭐️ Sponsor: Add code to this repo

- [ ] Create a PR to this repo with the below changes:
- [ ] Confirm that this repo is a self-contained repository with working commands that will build (at least) all in-scope contracts, and commands that will run tests producing gas reports for the relevant contracts.
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 48 business hours prior to audit start time.**
- [ ] Be prepared for a 🚨code freeze🚨 for the duration of the audit — important because it establishes a level playing field. We want to ensure everyone's looking at the same code, no matter when they look during the audit. (Note: this includes your own repo, since a PR can leak alpha to our wardens!)

## ⭐️ Sponsor: Repo checklist

- [ ] Modify the [Overview](#overview) section of this `README.md` file. Describe how your code is supposed to work with links to any relevant documentation and any other criteria/details that the auditors should keep in mind when reviewing. (Here are two well-constructed examples: [Ajna Protocol](https://github.com/code-423n4/2023-05-ajna) and [Maia DAO Ecosystem](https://github.com/code-423n4/2023-05-maia))
- [ ] Optional: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] Review and confirm the details created by the Scout (technical reviewer) who was assigned to your contest. *Note: any files not listed as "in scope" will be considered out of scope for the purposes of judging, even if the file will be part of the deployed contracts.*  

---

# Virtuals Protocol audit details
- Total Prize Pool: $60,000 in USDT
  - HM awards: $47,800 in USDT
    - If no valid Highs or Mediums are found, the HM pool is $0
  - QA awards: $2,000 in USDT 
  - Judge awards: $5,800 in USDT
  - Validator awards: $3,900 USDT
  - Scout awards: $500 in USDT
  - (this line can be removed if there is no mitigation) Mitigation Review: XXX XXX USDC
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts April 17, 2025 20:00 UTC
- Ends May XX, 2025 20:00 UTC

**Note re: risk level upgrades/downgrades**

Two important notes about judging phase risk adjustments: 
- High- or Medium-risk submissions downgraded to Low-risk (QA) will be ineligible for awards.
- Upgrading a Low-risk finding from a QA report to a Medium- or High-risk finding is not supported.

As such, wardens are encouraged to select the appropriate risk level carefully during the submission phase.

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2025-02-virtuals-protocol/blob/main/4naly3er-report.md).


_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

Many protocol contracts have some form of Admin or role-related execution functions. All of these higher access functions are separate multi-sig wallets such as Fireblocks, that require at least 2 of 3 approval to run


✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

# Overview

[ ⭐️ SPONSORS: add info here ]

## Links

- **Previous audits:** https://whitepaper.virtuals.io/info-hub/important-links-and-resources/security-audits
  - ✅ SCOUTS: If there are multiple report links, please format them in a list.
- **Documentation:** https://whitepaper.virtuals.io/
- **Website:** https://app.virtuals.io/
- **X/Twitter:** https://x.com/virtuals_io
- **Discord:** discord.gg/virtualsio

---

# Scope

[ ✅ SCOUTS: add scoping and technical details here ]

### Files in scope
- ✅ This should be completed using the `metrics.md` file
- ✅ Last row of the table should be Total: SLOC
- ✅ SCOUTS: Have the sponsor review and and confirm in text the details in the section titled "Scoping Q amp; A"

*For sponsors that don't use the scoping tool: list all files in scope in the table below (along with hyperlinks) -- and feel free to add notes to emphasize areas of focus.*

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [contracts/folder/sample.sol](https://github.com/code-423n4/repo-name/blob/contracts/folder/sample.sol) | 123 | This contract does XYZ | [`@openzeppelin/*`](https://openzeppelin.com/contracts/) |

### Files out of scope
✅ SCOUTS: List files/directories out of scope

## Scoping Q &amp; A

### General questions
### Are there any ERC20's in scope?: Yes

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".

Specific tokens (please specify)
 VIRTUAL Creating new agent tokens

### Are there any ERC777's in scope?: No

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".



### Are there any ERC721's in scope?: Yes

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".

Creating new ERC721 tokens as proof of agentic work

### Are there any ERC1155's in scope?: No

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".



✅ SCOUTS: Once done populating the table below, please remove all the Q/A data above.

| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |       🖊️             |
| Test coverage                           | ✅ SCOUTS: Please populate this after running the test coverage command                          |
| ERC721 used  by the protocol            |            🖊️              |
| ERC777 used by the protocol             |           🖊️                |
| ERC1155 used by the protocol            |              🖊️            |
| Chains the protocol will be deployed on | Base |

### ERC20 token behaviors in scope

| Question                                                                                                                                                   | Answer |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| [Missing return values](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#missing-return-values)                                                      |    |
| [Fee on transfer](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#fee-on-transfer)                                                                  |   |
| [Balance changes outside of transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#balance-modifications-outside-of-transfers-rebasingairdrops) |    |
| [Upgradeability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#upgradable-tokens)                                                                 |    |
| [Flash minting](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#flash-mintable-tokens)                                                              |    |
| [Pausability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#pausable-tokens)                                                                      |    |
| [Approval race protections](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#approval-race-protections)                                              |    |
| [Revert on approval to zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-approval-to-zero-address)                            |    |
| [Revert on zero value approvals](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-approvals)                                    |    |
| [Revert on zero value transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                    |    |
| [Revert on transfer to the zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-transfer-to-the-zero-address)                    |    |
| [Revert on large approvals and/or transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-large-approvals--transfers)                  |    |
| [Doesn't revert on failure](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#no-revert-on-failure)                                                   |    |
| [Multiple token addresses](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                          |    |
| [Low decimals ( < 6)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#low-decimals)                                                                 |    |
| [High decimals ( > 18)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#high-decimals)                                                              |    |
| [Blocklists](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#tokens-with-blocklists)                                                                |    |

### External integrations (e.g., Uniswap) behavior in scope:


| Question                                                  | Answer |
| --------------------------------------------------------- | ------ |
| Enabling/disabling fees (e.g. Blur disables/enables fees) | Yes   |
| Pausability (e.g. Uniswap pool gets paused)               |  No   |
| Upgradeability (e.g. Uniswap gets upgraded)               |   Yes  |


### EIP compliance checklist
N/A

✅ SCOUTS: Please format the response above 👆 using the template below👇

| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| src/Token.sol                           | ERC20, ERC721                |
| src/NFT.sol                             | ERC721                       |


# Additional context

## Main invariants

N/A

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

## Attack ideas (where to focus for bugs)
Bonding pool

Is there a way for users to pull liquidity from the bonding pool
Any risk of loss of funds
Access control related

Are all privileged actions guarded by access controls


✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

## All trusted roles in the protocol

Admin - controls fees and other higher-order functions 
Executor - Runs execution functions like fee/tax distribution 
Deployer - Deployer for contracts

✅ SCOUTS: Please format the response above 👆 using the template below👇

| Role                                | Description                       |
| --------------------------------------- | ---------------------------- |
| Owner                          | Has superpowers                |
| Administrator                             | Can change fees                       |

## Describe any novel or unique curve logic or mathematical models implemented in the contracts:

Simple bonding curve with a y = kx 

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

## Running tests

npm i
npx hardhat test

✅ SCOUTS: Please format the response above 👆 using the template below👇

```bash
git clone https://github.com/code-423n4/2023-08-arbitrum
git submodule update --init --recursive
cd governance
foundryup
make install
make build
make sc-election-test
```
To run code coverage
```bash
make coverage
```
To run gas benchmarks
```bash
make gas
```

✅ SCOUTS: Add a screenshot of your terminal showing the gas report
✅ SCOUTS: Add a screenshot of your terminal showing the test coverage

## Miscellaneous
Employees of Virtuals and employees' family members are ineligible to participate in this audit.

Code4rena's rules cannot be overridden by the contents of this README. In case of doubt, please check with C4 staff.





# Scope

*See [scope.txt](https://github.com/code-423n4/2025-02-virtuals-protocol/blob/main/scope.txt)*

### Files in scope


| File   | Logic Contracts | Interfaces | nSLOC | Purpose | Libraries used |
| ------ | --------------- | ---------- | ----- | -----   | ------------ |
| /contracts/AgentInference.sol | 1| **** | 63 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/utils/math/Math.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/contribution/ContributionNft.sol | 1| **** | 113 | |@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol<br>@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol<br>@openzeppelin/contracts/access/AccessControl.sol<br>@openzeppelin/contracts/interfaces/IERC5805.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/contribution/IContributionNft.sol | ****| 1 | 4 | |@openzeppelin/contracts/governance/IGovernor.sol|
| /contracts/contribution/IServiceNft.sol | ****| 1 | 3 | ||
| /contracts/contribution/ServiceNft.sol | 1| **** | 124 | |@openzeppelin/contracts/governance/IGovernor.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol<br>@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol<br>@openzeppelin/contracts/interfaces/IERC5805.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/fun/Bonding.sol | 1| **** | 300 | |@openzeppelin/contracts/utils/ReentrancyGuard.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol|
| /contracts/fun/FERC20.sol | 1| **** | 94 | |@openzeppelin/contracts/access/Ownable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol|
| /contracts/fun/FFactory.sol | 1| **** | 58 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol|
| /contracts/fun/FPair.sol | 1| **** | 78 | |@openzeppelin/contracts/utils/ReentrancyGuard.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol|
| /contracts/fun/FRouter.sol | 1| **** | 108 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol|
| /contracts/fun/IFPair.sol | ****| 1 | 3 | ||
| /contracts/genesis/FGenesis.sol | 1| **** | 115 | |@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/genesis/Genesis.sol | 1| **** | 353 | |@openzeppelin/contracts/utils/ReentrancyGuard.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol|
| /contracts/genesis/GenesisLib.sol | 1| **** | 35 | ||
| /contracts/genesis/GenesisTypes.sol | ****| **** | 34 | ||
| /contracts/genesis/MockAgentFactoryV3.sol | 1| **** | 40 | |@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol|
| /contracts/genesis/MockERC20.sol | 1| **** | 12 | |@openzeppelin/contracts/token/ERC20/ERC20.sol|
| /contracts/governance/GovernorCountingSimple.sol | 1| **** | 54 | |@openzeppelin/contracts/governance/Governor.sol|
| /contracts/governance/VirtualGenesisDAO.sol | 1| **** | 95 | |@openzeppelin/contracts/governance/Governor.sol<br>@openzeppelin/contracts/governance/extensions/GovernorSettings.sol<br>@openzeppelin/contracts/governance/extensions/GovernorStorage.sol<br>@openzeppelin/contracts/governance/extensions/GovernorVotes.sol<br>@openzeppelin/contracts/governance/extensions/GovernorVotesQuorumFraction.sol<br>@openzeppelin/contracts/utils/structs/Checkpoints.sol<br>@openzeppelin/contracts/access/AccessControl.sol|
| /contracts/governance/VirtualProtocolDAO.sol | 1| **** | 49 | |@openzeppelin/contracts/governance/Governor.sol<br>@openzeppelin/contracts/governance/extensions/GovernorSettings.sol<br>@openzeppelin/contracts/governance/extensions/GovernorStorage.sol<br>@openzeppelin/contracts/governance/extensions/GovernorVotes.sol<br>@openzeppelin/contracts/governance/extensions/GovernorVotesQuorumFraction.sol|
| /contracts/governance/veVirtualToken.sol | 1| **** | 32 | |@openzeppelin/contracts/token/ERC20/ERC20.sol<br>@openzeppelin/contracts/access/Ownable.sol<br>@openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol<br>@openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol|
| /contracts/libs/AddressCheckpoints.sol | 1| **** | 61 | |@openzeppelin/contracts/utils/math/Math.sol|
| /contracts/libs/Elo.sol | 1| **** | 32 | ||
| /contracts/libs/FixedPointMathLib.sol | 1| **** | 131 | ||
| /contracts/libs/IERC6551Registry.sol | ****| 1 | 11 | ||
| /contracts/libs/RewardSettingsCheckpoints.sol | 1| **** | 68 | |@openzeppelin/contracts/utils/math/Math.sol|
| /contracts/libs/RewardSettingsCheckpointsV2.sol | 1| **** | 65 | |@openzeppelin/contracts/utils/math/Math.sol|
| /contracts/libs/TokenSaver.sol | 1| **** | 17 | |@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts/access/AccessControl.sol|
| /contracts/pool/AeroAdaptor.sol | 1| 1 | 36 | |@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol|
| /contracts/pool/IRouter.sol | ****| 1 | 3 | ||
| /contracts/pool/IUniswapV2Factory.sol | ****| 1 | 4 | ||
| /contracts/pool/IUniswapV2Pair.sol | ****| 1 | 5 | ||
| /contracts/pool/IUniswapV2Router01.sol | ****| 1 | 3 | ||
| /contracts/pool/IUniswapV2Router02.sol | ****| 1 | 4 | ||
| /contracts/tax/AgentTax.sol | 1| **** | 219 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/utils/math/Math.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/tax/BondingTax.sol | 1| **** | 112 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/utils/math/Math.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/tax/IBondingTax.sol | ****| 1 | 3 | ||
| /contracts/tax/ITBABonus.sol | ****| 1 | 3 | ||
| /contracts/tax/LPRefund.sol | 1| **** | 45 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/tax/TBABonus.sol | 1| **** | 61 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol|
| /contracts/token/Airdrop.sol | 1| **** | 45 | |@openzeppelin/contracts/token/ERC20/ERC20.sol|
| /contracts/token/Virtual.sol | 1| **** | 15 | |@openzeppelin/contracts/token/ERC20/ERC20.sol<br>@openzeppelin/contracts/token/ERC20/extensions/ERC20Capped.sol<br>@openzeppelin/contracts/access/Ownable.sol|
| /contracts/virtualPersona/AgentDAO.sol | 1| **** | 150 | |@openzeppelin/contracts-upgradeable/governance/extensions/GovernorSettingsUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/governance/extensions/GovernorStorageUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesQuorumFractionUpgradeable.sol<br>@openzeppelin/contracts/utils/structs/Checkpoints.sol<br>@openzeppelin/contracts/token/ERC721/IERC721.sol<br>@openzeppelin/contracts/utils/Strings.sol|
| /contracts/virtualPersona/AgentFactory.sol | 1| **** | 275 | |@openzeppelin/contracts/proxy/Clones.sol<br>@openzeppelin/contracts/governance/IGovernor.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts/access/AccessControl.sol<br>@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol|
| /contracts/virtualPersona/AgentFactoryV3.sol | 1| **** | 327 | |@openzeppelin/contracts/proxy/Clones.sol<br>@openzeppelin/contracts/governance/IGovernor.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts/access/AccessControl.sol<br>@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol|
| /contracts/virtualPersona/AgentFactoryV4.sol | 1| **** | 369 | |@openzeppelin/contracts/proxy/Clones.sol<br>@openzeppelin/contracts/governance/IGovernor.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts/access/AccessControl.sol<br>@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol|
| /contracts/virtualPersona/AgentMigrator.sol | 1| **** | 129 | |@openzeppelin/contracts/proxy/Clones.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/governance/IGovernor.sol<br>@openzeppelin/contracts/access/Ownable.sol<br>@openzeppelin/contracts/utils/Pausable.sol|
| /contracts/virtualPersona/AgentNftV2.sol | 1| **** | 192 | |@openzeppelin/contracts/governance/extensions/GovernorVotes.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/interfaces/IERC5805.sol<br>@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol<br>@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol<br>@openzeppelin/contracts/token/ERC721/extensions/IERC721Enumerable.sol<br>@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/virtualPersona/AgentToken.sol | 1| **** | 422 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/access/Ownable2StepUpgradeable.sol<br>@openzeppelin/contracts/utils/structs/EnumerableSet.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol|
| /contracts/virtualPersona/AgentVeToken.sol | 1| **** | 98 | |@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/utils/structs/Checkpoints.sol<br>@openzeppelin/contracts/access/IAccessControl.sol|
| /contracts/virtualPersona/CoreRegistry.sol | 1| **** | 18 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| /contracts/virtualPersona/ERC20Votes.sol | 1| **** | 19 | |@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol<br>@openzeppelin/contracts/utils/math/SafeCast.sol|
| /contracts/virtualPersona/EloCalculator.sol | 1| **** | 49 | |@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol<br>@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol|
| /contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol | 1| **** | 55 | |@openzeppelin/contracts-upgradeable/governance/GovernorUpgradeable.sol|
| /contracts/virtualPersona/IAgentDAO.sol | ****| 1 | 5 | |@openzeppelin/contracts/governance/IGovernor.sol<br>@openzeppelin/contracts/governance/utils/IVotes.sol|
| /contracts/virtualPersona/IAgentFactory.sol | ****| 1 | 4 | |@openzeppelin/contracts/governance/IGovernor.sol|
| /contracts/virtualPersona/IAgentFactoryV3.sol | ****| 1 | 4 | |@openzeppelin/contracts/governance/IGovernor.sol|
| /contracts/virtualPersona/IAgentFactoryV4.sol | ****| 1 | 4 | |@openzeppelin/contracts/governance/IGovernor.sol|
| /contracts/virtualPersona/IAgentNft.sol | ****| 1 | 16 | ||
| /contracts/virtualPersona/IAgentToken.sol | ****| 1 | 144 | |@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol|
| /contracts/virtualPersona/IAgentVeToken.sol | ****| 1 | 3 | ||
| /contracts/virtualPersona/IERC20Config.sol | ****| 1 | 28 | ||
| /contracts/virtualPersona/IEloCalculator.sol | ****| 1 | 3 | ||
| /contracts/virtualPersona/IErrors.sol | ****| 1 | 156 | ||
| /contracts/virtualPersona/IExecutionInterface.sol | ****| 1 | 3 | ||
| /contracts/virtualPersona/IValidatorRegistry.sol | ****| 1 | 4 | ||
| /contracts/virtualPersona/ValidatorRegistry.sol | 1| **** | 51 | |@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol|
| **Totals** | **43** | **24** | **5238** | | |

### Files out of scope

*See [out_of_scope.txt](https://github.com/code-423n4/2025-02-virtuals-protocol/blob/main/out_of_scope.txt)*

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

