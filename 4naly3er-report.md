# Report

- [Report](#report)
  - [Gas Optimizations](#gas-optimizations)
    - [\[GAS-1\] Use ERC721A instead ERC721](#gas-1-use-erc721a-instead-erc721)
    - [\[GAS-2\] Don't use `_msgSender()` if not supporting EIP-2771](#gas-2-dont-use-_msgsender-if-not-supporting-eip-2771)
    - [\[GAS-3\] `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings)](#gas-3-a--a--b-is-more-gas-effective-than-a--b-for-state-variables-excluding-arrays-and-mappings)
    - [\[GAS-4\] Use assembly to check for `address(0)`](#gas-4-use-assembly-to-check-for-address0)
    - [\[GAS-5\] `array[index] += amount` is cheaper than `array[index] = array[index] + amount` (or related variants)](#gas-5-arrayindex--amount-is-cheaper-than-arrayindex--arrayindex--amount-or-related-variants)
    - [\[GAS-6\] Using bools for storage incurs overhead](#gas-6-using-bools-for-storage-incurs-overhead)
    - [\[GAS-7\] Cache array length outside of loop](#gas-7-cache-array-length-outside-of-loop)
    - [\[GAS-8\] State variables should be cached in stack variables rather than re-reading them from storage](#gas-8-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage)
    - [\[GAS-9\] Use calldata instead of memory for function arguments that do not get mutated](#gas-9-use-calldata-instead-of-memory-for-function-arguments-that-do-not-get-mutated)
    - [\[GAS-10\] For Operations that will not overflow, you could use unchecked](#gas-10-for-operations-that-will-not-overflow-you-could-use-unchecked)
    - [\[GAS-11\] Use Custom Errors instead of Revert Strings to save Gas](#gas-11-use-custom-errors-instead-of-revert-strings-to-save-gas)
    - [\[GAS-12\] Avoid contract existence checks by using low level calls](#gas-12-avoid-contract-existence-checks-by-using-low-level-calls)
    - [\[GAS-13\] Stack variable used as a cheaper cache for a state variable is only used once](#gas-13-stack-variable-used-as-a-cheaper-cache-for-a-state-variable-is-only-used-once)
    - [\[GAS-14\] State variables only set in the constructor should be declared `immutable`](#gas-14-state-variables-only-set-in-the-constructor-should-be-declared-immutable)
    - [\[GAS-15\] Functions guaranteed to revert when called by normal users can be marked `payable`](#gas-15-functions-guaranteed-to-revert-when-called-by-normal-users-can-be-marked-payable)
    - [\[GAS-16\] `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`)](#gas-16-i-costs-less-gas-compared-to-i-or-i--1-same-for---i-vs-i---or-i---1)
    - [\[GAS-17\] Using `private` rather than `public` for constants, saves gas](#gas-17-using-private-rather-than-public-for-constants-saves-gas)
    - [\[GAS-18\] Use shift right/left instead of division/multiplication if possible](#gas-18-use-shift-rightleft-instead-of-divisionmultiplication-if-possible)
    - [\[GAS-19\] Splitting require() statements that use \&\& saves gas](#gas-19-splitting-require-statements-that-use--saves-gas)
    - [\[GAS-20\] `uint256` to `bool` `mapping`: Utilizing Bitmaps to dramatically save on Gas](#gas-20-uint256-to-bool-mapping-utilizing-bitmaps-to-dramatically-save-on-gas)
    - [\[GAS-21\] Increments/decrements can be unchecked in for-loops](#gas-21-incrementsdecrements-can-be-unchecked-in-for-loops)
    - [\[GAS-22\] Use != 0 instead of \> 0 for unsigned integer comparison](#gas-22-use--0-instead-of--0-for-unsigned-integer-comparison)
    - [\[GAS-23\] `internal` functions not called by the contract should be removed](#gas-23-internal-functions-not-called-by-the-contract-should-be-removed)
  - [Non Critical Issues](#non-critical-issues)
    - [\[NC-1\] Replace `abi.encodeWithSignature` and `abi.encodeWithSelector` with `abi.encodeCall` which keeps the code typo/type safe](#nc-1-replace-abiencodewithsignature-and-abiencodewithselector-with-abiencodecall-which-keeps-the-code-typotype-safe)
    - [\[NC-2\] Missing checks for `address(0)` when assigning values to address state variables](#nc-2-missing-checks-for-address0-when-assigning-values-to-address-state-variables)
    - [\[NC-3\] Array indices should be referenced via `enum`s rather than via numeric literals](#nc-3-array-indices-should-be-referenced-via-enums-rather-than-via-numeric-literals)
    - [\[NC-4\] Constants should be in CONSTANT\_CASE](#nc-4-constants-should-be-in-constant_case)
    - [\[NC-5\] `constant`s should be defined rather than using magic numbers](#nc-5-constants-should-be-defined-rather-than-using-magic-numbers)
    - [\[NC-6\] Control structures do not follow the Solidity Style Guide](#nc-6-control-structures-do-not-follow-the-solidity-style-guide)
    - [\[NC-7\] Critical Changes Should Use Two-step Procedure](#nc-7-critical-changes-should-use-two-step-procedure)
    - [\[NC-8\] Consider disabling `renounceOwnership()`](#nc-8-consider-disabling-renounceownership)
    - [\[NC-9\] Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function](#nc-9-duplicated-requirerevert-checks-should-be-refactored-to-a-modifier-or-function)
    - [\[NC-10\] Event is never emitted](#nc-10-event-is-never-emitted)
    - [\[NC-11\] Events should use parameters to convey information](#nc-11-events-should-use-parameters-to-convey-information)
    - [\[NC-12\] Event missing indexed field](#nc-12-event-missing-indexed-field)
    - [\[NC-13\] Events that mark critical parameter changes should contain both the old and the new value](#nc-13-events-that-mark-critical-parameter-changes-should-contain-both-the-old-and-the-new-value)
    - [\[NC-14\] Function ordering does not follow the Solidity style guide](#nc-14-function-ordering-does-not-follow-the-solidity-style-guide)
    - [\[NC-15\] Functions should not be longer than 50 lines](#nc-15-functions-should-not-be-longer-than-50-lines)
    - [\[NC-16\] Change int to int256](#nc-16-change-int-to-int256)
    - [\[NC-17\] Change uint to uint256](#nc-17-change-uint-to-uint256)
    - [\[NC-18\] Interfaces should be defined in separate files from their usage](#nc-18-interfaces-should-be-defined-in-separate-files-from-their-usage)
    - [\[NC-19\] Lack of checks in setters](#nc-19-lack-of-checks-in-setters)
    - [\[NC-20\] Lines are too long](#nc-20-lines-are-too-long)
    - [\[NC-21\] `type(uint256).max` should be used instead of `2 ** 256 - 1`](#nc-21-typeuint256max-should-be-used-instead-of-2--256---1)
    - [\[NC-22\] Missing Event for critical parameters change](#nc-22-missing-event-for-critical-parameters-change)
    - [\[NC-23\] NatSpec is completely non-existent on functions that should have them](#nc-23-natspec-is-completely-non-existent-on-functions-that-should-have-them)
    - [\[NC-24\] File's first line is not an SPDX Identifier](#nc-24-files-first-line-is-not-an-spdx-identifier)
    - [\[NC-25\] Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor](#nc-25-use-a-modifier-instead-of-a-requireif-statement-for-a-special-msgsender-actor)
    - [\[NC-26\] Constant state variables defined more than once](#nc-26-constant-state-variables-defined-more-than-once)
    - [\[NC-27\] Consider using named mappings](#nc-27-consider-using-named-mappings)
    - [\[NC-28\] Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables](#nc-28-variable-names-that-consist-of-all-capital-letters-should-be-reserved-for-constantimmutable-variables)
    - [\[NC-29\] Owner can renounce while system is paused](#nc-29-owner-can-renounce-while-system-is-paused)
    - [\[NC-30\] Adding a `return` statement when the function defines a named return variable, is redundant](#nc-30-adding-a-return-statement-when-the-function-defines-a-named-return-variable-is-redundant)
    - [\[NC-31\] `require()` / `revert()` statements should have descriptive reason strings](#nc-31-requirerevertstatements-should-have-descriptive-reason-strings)
    - [\[NC-32\] Take advantage of Custom Error's return value property](#nc-32-take-advantage-of-custom-errors-return-value-property)
    - [\[NC-33\] Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. `10**18`)](#nc-33-use-scientific-notation-eg-1e18-rather-than-exponentiation-eg-1018)
    - [\[NC-34\] Use scientific notation for readability reasons for large multiples of ten](#nc-34-use-scientific-notation-for-readability-reasons-for-large-multiples-of-ten)
    - [\[NC-35\] Avoid the use of sensitive terms](#nc-35-avoid-the-use-of-sensitive-terms)
    - [\[NC-36\] Contract does not follow the Solidity style guide's suggested layout ordering](#nc-36-contract-does-not-follow-the-solidity-style-guides-suggested-layout-ordering)
    - [\[NC-37\] Use Underscores for Number Literals (add an underscore every 3 digits)](#nc-37-use-underscores-for-number-literals-add-an-underscore-every-3-digits)
    - [\[NC-38\] Internal and private variables and functions names should begin with an underscore](#nc-38-internal-and-private-variables-and-functions-names-should-begin-with-an-underscore)
    - [\[NC-39\] Event is missing `indexed` fields](#nc-39-event-is-missing-indexed-fields)
    - [\[NC-40\] Constants should be defined rather than using magic numbers](#nc-40-constants-should-be-defined-rather-than-using-magic-numbers)
    - [\[NC-41\] `public` functions not called by the contract should be declared `external` instead](#nc-41-public-functions-not-called-by-the-contract-should-be-declared-external-instead)
    - [\[NC-42\] Variables need not be initialized to zero](#nc-42-variables-need-not-be-initialized-to-zero)
  - [Low Issues](#low-issues)
    - [\[L-1\] `approve()`/`safeApprove()` may revert if the current approval is not zero](#l-1-approvesafeapprove-may-revert-if-the-current-approval-is-not-zero)
    - [\[L-2\] Use a 2-step ownership transfer pattern](#l-2-use-a-2-step-ownership-transfer-pattern)
    - [\[L-3\] Some tokens may revert when zero value transfers are made](#l-3-some-tokens-may-revert-when-zero-value-transfers-are-made)
    - [\[L-4\] Missing checks for `address(0)` when assigning values to address state variables](#l-4-missing-checks-for-address0-when-assigning-values-to-address-state-variables)
    - [\[L-5\] `decimals()` is not a part of the ERC-20 standard](#l-5-decimals-is-not-a-part-of-the-erc-20-standard)
    - [\[L-6\] Deprecated approve() function](#l-6-deprecated-approve-function)
    - [\[L-7\] Division by zero not prevented](#l-7-division-by-zero-not-prevented)
    - [\[L-8\] `domainSeparator()` isn't protected against replay attacks in case of a future chain split](#l-8-domainseparator-isnt-protected-against-replay-attacks-in-case-of-a-future-chain-split)
    - [\[L-9\] Duplicate import statements](#l-9-duplicate-import-statements)
    - [\[L-10\] Empty Function Body - Consider commenting why](#l-10-empty-function-body---consider-commenting-why)
    - [\[L-11\] Empty `receive()/payable fallback()` function does not authenticate requests](#l-11-empty-receivepayable-fallback-function-does-not-authenticate-requests)
    - [\[L-12\] External call recipient may consume all transaction gas](#l-12-external-call-recipient-may-consume-all-transaction-gas)
    - [\[L-13\] Initializers could be front-run](#l-13-initializers-could-be-front-run)
    - [\[L-14\] Signature use at deadlines should be allowed](#l-14-signature-use-at-deadlines-should-be-allowed)
    - [\[L-15\] Prevent accidentally burning tokens](#l-15-prevent-accidentally-burning-tokens)
    - [\[L-16\] NFT ownership doesn't support hard forks](#l-16-nft-ownership-doesnt-support-hard-forks)
    - [\[L-17\] Owner can renounce while system is paused](#l-17-owner-can-renounce-while-system-is-paused)
    - [\[L-18\] Possible rounding issue](#l-18-possible-rounding-issue)
    - [\[L-19\] Loss of precision](#l-19-loss-of-precision)
    - [\[L-20\] Solidity version 0.8.20+ may not work on other chains due to `PUSH0`](#l-20-solidity-version-0820-may-not-work-on-other-chains-due-to-push0)
    - [\[L-21\] Use `Ownable2Step.transferOwnership` instead of `Ownable.transferOwnership`](#l-21-use-ownable2steptransferownership-instead-of-ownabletransferownership)
    - [\[L-22\] File allows a version of solidity that is susceptible to an assembly optimizer bug](#l-22-file-allows-a-version-of-solidity-that-is-susceptible-to-an-assembly-optimizer-bug)
    - [\[L-23\] `symbol()` is not a part of the ERC-20 standard](#l-23-symbol-is-not-a-part-of-the-erc-20-standard)
    - [\[L-24\] Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when downcasting](#l-24-consider-using-openzeppelins-safecast-library-to-prevent-unexpected-overflows-when-downcasting)
    - [\[L-25\] Unsafe ERC20 operation(s)](#l-25-unsafe-erc20-operations)
    - [\[L-26\] Unspecific compiler version pragma](#l-26-unspecific-compiler-version-pragma)
    - [\[L-27\] Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions](#l-27-upgradeable-contract-is-missing-a-__gap50-storage-variable-to-allow-for-new-storage-variables-in-later-versions)
    - [\[L-28\] Upgradeable contract not initialized](#l-28-upgradeable-contract-not-initialized)
  - [Medium Issues](#medium-issues)
    - [\[M-1\] Contracts are vulnerable to fee-on-transfer accounting-related issues](#m-1-contracts-are-vulnerable-to-fee-on-transfer-accounting-related-issues)
    - [\[M-2\] `block.number` means different things on different L2s](#m-2-blocknumber-means-different-things-on-different-l2s)
    - [\[M-3\] Centralization Risk for trusted owners](#m-3-centralization-risk-for-trusted-owners)
      - [Impact](#impact)
    - [\[M-4\] `_safeMint()` should be used rather than `_mint()` wherever possible](#m-4-_safemint-should-be-used-rather-than-_mint-wherever-possible)
    - [\[M-5\] `increaseAllowance/decreaseAllowance` won't work on mainnet for USDT](#m-5-increaseallowancedecreaseallowance-wont-work-on-mainnet-for-usdt)
    - [\[M-6\] Direct `supportsInterface()` calls may cause caller to revert](#m-6-direct-supportsinterface-calls-may-cause-caller-to-revert)
    - [\[M-7\] Return values of `transfer()`/`transferFrom()` not checked](#m-7-return-values-of-transfertransferfrom-not-checked)
    - [\[M-8\] Unsafe use of `transfer()`/`transferFrom()`/`approve()`/ with `IERC20`](#m-8-unsafe-use-of-transfertransferfromapprove-with-ierc20)
  - [High Issues](#high-issues)
    - [\[H-1\] IERC20.approve() will revert for USDT](#h-1-ierc20approve-will-revert-for-usdt)

## Gas Optimizations

| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | Use ERC721A instead ERC721 | 1 |
| [GAS-2](#GAS-2) | Don't use `_msgSender()` if not supporting EIP-2771 | 51 |
| [GAS-3](#GAS-3) | `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings) | 29 |
| [GAS-4](#GAS-4) | Use assembly to check for `address(0)` | 65 |
| [GAS-5](#GAS-5) | `array[index] += amount` is cheaper than `array[index] = array[index] + amount` (or related variants) | 5 |
| [GAS-6](#GAS-6) | Using bools for storage incurs overhead | 18 |
| [GAS-7](#GAS-7) | Cache array length outside of loop | 14 |
| [GAS-8](#GAS-8) | State variables should be cached in stack variables rather than re-reading them from storage | 63 |
| [GAS-9](#GAS-9) | Use calldata instead of memory for function arguments that do not get mutated | 109 |
| [GAS-10](#GAS-10) | For Operations that will not overflow, you could use unchecked | 688 |
| [GAS-11](#GAS-11) | Use Custom Errors instead of Revert Strings to save Gas | 159 |
| [GAS-12](#GAS-12) | Avoid contract existence checks by using low level calls | 33 |
| [GAS-13](#GAS-13) | Stack variable used as a cheaper cache for a state variable is only used once | 18 |
| [GAS-14](#GAS-14) | State variables only set in the constructor should be declared `immutable` | 11 |
| [GAS-15](#GAS-15) | Functions guaranteed to revert when called by normal users can be marked `payable` | 91 |
| [GAS-16](#GAS-16) | `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`) | 29 |
| [GAS-17](#GAS-17) | Using `private` rather than `public` for constants, saves gas | 26 |
| [GAS-18](#GAS-18) | Use shift right/left instead of division/multiplication if possible | 3 |
| [GAS-19](#GAS-19) | Splitting require() statements that use && saves gas | 5 |
| [GAS-20](#GAS-20) | `uint256` to `bool` `mapping`: Utilizing Bitmaps to dramatically save on Gas | 4 |
| [GAS-21](#GAS-21) | Increments/decrements can be unchecked in for-loops | 21 |
| [GAS-22](#GAS-22) | Use != 0 instead of > 0 for unsigned integer comparison | 54 |
| [GAS-23](#GAS-23) | `internal` functions not called by the contract should be removed | 24 |

### <a name="GAS-1"></a>[GAS-1] Use ERC721A instead ERC721

ERC721A standard, ERC721A is an improvement standard for ERC721 tokens. It was proposed by the Azuki team and used for developing their NFT collection. Compared with ERC721, ERC721A is a more gas-efficient standard to mint a lot of of NFTs simultaneously. It allows developers to mint multiple NFTs at the same gas price. This has been a great improvement due to Ethereum's sky-rocketing gas fee.

    Reference: https://nextrope.com/erc721-vs-erc721a-2/

*Instances (1)*:

```solidity
File: contracts/virtualPersona/AgentDAO.sol

10: import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

### <a name="GAS-2"></a>[GAS-2] Don't use `_msgSender()` if not supporting EIP-2771

Use `msg.sender` if the code does not implement [EIP-2771 trusted forwarder](https://eips.ethereum.org/EIPS/eip-2771) support

*Instances (51)*:

```solidity
File: contracts/AgentInference.sol

41:         address sender = _msgSender();

68:         address sender = _msgSender();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

48:         _admin = _msgSender();

104:         require(_msgSender() == _admin, "Only admin can set admin");

166:         require(_msgSender() == _admin, "Only admin can set elo calculator");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

52:         __Ownable_init(_msgSender());

60:         require(_msgSender() == info.dao, "Caller is not VIRTUAL DAO");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/FERC20.sol

35:         _balances[_msgSender()] = _totalSupply;

37:         isExcludedFromMaxTx[_msgSender()] = true;

43:         emit Transfer(address(0), _msgSender(), _totalSupply);

67:         _transfer(_msgSender(), recipient, amount);

77:         _approve(_msgSender(), spender, amount);

85:         _approve(sender, _msgSender(), _allowances[sender][_msgSender()] - amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

44:         _grantRole(DEFAULT_ADMIN_ROLE, _msgSender());

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/libs/TokenSaver.sol

16:         require(hasRole(TOKEN_SAVER_ROLE, _msgSender()), "TokenSaver.onlyTokenSaver: permission denied");

22:         emit TokenSaved(_msgSender(), _receiver, _token, _amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/tax/AgentTax.sol

256:         address sender = _msgSender();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

50:         require(_msgSender() == address(bondingRouter), "Only bonding router");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

37:         IERC20(token).safeTransfer(_msgSender(), IERC20(token).balanceOf(address(this)));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

82:         address proposer = _msgSender();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

146:         address sender = _msgSender();

400:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

401:         sender = ContextUpgradeable._msgSender();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

149:         address sender = _msgSender();

414:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

415:         sender = ContextUpgradeable._msgSender();

433:         address sender = _msgSender();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

147:         address sender = _msgSender();

438:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

439:         sender = ContextUpgradeable._msgSender();

460:         address sender = _msgSender();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

42:     constructor(address agentNft_) Ownable(_msgSender()) {

105:         require(founder == _msgSender(), "Not founder");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

34:         require(_msgSender() == virtualInfos[virtualId].dao, "Caller is not VIRTUAL DAO");

39:         require(_msgSender() == _serviceNft, "Caller is not Service NFT");

180:         require(_msgSender() == virtualInfos[virtualId].dao, "Caller is not VIRTUAL DAO");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

69:         if (owner() != _msgSender() && address(_factory) != _msgSender()) {

108:         _factory = IAgentFactory(_msgSender());

499:         address owner = _msgSender();

522:         address owner = _msgSender();

544:         address spender = _msgSender();

563:         address owner = _msgSender();

583:         address owner = _msgSender();

869:         (bool success, ) = _msgSender().call{value: amount_}("");

895:         IERC20(token_).safeTransfer(_msgSender(), amount_);

1011:         _burn(_msgSender(), value);

1026:         _spendAllowance(account, _msgSender(), value);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

62:         address sender = _msgSender();

85:         require(_msgSender() == founder, "Not founder");

91:         require(IAccessControl(agentNft).hasRole(ADMIN_ROLE, _msgSender()), "Not admin");

96:         address sender = _msgSender();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="GAS-3"></a>[GAS-3] `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings)

This saves **16 gas per instance.**

*Instances (29)*:

```solidity
File: contracts/AgentInference.sol

47:             total += amounts[i];

75:             total += amounts[i];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/genesis/Genesis.sol

198:         mapAddrToVirtuals[msg.sender] += virtualsAmt;

230:             totalRefundAmount += refundVirtualsTokenUserAmounts[i];

277:             totalDistributionAmount += distributeAgentTokenUserAmounts[i];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/GovernorCountingSimple.sol

98:             proposalVote.againstVotes += weight;

100:             proposalVote.forVotes += weight;

102:             proposalVote.abstainVotes += weight;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/GovernorCountingSimple.sol)

```solidity
File: contracts/tax/AgentTax.sol

183:             totalAmount += amounts[i];

186:         agentAmounts.amountCollected += totalAmount;

246:             agentAmounts.amountSwapped += amountToSwap;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

55:             total += amount;

63:         refunds[txhash] += amount;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

80:             _agentPaidAmounts[agentId] += bonus;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

174:         _proposalMaturities[proposalId] += (maturity * weight);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

622:         _balances[to] += amountMinusTax;

699:                         projectTaxPendingSwap += uint128(projectTax);

700:                         tax += projectTax;

707:                         projectTaxPendingSwap += uint128(projectTax);

708:                         tax += projectTax;

713:                     _balances[address(this)] += tax;

914:         _totalSupply += uint128(amount);

917:             _balances[account] += amount;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

48:                 eloB += change;

50:                 eloA += change;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

100:             proposalVote.againstVotes += weight;

102:             proposalVote.forVotes += weight;

104:             proposalVote.abstainVotes += weight;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

63:             totalScore += validatorScore(virtualId, validatorAt(virtualId, i));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="GAS-4"></a>[GAS-4] Use assembly to check for `address(0)`

*Saves 6 gas per instance*

*Instances (65)*:

```solidity
File: contracts/fun/FERC20.sol

91:         require(owner != address(0), "ERC20: approve from the zero address");

92:         require(spender != address(0), "ERC20: approve to the zero address");

100:         require(from != address(0), "ERC20: transfer from the zero address");

101:         require(to != address(0), "ERC20: transfer to the zero address");

126:         require(user != address(0), "ERC20: Exclude Max Tx from the zero address");

132:         require(user != address(0), "Invalid address");

137:         require(user != address(0), "Invalid address");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

42:         require(tokenA != address(0), "Zero addresses are not allowed.");

43:         require(tokenB != address(0), "Zero addresses are not allowed.");

44:         require(router != address(0), "No router");

75:         require(newVault_ != address(0), "Zero addresses are not allowed.");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

27:         require(router_ != address(0), "Zero addresses are not allowed.");

28:         require(token0 != address(0), "Zero addresses are not allowed.");

29:         require(token1 != address(0), "Zero addresses are not allowed.");

72:         require(_user != address(0), "Zero addresses are not allowed.");

73:         require(_token != address(0), "Zero addresses are not allowed.");

83:         require(recipient != address(0), "Zero addresses are not allowed.");

89:         require(recipient != address(0), "Zero addresses are not allowed.");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

34:         require(factory_ != address(0), "Zero addresses are not allowed.");

35:         require(assetToken_ != address(0), "Zero addresses are not allowed.");

46:         require(token != address(0), "Zero addresses are not allowed.");

80:         require(token_ != address(0), "Zero addresses are not allowed.");

100:         require(tokenAddress != address(0), "Zero addresses are not allowed.");

101:         require(to != address(0), "Zero addresses are not allowed.");

136:         require(tokenAddress != address(0), "Zero addresses are not allowed.");

137:         require(to != address(0), "Zero addresses are not allowed.");

166:         require(tokenAddress != address(0), "Zero addresses are not allowed.");

178:         require(spender != address(0), "Zero addresses are not allowed.");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

59:             p.virtualToken != address(0) &&

60:                 p.feeAddr != address(0) &&

61:                 p.tbaImpl != address(0) &&

62:                 p.agentFactory != address(0),

112:         require(addr != address(0), "Not found");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

105:         require(agentTokenAddress == address(0), ERR_TOKEN_LAUNCHED);

110:         require(agentTokenAddress != address(0), ERR_TOKEN_NOT_LAUNCHED);

125:         require(isFailed || isCancelled || agentTokenAddress != address(0), "Genesis not finalized yet");

138:         require(params.factory != address(0), "Invalid factory address");

143:         require(params.tbaImplementation != address(0), "Invalid TBA implementation address");

144:         require(params.agentFactoryAddress != address(0), "Invalid agent factory address");

145:         require(params.virtualTokenAddress != address(0), "Invalid virtual token address");

234:         bool isFirstLaunch = agentTokenAddress == address(0);

268:             require(agentToken != address(0), "Agent token creation failed");

453:         require(token != address(0), "Invalid token address");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/tax/AgentTax.sol

192:         if (recipient.tba == address(0)) {

209:         require(taxRecipient.tba != address(0), "Agent does not have TBA");

237:                 if (address(tbaBonus) != address(0)) {

258:         if (recipient.tba == address(0)) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/TBABonus.sol

66:         require(recipient != address(0), "Invalid recipient");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

162:         if (owner == address(0)) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

207:         require(_tokenAdmin != address(0), "Token admin not set");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

202:         require(_tokenAdmin != address(0), "Token admin not set");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

197:         if (customToken != address(0)) {

208:         require(_tokenAdmin != address(0), "Token admin not set");

219:         if (token == address(0)) {

518:         require(_applicationToken[id] != address(0), "Not custom token application");

546:         require(factory.getPair(tokenAddr, assetToken) == address(0), "pool already exists");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

175:         require(info.tba == address(0), "TBA already set");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

196:         if (uniswapV2Pair_ == address(0)) {

302:         if (newLiquidityPool_ == address(0)) {

656:         if (from_ == address(0)) {

660:         if (to_ == address(0)) {

908:         if (account == address(0)) {

936:         if (account == address(0)) {

972:         if (owner == address(0)) {

976:         if (spender == address(0)) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="GAS-5"></a>[GAS-5] `array[index] += amount` is cheaper than `array[index] = array[index] + amount` (or related variants)

When updating a value in an array with arithmetic, using `array[index] += amount` is cheaper than `array[index] = array[index] + amount`.

This is because you avoid an additional `mload` when the array is stored in memory, and an `sload` when the array is stored in storage.

This can be applied for any arithmetic operation including `+=`, `-=`,`/=`,`*=`,`^=`,`&=`, `%=`, `<<=`,`>>=`, and `>>>=`.

This optimization can be particularly significant if the pattern occurs during a loop.

*Saves 28 gas for a storage array, 38 for a memory array*

*Instances (5)*:

```solidity
File: contracts/contribution/ServiceNft.sol

102:             _impacts[proposalId] = rawImpact - _impacts[datasetId];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/FERC20.sol

108:         _balances[from] = _balances[from] - amount;

109:         _balances[to] = _balances[to] + amount;

133:         _balances[user] = _balances[user] - amount;

138:         _balances[user] = _balances[user] - amount;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

### <a name="GAS-6"></a>[GAS-6] Using bools for storage incurs overhead

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

*Instances (18)*:

```solidity
File: contracts/contribution/ContributionNft.sol

28:     mapping(uint256 => bool) public modelContributions;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/fun/FERC20.sol

24:     mapping(address => bool) private isExcludedFromMaxTx;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/genesis/Genesis.sol

46:     bool public isFailed;

47:     bool public isCancelled;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/GovernorCountingSimple.sol

26:         mapping(address voter => bool) hasVoted;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/GovernorCountingSimple.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

24:     mapping(uint256 => bool) _earlyExecutions;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

77:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

77:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

72:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

29:     mapping(uint256 => bool) public migratedAgents;

31:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

50:     mapping(uint256 => bool) private _blacklists;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

26:     bool internal _tokenHasTax;

39:     bool private _autoSwapInProgress;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

21:     bool public canStake; // To control private/public agent mode

30:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

26:         mapping(address voter => bool) hasVoted;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

8:     mapping(uint256 virtualId => mapping(address account => bool isValidator)) private _validatorsMap;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="GAS-7"></a>[GAS-7] Cache array length outside of loop

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (14)*:

```solidity
File: contracts/AgentInference.sol

46:         for (uint256 i = 0; i < amounts.length; i++) {

52:         for (uint256 i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/Bonding.sol

412:         for (uint i = 0; i < accounts.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/Genesis.sol

224:         for (uint256 i = 0; i < refundVirtualsTokenUserAmounts.length; i++) {

276:         for (uint256 i = 0; i < distributeAgentTokenUserAmounts.length; i++) {

286:         for (uint256 i = 0; i < refundVirtualsTokenUserAddresses.length; i++) {

298:         for (uint256 i = 0; i < distributeAgentTokenUserAddresses.length; i++) {

327:         for (uint256 i = 0; i < participantIndexes.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

21:         for (uint256 i = 0; i < froms.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/tax/AgentTax.sol

177:         for (uint i = 0; i < txhashes.length; i++) {

272:         for (uint i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

47:         for (uint i = 0; i < txhashes.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

53:         for (uint256 i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

42:         for (uint256 i = 0; i < battles.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="GAS-8"></a>[GAS-8] State variables should be cached in stack variables rather than re-reading them from storage

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*Saves 100 gas per instance*

*Instances (63)*:

```solidity
File: contracts/contribution/ServiceNft.sol

76:         bool isModel = IContributionNft(contributionNft).isModel(proposalId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

274:         IERC20(assetToken).forceApprove(address(router), initialPurchase);

378:         uint256 id = IAgentFactoryV3(agentFactory).initFromBondingCurve(

390:         address agentToken = IAgentFactoryV3(agentFactory).executeBondingCurveApplication(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

35:         _balances[_msgSender()] = _totalSupply;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FRouter.sol

148:         IERC20(assetToken).safeTransferFrom(to, pair, amount);

150:         IERC20(assetToken).safeTransferFrom(to, feeTo, txFee);

152:         uint256 amountOut = getAmountsOut(tokenAddress, assetToken, amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

106:         emit GenesisCreated(genesisID, addr);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

188:             IERC20(virtualTokenAddress).allowance(msg.sender, address(this)) >= virtualsAmt,

200:         IERC20(virtualTokenAddress).safeTransferFrom(msg.sender, address(this), virtualsAmt);

239:             IERC20(virtualTokenAddress).balanceOf(address(this)) >= requiredVirtualsBalance,

249:             uint256 id = IAgentFactoryV3(agentFactoryAddress).initFromBondingCurve(

257:                 reserveAmount,

261:             address agentToken = IAgentFactoryV3(agentFactoryAddress).executeBondingCurveApplication(

290:             IERC20(virtualTokenAddress).safeTransfer(

302:         emit GenesisSucceeded(genesisId);

345:             emit GenesisFailed(genesisId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

53:         IERC20(tokenIn).safeTransferFrom(msg.sender, address(this), amountIn);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/tax/AgentTax.sol

133:         IERC20(taxToken).forceApprove(router_, type(uint256).max);

143:             creatorFeeRate

220:         path[0] = taxToken;

236:                 IERC20(assetToken).safeTransfer(taxRecipient.creator, creatorFee);

243:                 IERC20(assetToken).safeTransfer(treasury, feeAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

96:         IERC20(taxToken).forceApprove(oldRouter, 0);

136:         path[0] = taxToken;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

181:         address serviceNft = IAgentNft(_agentNft).getServiceNft();

189:             maturity = IEloCalculator(IAgentNft(_agentNft).getEloCalculator()).battleElo(maturity, votes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

149:             IERC20(assetToken).allowance(sender, address(this)) >= applicationThreshold,

154:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

163:             applicationThreshold,

242:         IAgentNft(nft).mint(

263:             nft,

266:         IAgentNft(nft).setTBA(virtualId, tbaAddress);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

152:             IERC20(assetToken).allowance(sender, address(this)) >= applicationThreshold,

157:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

166:             applicationThreshold,

235:         IAgentNft(nft).mint(

256:             nft,

259:         IAgentNft(nft).setTBA(virtualId, tbaAddress);

436:             IERC20(assetToken).allowance(sender, address(this)) >= applicationThreshold_,

441:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

150:             IERC20(assetToken).allowance(sender, address(this)) >= applicationThreshold,

155:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

164:             applicationThreshold,

228:             IERC20(assetToken).forceApprove(_uniswapRouter, initialAmount);

228:             IERC20(assetToken).forceApprove(_uniswapRouter, initialAmount);

230:             IUniswapV2Router02(_uniswapRouter).addLiquidity(

232:                 assetToken,

259:         IAgentNft(nft).mint(

280:             nft,

283:         IAgentNft(nft).setTBA(virtualId, tbaAddress);

468:             IERC20(assetToken).allowance(sender, address(this)) >= applicationThreshold,

478:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

489:             applicationThreshold,

548:         uniswapV2Pair_ = factory.createPair(tokenAddr, assetToken);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

197:             uniswapV2Pair_ = IUniswapV2Factory(_uniswapRouter.factory()).createPair(address(this), pairToken);

244:         IERC20(pairToken).approve(address(_uniswapRouter), type(uint256).max);

248:             pairToken,

250:             IERC20(pairToken).balanceOf(address(this)),

773:             to_ != address(_uniswapRouter));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

65:         require(IERC20(assetToken).allowance(sender, address(this)) >= amount, "Insufficient asset token allowance");

78:         IERC20(assetToken).safeTransferFrom(sender, address(this), amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="GAS-9"></a>[GAS-9] Use calldata instead of memory for function arguments that do not get mutated

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly bypasses this loop.

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it's still more gas-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one.

 *Saves 60 gas per instance*

*Instances (109)*:

```solidity
File: contracts/AgentInference.sol

37:         uint256[] memory agentIds,

38:         uint256[] memory amounts,

39:         uint8[][] memory coreIds

63:         bytes32[] memory promptHashes,

64:         uint256[] memory agentIds,

65:         uint256[] memory amounts,

66:         uint8[][] memory coreIds

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

69:         string memory newTokenURI,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/fun/Bonding.sol

166:     function setDeployParams(DeployParams memory params) public onlyOwner {

179:         string memory _name,

180:         string memory _ticker,

181:         uint8[] memory cores,

182:         string memory desc,

183:         string memory img,

184:         string[4] memory urls,

191:         string memory _name,

192:         string memory _ticker,

193:         uint8[] memory cores,

194:         string memory desc,

195:         string memory img,

196:         string[4] memory urls,

405:     function unwrapToken(address srcTokenAddress, address[] memory accounts) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/FGenesis.sol

46:     function initialize(Params memory p) external initializer {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

49:         string memory,

50:         string memory,

51:         string memory,

52:         uint8[] memory,

70:         string memory,

71:         string memory,

72:         uint8[] memory,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

62:         address[] memory targets,

63:         uint256[] memory values,

64:         bytes[] memory calldatas,

65:         string memory description

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/VirtualProtocolDAO.sol

47:         address[] memory targets,

48:         uint256[] memory values,

49:         bytes[] memory calldatas,

50:         string memory description

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualProtocolDAO.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

16:         address[] memory froms,

17:         address[] memory tos,

18:         uint256[] memory values

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

24:     function getAmountsOut(uint256 amountIn, Route[] memory routes) external view returns (uint256[] memory amounts);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/tax/AgentTax.sol

170:         bytes32[] memory txhashes,

171:         uint256[] memory amounts,

269:     function dcaSell(uint256[] memory agentIds, uint256 slippage, uint256 maxOverride) public onlyRole(EXECUTOR_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

42:         bytes32[] memory txhashes,

43:         uint256[] memory amounts

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

50:     function setAllowances(uint256[] memory agentIds, uint256[] memory allowances) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

41:         string memory name,

77:         address[] memory targets,

78:         uint256[] memory values,

79:         bytes[] memory calldatas,

80:         string memory description

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

137:         string memory name,

138:         string memory symbol,

139:         string memory tokenURI,

140:         uint8[] memory cores,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

140:         string memory name,

141:         string memory symbol,

142:         string memory tokenURI,

143:         uint8[] memory cores,

423:         string memory name,

424:         string memory symbol,

425:         uint8[] memory cores,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

138:         string memory name,

139:         string memory symbol,

140:         string memory tokenURI,

141:         uint8[] memory cores,

453:         uint8[] memory cores,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

100:     function migrateAgent(uint256 id, string memory name, string memory symbol, bool canStake) external noReentrant {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

88:         string memory newTokenURI,

91:         uint8[] memory coreTypes,

116:     function addCoreType(string memory label) public onlyRole(DEFAULT_ADMIN_ROLE) {

163:     function setCoreTypes(uint256 virtualId, uint8[] memory coreTypes) external onlyVirtualDAO(virtualId) {

169:     function setTokenURI(uint256 virtualId, string memory newTokenURI) public onlyVirtualDAO(virtualId) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

80:         address[3] memory integrationAddresses_,

81:         bytes memory baseParams_,

82:         bytes memory supplyParams_,

83:         bytes memory taxParams_

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

40:         string memory _name,

41:         string memory _symbol,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

39:     function battleElo(uint256 currentRating, uint8[] memory battles) public view returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/IAgentDAO.sol

9:         string memory name,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentDAO.sol)

```solidity
File: contracts/virtualPersona/IAgentFactory.sol

8:         string memory name,

9:         string memory symbol,

10:         string memory tokenURI,

11:         uint8[] memory cores,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentFactory.sol)

```solidity
File: contracts/virtualPersona/IAgentFactoryV3.sol

8:         string memory name,

9:         string memory symbol,

10:         string memory tokenURI,

11:         uint8[] memory cores,

23:         string memory name,

24:         string memory symbol,

25:         uint8[] memory cores,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/IAgentFactoryV4.sol

8:         string memory name,

9:         string memory symbol,

10:         string memory tokenURI,

11:         uint8[] memory cores,

24:         uint8[] memory cores,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/IAgentNft.sol

25:         string memory newTokenURI,

28:         uint8[] memory coreTypes,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentNft.sol)

```solidity
File: contracts/virtualPersona/IAgentToken.sol

256:         address[3] memory integrationAddresses_,

257:         bytes memory baseParams_,

258:         bytes memory supplyParams_,

259:         bytes memory taxParams_

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentToken.sol)

```solidity
File: contracts/virtualPersona/IAgentVeToken.sol

6:         string memory _name,

7:         string memory _symbol,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentVeToken.sol)

```solidity
File: contracts/virtualPersona/IEloCalculator.sol

5:     function battleElo(uint256 currentRating, uint8[] memory battles) external view returns (uint256);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IEloCalculator.sol)

```solidity
File: contracts/virtualPersona/IExecutionInterface.sol

5:     function execute(address to, uint256 value, bytes memory data, uint8 operation) external returns (bool success);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IExecutionInterface.sol)

### <a name="GAS-10"></a>[GAS-10] For Operations that will not overflow, you could use unchecked

*Instances (688)*:

```solidity
File: contracts/AgentInference.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

4: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "@openzeppelin/contracts/utils/math/Math.sol";

7: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

8: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

9: import "@openzeppelin/contracts/utils/math/Math.sol";

10: import "./virtualPersona/IAgentNft.sol";

17:     mapping(uint256 agentId => uint256) public inferenceCount; // Inference count per agent

46:         for (uint256 i = 0; i < amounts.length; i++) {

47:             total += amounts[i];

52:         for (uint256 i = 0; i < agentIds.length; i++) {

57:             inferenceCount[agentId]++;

74:         for (uint256 i = 0; i < len; i++) {

75:             total += amounts[i];

82:         for (uint256 i = 0; i < len; i++) {

89:             inferenceCount[agentId]++;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol";

8: import "@openzeppelin/contracts/access/AccessControl.sol";

9: import "@openzeppelin/contracts/interfaces/IERC5805.sol";

10: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

11: import "./IContributionNft.sol";

12: import "../virtualPersona/IAgentNft.sol";

33:     address private _admin; // Admin is able to create contribution proposal without votes

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/IContributionNft.sol

4: import "@openzeppelin/contracts/governance/IGovernor.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/IContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

4: import "@openzeppelin/contracts/governance/IGovernor.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol";

9: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

10: import "@openzeppelin/contracts/interfaces/IERC5805.sol";

11: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

12: import "../virtualPersona/IAgentNft.sol";

13: import "../virtualPersona/IAgentDAO.sol";

14: import "./IContributionNft.sol";

15: import "./IServiceNft.sol";

36:     mapping(uint256 personaId => mapping(uint8 coreId => uint256 serviceId)) private _coreServices; // Latest service NFT id for a core

95:             ? _maturities[proposalId] - _maturities[prevServiceId]

101:             _impacts[datasetId] = (rawImpact * datasetImpactWeight) / 10000;

102:             _impacts[proposalId] = rawImpact - _impacts[datasetId];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

5: import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

6: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

8: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

9: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

12: import "./FFactory.sol";

13: import "./IFPair.sol";

14: import "./FRouter.sol";

15: import "./FERC20.sol";

16: import "../virtualPersona/IAgentFactoryV3.sol";

111:         fee = (fee_ * 1 ether) / 1000;

203:         uint256 initialPurchase = (purchaseAmount - fee);

215:         uint256 k = ((K * 10000) / assetRate);

216:         uint256 liquidity = (((k * 10000 ether) / supply) * 1 ether) / 10000;

226:             price: supply / liquidity,

228:             liquidity: liquidity * 2,

231:             prevPrice: supply / liquidity,

247:             trading: true, // Can only be traded once creator made initial purchase

292:         uint256 newReserveA = reserveA + amount0In;

293:         uint256 newReserveB = reserveB - amount1Out;

294:         uint256 duration = block.timestamp - tokenInfo[tokenAddress].data.lastUpdated;

296:         uint256 liquidity = newReserveB * 2;

297:         uint256 mCap = (tokenInfo[tokenAddress].data.supply * newReserveB) / newReserveA;

298:         uint256 price = newReserveA / newReserveB;

299:         uint256 volume = duration > 86400 ? amount1Out : tokenInfo[tokenAddress].data.volume24H + amount1Out;

307:         tokenInfo[tokenAddress].data.volume = tokenInfo[tokenAddress].data.volume + amount1Out;

329:         uint256 newReserveA = reserveA - amount0Out;

330:         uint256 newReserveB = reserveB + amount1In;

331:         uint256 duration = block.timestamp - tokenInfo[tokenAddress].data.lastUpdated;

333:         uint256 liquidity = newReserveB * 2;

334:         uint256 mCap = (tokenInfo[tokenAddress].data.supply * newReserveB) / newReserveA;

335:         uint256 price = newReserveA / newReserveB;

336:         uint256 volume = duration > 86400 ? amount1In : tokenInfo[tokenAddress].data.volume24H + amount1In;

342:         tokenInfo[tokenAddress].data.volume = tokenInfo[tokenAddress].data.volume + amount1In;

392:             _token.data.supply / (10 ** token_.decimals()),

393:             tokenBalance / (10 ** token_.decimals()),

412:         for (uint i = 0; i < accounts.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

4: import "@openzeppelin/contracts/access/Ownable.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

33:         _totalSupply = supply * 10 ** _decimals;

85:         _approve(sender, _msgSender(), _allowances[sender][_msgSender()] - amount);

108:         _balances[from] = _balances[from] - amount;

109:         _balances[to] = _balances[to] + amount;

116:         _maxTxAmount = (maxTx * _totalSupply) / 100;

133:         _balances[user] = _balances[user] - amount;

138:         _balances[user] = _balances[user] - amount;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

8: import "./FPair.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

4: import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

8: import "./IFPair.sol";

48:         _pool = Pool({reserve0: reserve0, reserve1: reserve1, k: reserve0 * reserve1, lastUpdated: block.timestamp});

61:         uint256 _reserve0 = (_pool.reserve0 + amount0In) - amount0Out;

62:         uint256 _reserve1 = (_pool.reserve1 + amount1In) - amount1Out;

103:         return _pool.reserve1 / _pool.reserve0;

107:         return _pool.reserve0 / _pool.reserve1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

6: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

7: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

8: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

10: import "./FFactory.sol";

11: import "./IFPair.sol";

12: import "../tax/IBondingTax.sol";

59:             uint256 newReserveB = reserveB + amountIn;

61:             uint256 newReserveA = k / newReserveB;

63:             amountOut = reserveA - newReserveA;

65:             uint256 newReserveA = reserveA + amountIn;

67:             uint256 newReserveB = k / newReserveA;

69:             amountOut = reserveB - newReserveB;

114:         uint256 txFee = (fee * amountOut) / 100;

116:         uint256 amount = amountOut - txFee;

143:         uint256 txFee = (fee * amountIn) / 100;

146:         uint256 amount = amountIn - txFee;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

4: import {Genesis} from "./Genesis.sol";

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

6: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

7: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

8: import "../virtualPersona/AgentFactoryV3.sol";

9: import "./GenesisTypes.sol";

10: import "./GenesisLib.sol";

11: import "../virtualPersona/AgentFactoryV3.sol";

14:     using GenesisLib for *;

82:         gParams.endTime = gParams.startTime + params.duration;

84:         genesisID++;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

4: import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

6: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

7: import "./FGenesis.sol";

8: import "../virtualPersona/IAgentFactoryV3.sol";

10: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

11: import "./GenesisTypes.sol";

156:         factory = FGenesis(params.factory); // the FGenesis Proxy

198:         mapAddrToVirtuals[msg.sender] += virtualsAmt;

224:         for (uint256 i = 0; i < refundVirtualsTokenUserAmounts.length; i++) {

230:             totalRefundAmount += refundVirtualsTokenUserAmounts[i];

236:         uint256 requiredVirtualsBalance = isFirstLaunch ? totalRefundAmount + reserveAmount : totalRefundAmount;

265:                 address(this) // vault

276:         for (uint256 i = 0; i < distributeAgentTokenUserAmounts.length; i++) {

277:             totalDistributionAmount += distributeAgentTokenUserAmounts[i];

286:         for (uint256 i = 0; i < refundVirtualsTokenUserAddresses.length; i++) {

288:             mapAddrToVirtuals[refundVirtualsTokenUserAddresses[i]] -= refundVirtualsTokenUserAmounts[i];

298:         for (uint256 i = 0; i < distributeAgentTokenUserAddresses.length; i++) {

327:         for (uint256 i = 0; i < participantIndexes.length; i++) {

333:                 refundUserCountForFailed++;

365:         if (startIndex + pageSize > participants.length) {

366:             actualPageSize = participants.length - startIndex;

370:         for (uint256 i = 0; i < actualPageSize; i++) {

371:             page[i] = participants[startIndex + i];

388:         for (uint256 i = 0; i < length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/GenesisLib.sol

5: import {Genesis} from "./Genesis.sol";

6: import "./GenesisTypes.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/GenesisLib.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "../virtualPersona/IAgentFactoryV3.sol";

6: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

22:         address, // tokenImplementation

23:         address, // veTokenImplementation

24:         address, // daoImplementation

25:         address, // tbaRegistry

26:         address, // assetToken

27:         address, // nft

28:         uint256, // applicationThreshold

29:         address, // vault

30:         uint256 // nextId

66:         return 1; // Mock implementation returns 1

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/genesis/MockERC20.sol

4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockERC20.sol)

```solidity
File: contracts/governance/GovernorCountingSimple.sol

6: import "@openzeppelin/contracts/governance/Governor.sol";

62:         return quorum(proposalSnapshot(proposalId)) <= proposalVote.forVotes + proposalVote.abstainVotes;

82:         bytes memory // params

98:             proposalVote.againstVotes += weight;

100:             proposalVote.forVotes += weight;

102:             proposalVote.abstainVotes += weight;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/GovernorCountingSimple.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

5: import "@openzeppelin/contracts/governance/Governor.sol";

6: import "@openzeppelin/contracts/governance/extensions/GovernorSettings.sol";

7: import "@openzeppelin/contracts/governance/extensions/GovernorStorage.sol";

8: import "@openzeppelin/contracts/governance/extensions/GovernorVotes.sol";

9: import "@openzeppelin/contracts/governance/extensions/GovernorVotesQuorumFraction.sol";

10: import "./GovernorCountingSimple.sol";

11: import {Checkpoints} from "@openzeppelin/contracts/utils/structs/Checkpoints.sol";

12: import "@openzeppelin/contracts/access/AccessControl.sol";

84:         Checkpoints.Checkpoint224 memory latest = _quorumCheckpoints.at(SafeCast.toUint32(length - 1));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/VirtualProtocolDAO.sol

4: import "@openzeppelin/contracts/governance/Governor.sol";

5: import "@openzeppelin/contracts/governance/extensions/GovernorSettings.sol";

6: import "@openzeppelin/contracts/governance/extensions/GovernorStorage.sol";

7: import "@openzeppelin/contracts/governance/extensions/GovernorVotes.sol";

8: import "@openzeppelin/contracts/governance/extensions/GovernorVotesQuorumFraction.sol";

9: import "./GovernorCountingSimple.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualProtocolDAO.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

5: import "@openzeppelin/contracts/access/Ownable.sol";

6: import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol";

7: import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol";

21:         for (uint256 i = 0; i < froms.length; i++) {

29:     function approve(address /*spender*/, uint256 /*value*/) public override returns (bool) {

33:     function transfer(address /*to*/, uint256 /*value*/) public override returns (bool) {

37:     function transferFrom(address /*from*/, address /*to*/, uint256 /*value*/) public override returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

5: import {Math} from "@openzeppelin/contracts/utils/math/Math.sol";

27:             Checkpoint memory last = self[pos - 1];

34:                 self[pos - 1]._value = value;

45:         return pos == 0 ? address(0) : self._checkpoints[pos - 1]._value;

55:             uint256 mid = len - Math.sqrt(len);

59:                 low = mid + 1;

65:         return pos == 0 ? address(0) : self._checkpoints[pos - 1]._value;

79:                 low = mid + 1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/Elo.sol

4: import {FixedPointMathLib as fp} from "./FixedPointMathLib.sol";

26:         uint256 _kFactor; // scaled up `kFactor` by 100

28:         uint256 ratingDiff; // absolute value difference between `ratingA` and `ratingB`

33:             _kFactor = kFactor * 10_000;

34:             ratingDiff = _negative ? ratingA - ratingB : ratingB - ratingA;

48:         uint256 n; // numerator of the power, with scaling, (numerator of `ratingDiff / 400`)

49:         uint256 _powered; // the value of 10 ^ numerator

50:         uint256 powered; // the value of 16th root of 10 ^ numerator (fully resolved 10 ^ (ratingDiff / 400))

51:         uint256 kExpectedScore; // the expected score with K factor distributed

52:         uint256 kScore; // the actual score with K factor distributed

56:             n = _negative ? 800 - ratingDiff : 800 + ratingDiff;

59:             _powered = fp.rpow(10, n / 25, 1); // divide by 25 to avoid reach uint256 max

60:             powered = sixteenthRoot(_powered); // x ^ (1 / 16) is the same as 16th root of x

63:             kExpectedScore = _kFactor / (100 + powered); // both numerator and denominator scaled up by 100

64:             kScore = kFactor * score; // input score is already scaled up by 100

68:             change = negative ? kExpectedScore - kScore : kScore - kExpectedScore;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

12:     uint256 internal constant MAX_UINT256 = 2 ** 256 - 1;

14:     uint256 internal constant WAD = 1e18; // The scalar of ETH and most ERC20s.

17:         return mulDivDown(x, y, WAD); // Equivalent to (x * y) / WAD rounded down.

21:         return mulDivUp(x, y, WAD); // Equivalent to (x * y) / WAD rounded up.

25:         return mulDivDown(x, WAD, y); // Equivalent to (x * WAD) / y rounded down.

29:         return mulDivUp(x, WAD, y); // Equivalent to (x * WAD) / y rounded up.

155:             let y := x // We start y at x, which will help us make our initial estimate.

157:             z := 181 // The "correct" value is 1, but this saves a multiplication later.

197:             z := shr(18, mul(z, add(y, 65536))) // A mul() is saved from starting z at 181.

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

5: import {Math} from "@openzeppelin/contracts/utils/math/Math.sol";

18:         uint16 parentShares; // Optional rewards for contribution's parent

19:         uint256 stakeThreshold; // Each VIRTUAL will require minimum amount of staked tokens to be considered for rewards

35:             Checkpoint memory last = self[pos - 1];

42:                 self[pos - 1]._value = value;

53:         return pos == 0 ? RewardSettings(0, 0, 0, 0, 0) : self._checkpoints[pos - 1]._value;

63:             uint256 mid = len - Math.sqrt(len);

67:                 low = mid + 1;

73:         return pos == 0 ? RewardSettings(0, 0, 0, 0, 0) : self._checkpoints[pos - 1]._value;

87:                 low = mid + 1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

5: import {Math} from "@openzeppelin/contracts/utils/math/Math.sol";

32:             Checkpoint memory last = self[pos - 1];

39:                 self[pos - 1]._value = value;

50:         return pos == 0 ? RewardSettings(0, 0) : self._checkpoints[pos - 1]._value;

60:             uint256 mid = len - Math.sqrt(len);

64:                 low = mid + 1;

70:         return pos == 0 ? RewardSettings(0, 0) : self._checkpoints[pos - 1]._value;

84:                 low = mid + 1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/libs/TokenSaver.sol

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

6: import "@openzeppelin/contracts/access/AccessControl.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

4: import "./IRouter.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/pool/IUniswapV2Router02.sol

3: import "./IUniswapV2Router01.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router02.sol)

```solidity
File: contracts/tax/AgentTax.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/utils/math/Math.sol";

6: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: import "@openzeppelin/contracts/utils/math/Math.sol";

9: import "../pool/IRouter.sol";

10: import "../virtualPersona/IAgentNft.sol";

11: import "./ITBABonus.sol";

50:     mapping(uint256 agentId => address tba) private _agentTba; // cache to prevent calling AgentNft frequently

121:         require((feeRate_ + creatorFeeRate_) == DENOM, "Invalid fee rates");

177:         for (uint i = 0; i < txhashes.length; i++) {

183:             totalAmount += amounts[i];

186:         agentAmounts.amountCollected += totalAmount;

202:         uint256 amountToSwap = agentAmounts.amountCollected - agentAmounts.amountSwapped;

227:             router.swapExactTokensForTokens(amountToSwap, minOutput, path, address(this), block.timestamp + 300)

232:             uint256 feeAmount = (assetReceived * feeRate) / DENOM;

233:             uint256 creatorFee = assetReceived - feeAmount;

246:             agentAmounts.amountSwapped += amountToSwap;

272:         for (uint i = 0; i < agentIds.length; i++) {

276:             uint256 amountToSwap = agentAmounts.amountCollected - agentAmounts.amountSwapped;

282:             uint256 minOutput = ((amountToSwap * (DENOM - slippage)) / DENOM);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/utils/math/Math.sol";

6: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: import "@openzeppelin/contracts/utils/math/Math.sol";

9: import "./IBondingTax.sol";

10: import "../pool/IRouter.sol";

77:         _slippage = 100; // default to 1%

143:         uint256 minOutput = (expectedOutput * (10000 - _slippage)) / 10000;

145:         try router.swapExactTokensForTokens(amount, minOutput, path, treasury, block.timestamp + 300) returns (

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

4: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

6: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

47:         for (uint i = 0; i < txhashes.length; i++) {

55:             total += amount;

63:         refunds[txhash] += amount;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: import "./ITBABonus.sol";

53:         for (uint256 i = 0; i < agentIds.length; i++) {

72:         uint256 allowance = _agentAllowances[agentId] - _agentPaidAmounts[agentId];

73:         uint256 bonus = (amount * bonusRate) / DENOM;

80:             _agentPaidAmounts[agentId] += bonus;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/token/Airdrop.sol

3: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

39:                     or(eq(mload(0x00), 1), iszero(returndatasize())), // Returned 1 or nothing.

75:                         or(eq(mload(0x00), 1), iszero(returndatasize())), // Returned 1 or nothing.

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Airdrop.sol)

```solidity
File: contracts/token/Virtual.sol

5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

6: import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Capped.sol";

7: import "@openzeppelin/contracts/access/Ownable.sol";

13:     ) ERC20("Virtual Protocol", "VIRTUAL") ERC20Capped(1000000000 * 10 ** 18) Ownable(initialOwner) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

4: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorSettingsUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorStorageUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesQuorumFractionUpgradeable.sol";

7: import "@openzeppelin/contracts/utils/structs/Checkpoints.sol";

8: import "./IAgentDAO.sol";

9: import "./GovernorCountingSimpleUpgradeable.sol";

10: import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

11: import "@openzeppelin/contracts/utils/Strings.sol";

12: import "../contribution/IContributionNft.sol";

13: import "./IEloCalculator.sol";

14: import "../contribution/IServiceNft.sol";

15: import "./IAgentNft.sol";

88:         uint256 proposerVotes = getVotes(proposer, clock() - 1);

136:             ++_totalScore;

137:             _scores[account].push(SafeCast.toUint48(block.number), SafeCast.toUint208(scoreOf(account)) + 1);

174:         _proposalMaturities[proposalId] += (maturity * weight);

197:         return Math.min(10000, _proposalMaturities[proposalId] / forVotes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

4: import "@openzeppelin/contracts/proxy/Clones.sol";

5: import "@openzeppelin/contracts/governance/IGovernor.sol";

6: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

7: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

8: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

9: import "@openzeppelin/contracts/access/AccessControl.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

12: import "./IAgentFactory.sol";

13: import "./IAgentToken.sol";

14: import "./IAgentVeToken.sol";

15: import "./IAgentDAO.sol";

16: import "./IAgentNft.sol";

17: import "../libs/IERC6551Registry.sol";

26:     address public tbaRegistry; // Token bound account

32:     address public assetToken; // Base currency

33:     uint256 public maturityDuration; // Staking duration in seconds for initial LP. eg: 10years

35:     bytes32 public constant WITHDRAW_ROLE = keccak256("WITHDRAW_ROLE"); // Able to withdraw and execute applications

64:     address public gov; // Deprecated in v2, execution of application does not require DAO decision anymore

75:     address private _vault; // Vault to hold all Virtual NFTs

92:     address private _minter; // Unused

94:     address public defaultDelegatee; // Unused

99:     uint16 private _tokenMultiplier; // Unused

156:         uint256 id = _nextId++;

157:         uint256 proposalEndBlock = block.number; // No longer required in v2

314:             block.timestamp + maturityDuration,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

4: import "@openzeppelin/contracts/proxy/Clones.sol";

5: import "@openzeppelin/contracts/governance/IGovernor.sol";

6: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

7: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

8: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

9: import "@openzeppelin/contracts/access/AccessControl.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

12: import "./IAgentFactoryV3.sol";

13: import "./IAgentToken.sol";

14: import "./IAgentVeToken.sol";

15: import "./IAgentDAO.sol";

16: import "./IAgentNft.sol";

17: import "../libs/IERC6551Registry.sol";

26:     address public tbaRegistry; // Token bound account

32:     address public assetToken; // Base currency

33:     uint256 public maturityDuration; // Staking duration in seconds for initial LP. eg: 10years

35:     bytes32 public constant WITHDRAW_ROLE = keccak256("WITHDRAW_ROLE"); // Able to withdraw and execute applications

64:     address public gov; // Deprecated in v2, execution of application does not require DAO decision anymore

75:     address private _vault; // Vault to hold all Virtual NFTs

92:     address private _minter; // Unused

99:     uint16 private _tokenMultiplier; // Unused

159:         uint256 id = _nextId++;

160:         uint256 proposalEndBlock = block.number; // No longer required in v2

328:             block.timestamp + maturityDuration,

443:         uint256 id = _nextId++;

444:         uint256 proposalEndBlock = block.number; // No longer required in v2

475:             totalSupply - lpSupply,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

4: import "@openzeppelin/contracts/proxy/Clones.sol";

5: import "@openzeppelin/contracts/governance/IGovernor.sol";

6: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

7: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

8: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

9: import "@openzeppelin/contracts/access/AccessControl.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

12: import "./IAgentFactoryV4.sol";

13: import "./IAgentToken.sol";

14: import "./IAgentVeToken.sol";

15: import "./IAgentDAO.sol";

16: import "./IAgentNft.sol";

17: import "../libs/IERC6551Registry.sol";

18: import "../pool/IUniswapV2Factory.sol";

19: import "../pool/IUniswapV2Router02.sol";

28:     address public tbaRegistry; // Token bound account

34:     address public assetToken; // Base currency

35:     uint256 public maturityDuration; // Staking duration in seconds for initial LP. eg: 10years

37:     bytes32 public constant WITHDRAW_ROLE = keccak256("WITHDRAW_ROLE"); // Able to withdraw and execute applications

70:     address private _vault; // Vault to hold all Virtual NFTs

157:         uint256 id = _nextId++;

158:         uint256 proposalEndBlock = block.number; // No longer required in v2

352:             block.timestamp + maturityDuration,

480:         uint256 id = _nextId++;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

4: import "@openzeppelin/contracts/proxy/Clones.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "@openzeppelin/contracts/governance/IGovernor.sol";

7: import "@openzeppelin/contracts/access/Ownable.sol";

8: import "@openzeppelin/contracts/utils/Pausable.sol";

10: import "./AgentNftV2.sol";

11: import "./IAgentVeToken.sol";

12: import "./IAgentDAO.sol";

13: import "./IAgentToken.sol";

163:             block.timestamp + maturityDuration,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

4: import "@openzeppelin/contracts/governance/extensions/GovernorVotes.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import {IERC5805} from "@openzeppelin/contracts/interfaces/IERC5805.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol";

9: import "@openzeppelin/contracts/token/ERC721/extensions/IERC721Enumerable.sol";

10: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

11: import "@openzeppelin/contracts/interfaces/IERC5805.sol";

12: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

13: import "./IAgentNft.sol";

14: import "./CoreRegistry.sol";

15: import "./ValidatorRegistry.sol";

16: import "./IAgentDAO.sol";

31:     bytes32 public constant VALIDATOR_ADMIN_ROLE = keccak256("VALIDATOR_ADMIN_ROLE"); // Validator admin can manage validators for all personas

96:         _nextVirtualId++;

206:         for (uint256 i = 0; i < total; i++) {

227:         return _nextVirtualId - 1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/access/Ownable2StepUpgradeable.sol";

7: import "@openzeppelin/contracts/utils/structs/EnumerableSet.sol";

8: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

9: import "../pool/IUniswapV2Router02.sol";

10: import "../pool/IUniswapV2Factory.sol";

11: import "./IAgentToken.sol";

12: import "./IAgentFactory.sol";

33:     address public pairToken; // The token used to trade for this token

43:     address public vault; // Project supply vault

61:     IAgentFactory private _factory; // Single source of truth

95:         uint256 lpSupply = supplyParams.lpSupply * (10 ** decimals());

96:         uint256 vaultSupply = supplyParams.vaultSupply * (10 ** decimals());

109:         _autoSwapInProgress = true; // We don't want to tax initial liquidity

135:             erc20SupplyParameters_.maxSupply != (erc20SupplyParameters_.vaultSupply + erc20SupplyParameters_.lpSupply)

564:         _approve(owner, spender, allowance(owner, spender) + addedValue);

589:             _approve(owner, spender, currentAllowance - subtractedValue);

621:         _balances[from] = fromBalance - amount;

622:         _balances[to] += amountMinusTax;

698:                         uint256 projectTax = ((sentAmount_ * projectSellTaxBasisPoints) / BP_DENOM);

699:                         projectTaxPendingSwap += uint128(projectTax);

700:                         tax += projectTax;

706:                         uint256 projectTax = ((sentAmount_ * projectBuyTaxBasisPoints) / BP_DENOM);

707:                         projectTaxPendingSwap += uint128(projectTax);

708:                         tax += projectTax;

713:                     _balances[address(this)] += tax;

715:                     amountLessTax_ -= tax;

736:             uint256 swapThresholdInTokens = (_totalSupply * swapThresholdBasisPoints) / BP_DENOM;

742:                 if (swapBalance > swapThresholdInTokens * MAX_SWAP_THRESHOLD_MULTIPLE) {

743:                     swapBalance = swapThresholdInTokens * MAX_SWAP_THRESHOLD_MULTIPLE;

796:                 block.timestamp + 600

815:                 projectTaxPendingSwap -= uint128((projectTaxPendingSwap * swapBalance_) / contractBalance_);

914:         _totalSupply += uint128(amount);

917:             _balances[account] += amount;

948:             _balances[account] = accountBalance - amount;

950:             _totalSupply -= uint128(amount);

1000:                 _approve(owner, spender, currentAllowance - amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";

5: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

6: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

7: import {Checkpoints} from "@openzeppelin/contracts/utils/structs/Checkpoints.sol";

8: import "./IAgentVeToken.sol";

9: import "./IAgentNft.sol";

10: import "./ERC20Votes.sol";

11: import "@openzeppelin/contracts/access/IAccessControl.sol";

18:     address public assetToken; // This is the token that is staked

20:     uint256 public matureAt; // The timestamp when the founder can withdraw the tokens

21:     bool public canStake; // To control private/public agent mode

22:     uint256 public initialLock; // Initial locked amount

60:         require(canStake || totalSupply() == 0, "Staking is disabled for private agent"); // Either public or first staker

99:         if ((sender == founder) && ((balanceOf(sender) - amount) < initialLock)) {

118:     function transfer(address /*to*/, uint256 /*value*/) public override returns (bool) {

122:     function transferFrom(address /*from*/, address /*to*/, uint256 /*value*/) public override returns (bool) {

126:     function approve(address /*spender*/, uint256 /*value*/) public override returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

13:         coreTypes[_nextCoreType++] = "Cognitive Core";

14:         coreTypes[_nextCoreType++] = "Voice and Sound Core";

15:         coreTypes[_nextCoreType++] = "Visual Core";

16:         coreTypes[_nextCoreType++] = "Domain Expertise Core";

20:         uint8 coreType = _nextCoreType++;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/ERC20Votes.sol

5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol";

6: import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";

7: import "../libs/AddressCheckpoints.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ERC20Votes.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

4: import "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

6: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

7: import {Elo} from "../libs/Elo.sol";

8: import "./IEloCalculator.sol";

35:         return (numerator + denominator - 1) / denominator;

42:         for (uint256 i = 0; i < battles.length; i++) {

47:                 eloA -= change;

48:                 eloB += change;

50:                 eloA += change;

51:                 eloB -= change;

54:         return currentRating + eloA - 1000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

6: import "@openzeppelin/contracts-upgradeable/governance/GovernorUpgradeable.sol";

64:         return quorum(proposalSnapshot(proposalId)) <= proposalVote.forVotes + proposalVote.abstainVotes;

84:         bytes memory // params

100:             proposalVote.againstVotes += weight;

102:             proposalVote.forVotes += weight;

104:             proposalVote.abstainVotes += weight;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

```solidity
File: contracts/virtualPersona/IAgentDAO.sol

4: import "@openzeppelin/contracts/governance/IGovernor.sol";

5: import "@openzeppelin/contracts/governance/utils/IVotes.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentDAO.sol)

```solidity
File: contracts/virtualPersona/IAgentFactory.sol

4: import "@openzeppelin/contracts/governance/IGovernor.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentFactory.sol)

```solidity
File: contracts/virtualPersona/IAgentFactoryV3.sol

4: import "@openzeppelin/contracts/governance/IGovernor.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/IAgentFactoryV4.sol

4: import "@openzeppelin/contracts/governance/IGovernor.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/IAgentNft.sol

4: import "./IValidatorRegistry.sol";

8:         address dao; // Agent DAO can update the agent metadata

11:         address tba; // Token Bound Address

18:         address pool; // Liquidity pool for the agent

19:         address veToken; // Voting escrow token

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentNft.sol)

```solidity
File: contracts/virtualPersona/IAgentToken.sol

4: import "@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "./IERC20Config.sol";

7: import "./IErrors.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentToken.sol)

```solidity
File: contracts/virtualPersona/IErrors.sol

6:         OK, //                                                  No error

7:         INVALID_NUMITEMS, //                                    The numItem value is 0

8:         SPOT_PRICE_OVERFLOW //                                  The updated spot price doesn't fit into 128 bits

11:     error AdapterParamsMustBeEmpty(); //                      The adapter parameters on this LZ call must be empty.

13:     error AdditionToPoolIsBelowPerTransactionMinimum(); //    The contribution amount is less than the minimum.

15:     error AdditionToPoolWouldExceedPoolCap(); //              This addition to the pool would exceed the pool cap.

17:     error AdditionToPoolWouldExceedPerAddressCap(); //        This addition to the pool would exceed the per address cap.

19:     error AddressAlreadySet(); //                             The address being set can only be set once, and is already non-0.

21:     error AllowanceDecreasedBelowZero(); //                   You cannot decrease the allowance below zero.

23:     error AlreadyInitialised(); //                            The contract is already initialised: it cannot be initialised twice!

25:     error ApprovalCallerNotOwnerNorApproved(); //             The caller must own the token or be an approved operator.

27:     error ApproveFromTheZeroAddress(); //                     Approval cannot be called from the zero address (indeed, how have you??).

29:     error ApproveToTheZeroAddress(); //                       Approval cannot be given to the zero address.

31:     error ApprovalQueryForNonexistentToken(); //              The token does not exist.

33:     error AuctionStatusIsNotEnded(); //                       Throw if the action required the auction to be closed, and it isn't.

35:     error AuctionStatusIsNotOpen(); //                        Throw if the action requires the auction to be open, and it isn't.

37:     error AuxCallFailed(address[] modules, uint256 value, bytes data, uint256 txGas); //                                                     An auxilliary call from the drop factory failed.

39:     error BalanceMismatch(); //                               An error when comparing balance amounts.

41:     error BalanceQueryForZeroAddress(); //                    Cannot query the balance for the zero address.

43:     error BidMustBeBelowTheFloorWhenReducingQuantity(); //    Only bids that are below the floor can reduce the quantity of the bid.

45:     error BidMustBeBelowTheFloorForRefundDuringAuction(); //  Only bids that are below the floor can be refunded during the auction.

47:     error BondingCurveError(BondingCurveErrorType error); //  An error of the type specified has occured in bonding curve processing.

49:     error BurnExceedsBalance(); //                            The amount you have selected to burn exceeds the addresses balance.

51:     error BurnFromTheZeroAddress(); //                        Tokens cannot be burned from the zero address. (Also, how have you called this!?!)

53:     error CallerIsNotDepositBoxOwner(); //                    The caller is not the owner of the deposit box.

55:     error CallerIsNotFactory(); //                            The caller of this function must match the factory address in storage.

57:     error CallerIsNotFactoryOrProjectOwner(); //              The caller of this function must match the factory address OR project owner address.

59:     error CallerIsNotFactoryProjectOwnerOrPool(); //          The caller of this function must match the factory address, project owner or pool address.

61:     error CallerIsNotTheOwner(); //                           The caller is not the owner of this contract.

63:     error CallerIsNotTheManager(); //                         The caller is not the manager of this contract.

65:     error CallerMustBeLzApp(); //                             The caller must be an LZ application.

67:     error CallerIsNotPlatformAdmin(address caller); //        The caller of this function must be part of the platformAdmin group.

69:     error CallerIsNotSuperAdmin(address caller); //           The caller of this function must match the superAdmin address in storage.

71:     error CannotAddLiquidityOnCreateAndUseDRIPool(); //       Cannot use both liquidity added on create and a DRIPool in the same token.

73:     error CannotSetNewOwnerToTheZeroAddress(); //             You can't set the owner of this contract to the zero address (address(0)).

75:     error CannotSetToZeroAddress(); //                        The corresponding address cannot be set to the zero address (address(0)).

77:     error CannotSetNewManagerToTheZeroAddress(); //           Cannot transfer the manager to the zero address (address(0)).

79:     error CannotWithdrawThisToken(); //                       Cannot withdraw the specified token.

81:     error CanOnlyReduce(); //                                 The given operation can only reduce the value specified.

83:     error CollectionAlreadyRevealed(); //                     The collection is already revealed; you cannot call reveal again.

85:     error ContractIsDecommissioned(); //                      This contract is decommissioned!

87:     error ContractIsPaused(); //                              The call requires the contract to be unpaused, and it is paused.

89:     error ContractIsNotPaused(); //                           The call required the contract to be paused, and it is NOT paused.

91:     error DecreasedAllowanceBelowZero(); //                   The request would decrease the allowance below zero, and that is not allowed.

93:     error DestinationIsNotTrustedSource(); //                 The destination that is being called through LZ has not been set as trusted.

95:     error DeployerOnly(); //                                  This method can only be called by the deployer address.

97:     error DeploymentError(); //                               Error on deployment.

99:     error DepositBoxIsNotOpen(); //                           This action cannot complete as the deposit box is not open.

101:     error DriPoolAddressCannotBeAddressZero(); //             The Dri Pool address cannot be the zero address.

103:     error GasLimitIsTooLow(); //                              The gas limit for the LayerZero call is too low.

105:     error IncorrectConfirmationValue(); //                    You need to enter the right confirmation value to call this funtion (usually 69420).

107:     error IncorrectPayment(); //                              The function call did not include passing the correct payment.

109:     error InitialLiquidityAlreadyAdded(); //                  Initial liquidity has already been added. You can't do it again.

111:     error InitialLiquidityNotYetAdded(); //                   Initial liquidity needs to have been added for this to succedd.

113:     error InsufficientAllowance(); //                         There is not a high enough allowance for this operation.

115:     error InvalidAdapterParams(); //                          The current adapter params for LayerZero on this contract won't work :(.

117:     error InvalidAddress(); //                                An address being processed in the function is not valid.

119:     error InvalidEndpointCaller(); //                         The calling address is not a valid LZ endpoint. The LZ endpoint was set at contract creation

122:     error InvalidMinGas(); //                                 The minimum gas setting for LZ in invalid.

124:     error InvalidOracleSignature(); //                        The signature provided with the contract call is not valid, either in format or signer.

126:     error InvalidPayload(); //                                The LZ payload is invalid

128:     error InvalidReceiver(); //                               The address used as a target for funds is not valid.

130:     error InvalidSourceSendingContract(); //                  The LZ message is being related from a source contract on another chain that is NOT trusted.

132:     error InvalidTotalShares(); //                            Total shares must equal 100 percent in basis points.

134:     error LimitsCanOnlyBeRaised(); //                          Limits are UP ONLY.

136:     error ListLengthMismatch(); //                            Two or more lists were compared and they did not match length.

138:     error LiquidityPoolMustBeAContractAddress(); //           Cannot add a non-contract as a liquidity pool.

140:     error LiquidityPoolCannotBeAddressZero(); //              Cannot add a liquidity pool from the zero address.

142:     error LPLockUpMustFitUint88(); //                         LP lockup is held in a uint88, so must fit.

144:     error NoTrustedPathRecord(); //                           LZ needs a trusted path record for this to work. What's that, you ask?

146:     error MachineAddressCannotBeAddressZero(); //             Cannot set the machine address to the zero address.

148:     error ManagerUnauthorizedAccount(); //                    The caller is not the pending manager.

150:     error MaxBidQuantityIs255(); //                           Validation: as we use a uint8 array to track bid positions the max bid quantity is 255.

152:     error MaxPublicMintAllowanceExceeded(uint256 requested, uint256 alreadyMinted, uint256 maxAllowance); //                                                     The calling address has requested a quantity that would exceed the max allowance.

154:     error MaxSupplyTooHigh(); //                              Max supply must fit in a uint128.

156:     error MaxTokensPerWalletExceeded(); //                    The transfer would exceed the max tokens per wallet limit.

158:     error MaxTokensPerTxnExceeded(); //                       The transfer would exceed the max tokens per transaction limit.

160:     error MetadataIsLocked(); //                              The metadata on this contract is locked; it cannot be altered!

162:     error MinGasLimitNotSet(); //                             The minimum gas limit for LayerZero has not been set.

164:     error MintERC2309QuantityExceedsLimit(); //               The `quantity` minted with ERC2309 exceeds the safety limit.

166:     error MintingIsClosedForever(); //                        Minting is, as the error suggests, so over (and locked forever).

168:     error MintToZeroAddress(); //                             Cannot mint to the zero address.

170:     error MintZeroQuantity(); //                              The quantity of tokens minted must be more than zero.

172:     error NewBuyTaxBasisPointsExceedsMaximum(); //            Project owner trying to set the tax rate too high.

174:     error NewSellTaxBasisPointsExceedsMaximum(); //           Project owner trying to set the tax rate too high.

176:     error NoETHForLiquidityPair(); //                         No ETH has been provided for the liquidity pair.

178:     error TaxPeriodStillInForce(); //                         The minimum tax period has not yet expired.

180:     error NoPaymentDue(); //                                  No payment is due for this address.

182:     error NoRefundForCaller(); //                             Error thrown when the calling address has no refund owed.

184:     error NoStoredMessage(); //                               There is no stored message matching the passed parameters.

186:     error NothingToClaim(); //                                The calling address has nothing to claim.

188:     error NoTokenForLiquidityPair(); //                       There is no token to add to the LP.

190:     error OperationDidNotSucceed(); //                        The operation failed (vague much?).

192:     error OracleSignatureHasExpired(); //                     A signature has been provided but it is too old.

194:     error OwnershipNotInitializedForExtraData(); //           The `extraData` cannot be set on an uninitialized ownership slot.

196:     error OwnerQueryForNonexistentToken(); //                 The token does not exist.

198:     error CallerIsNotAdminNorFactory(); //                  The caller of this function must match the factory address or be an admin.

200:     error ParametersDoNotMatchSignedMessage(); //             The parameters passed with the signed message do not match the message itself.

202:     error ParamTooLargeStartDate(); //                        The passed parameter exceeds the var type max.

204:     error ParamTooLargeEndDate(); //                          The passed parameter exceeds the var type max.

206:     error ParamTooLargeMinETH(); //                           The passed parameter exceeds the var type max.

208:     error ParamTooLargePerAddressMax(); //                    The passed parameter exceeds the var type max.

210:     error ParamTooLargeVestingDays(); //                      The passed parameter exceeds the var type max.

212:     error ParamTooLargePoolSupply(); //                       The passed parameter exceeds the var type max.

214:     error ParamTooLargePoolPerTxnMinETH(); //                 The passed parameter exceeds the var type max.

216:     error PassedConfigDoesNotMatchApproved(); //              The config provided on the call does not match the approved config.

218:     error PauseCutOffHasPassed(); //                          The time period in which we can pause has passed; this contract can no longer be paused.

220:     error PaymentMustCoverPerMintFee(); //                    The payment passed must at least cover the per mint fee for the quantity requested.

222:     error PermitDidNotSucceed(); //                           The safeERC20 permit failed.

224:     error PlatformAdminCannotBeAddressZero(); //              We cannot use the zero address (address(0)) as a platformAdmin.

226:     error PlatformTreasuryCannotBeAddressZero(); //           The treasury address cannot be set to the zero address.

228:     error PoolIsAboveMinimum(); //                            You required the pool to be below the minimum, and it is not

230:     error PoolIsBelowMinimum(); //                            You required the pool to be above the minimum, and it is not

232:     error PoolPhaseIsClosed(); //                             The block.timestamp is either before the pool is open or after it is closed.

234:     error PoolPhaseIsNotAfter(); //                           The block.timestamp is either before or during the pool open phase.

236:     error PoolVestingNotYetComplete(); //                     Tokens in the pool are not yet vested.

238:     error ProjectOwnerCannotBeAddressZero(); //               The project owner has to be a non zero address.

240:     error ProofInvalid(); //                                  The provided proof is not valid with the provided arguments.

242:     error QuantityExceedsRemainingCollectionSupply(); //      The requested quantity would breach the collection supply.

244:     error QuantityExceedsRemainingPhaseSupply(); //           The requested quantity would breach the phase supply.

246:     error QuantityExceedsMaxPossibleCollectionSupply(); //    The requested quantity would breach the maximum trackable supply

248:     error ReferralIdAlreadyUsed(); //                         This referral ID has already been used; they are one use only.

250:     error RequestingMoreThanAvailableBalance(); //             The request exceeds the available balance.

256:     ); //                                                     Number of tokens requested for this mint exceeds the remaining allocation (taking the

259:     error RoyaltyFeeWillExceedSalePrice(); //                 The ERC2981 royalty specified will exceed the sale price.

261:     error ShareTotalCannotBeZero(); //                        The total of all the shares cannot be nothing.

263:     error SliceOutOfBounds(); //                              The bytes slice operation was out of bounds.

265:     error SliceOverflow(); //                                 The bytes slice operation overlowed.

267:     error SuperAdminCannotBeAddressZero(); //                 The superAdmin cannot be the sero address (address(0)).

269:     error SupplyTotalMismatch(); //                           The sum of the team supply and lp supply does not match.

271:     error SupportWindowIsNotOpen(); //                        The project owner has not requested support within the support request expiry window.

273:     error TaxFreeAddressCannotBeAddressZero(); //             A tax free address cannot be address(0)

275:     error TemplateCannotBeAddressZero(); //                   The address for a template cannot be address zero (address(0)).

277:     error TemplateNotFound(); //                              There is no template that matches the passed template Id.

279:     error ThisMintIsClosed(); //                              It's over (well, this mint is, anyway).

281:     error TotalSharesMustMatchDenominator(); //               The total of all shares must equal the denominator value.

283:     error TransferAmountExceedsBalance(); //                  The transfer amount exceeds the accounts available balance.

285:     error TransferCallerNotOwnerNorApproved(); //             The caller must own the token or be an approved operator.

287:     error TransferFailed(); //                                The transfer has failed.

289:     error TransferFromIncorrectOwner(); //                    The token must be owned by `from`.

291:     error TransferToNonERC721ReceiverImplementer(); //        Cannot safely transfer to a contract that does not implement the ERC721Receiver interface.

293:     error TransferFromZeroAddress(); //                       Cannot transfer from the zero address. Indeed, this surely is impossible, and likely a waste to check??

295:     error TransferToZeroAddress(); //                         Cannot transfer to the zero address.

297:     error UnrecognisedVRFMode(); //                           Currently supported VRF modes are 0: chainlink and 1: arrng

299:     error URIQueryForNonexistentToken(); //                   The token does not exist.

301:     error ValueExceedsMaximum(); //                           The value sent exceeds the maximum allowed (super useful explanation huh?).

303:     error VRFCoordinatorCannotBeAddressZero(); //             The VRF coordinator cannot be the zero address (address(0)).

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IErrors.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "./IValidatorRegistry.sol";

41:         return _baseValidatorScore[validator][virtualId] + _getScoreOf(virtualId, validator);

49:         return _baseValidatorScore[validator][virtualId] + _getPastScore(virtualId, validator, timepoint);

62:         for (uint256 i = 0; i < validatorCount(virtualId); i++) {

63:             totalScore += validatorScore(virtualId, validatorAt(virtualId, i));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="GAS-11"></a>[GAS-11] Use Custom Errors instead of Revert Strings to save Gas

Custom errors are available from solidity version 0.8.4. Custom errors save [**~50 gas**](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

Additionally, custom errors can be used inside and outside of contracts (including interfaces and libraries).

Source: <https://blog.soliditylang.org/2021/04/21/custom-errors/>:

> Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4), there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Consider replacing **all revert strings** with custom errors in the solution, and particularly those that have multiple occurrences:

*Instances (159)*:

```solidity
File: contracts/AgentInference.sol

44:         require(agentIds.length == amounts.length && agentIds.length == coreIds.length, "Invalid input");

50:         require(token.balanceOf(sender) >= total, "Insufficient balance");

72:         require(len == amounts.length && len == coreIds.length && len == promptHashes.length, "Invalid input");

78:         require(token.balanceOf(sender) >= total, "Insufficient balance");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

80:         require(parentId != proposalId, "Cannot be parent of itself");

104:         require(_msgSender() == _admin, "Only admin can set admin");

166:         require(_msgSender() == _admin, "Only admin can set elo calculator");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

60:         require(_msgSender() == info.dao, "Caller is not VIRTUAL DAO");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

161:         require(newRate > 0, "Rate err");

171:         require(_checkIfProfileExists(account), "User Profile dose not exist.");

200:         require(purchaseAmount > fee, "Purchase amount must be greater than fee");

202:         require(IERC20(assetToken).balanceOf(msg.sender) >= purchaseAmount, "Insufficient amount");

282:         require(tokenInfo[tokenAddress].trading, "Token not trading");

319:         require(tokenInfo[tokenAddress].trading, "Token not trading");

362:         require(_token.trading && !_token.tradingOnUniswap, "trading is already open");

407:         require(info.tradingOnUniswap, "Token is not graduated yet");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

91:         require(owner != address(0), "ERC20: approve from the zero address");

92:         require(spender != address(0), "ERC20: approve to the zero address");

100:         require(from != address(0), "ERC20: transfer from the zero address");

101:         require(to != address(0), "ERC20: transfer to the zero address");

102:         require(amount > 0, "Transfer amount must be greater than zero");

105:             require(amount <= _maxTxAmount, "Exceeds MaxTx");

126:         require(user != address(0), "ERC20: Exclude Max Tx from the zero address");

132:         require(user != address(0), "Invalid address");

137:         require(user != address(0), "Invalid address");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

42:         require(tokenA != address(0), "Zero addresses are not allowed.");

43:         require(tokenB != address(0), "Zero addresses are not allowed.");

44:         require(router != address(0), "No router");

75:         require(newVault_ != address(0), "Zero addresses are not allowed.");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

27:         require(router_ != address(0), "Zero addresses are not allowed.");

28:         require(token0 != address(0), "Zero addresses are not allowed.");

29:         require(token1 != address(0), "Zero addresses are not allowed.");

37:         require(router == msg.sender, "Only router can call this function");

46:         require(_pool.lastUpdated == 0, "Already minted");

72:         require(_user != address(0), "Zero addresses are not allowed.");

73:         require(_token != address(0), "Zero addresses are not allowed.");

83:         require(recipient != address(0), "Zero addresses are not allowed.");

89:         require(recipient != address(0), "Zero addresses are not allowed.");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

34:         require(factory_ != address(0), "Zero addresses are not allowed.");

35:         require(assetToken_ != address(0), "Zero addresses are not allowed.");

46:         require(token != address(0), "Zero addresses are not allowed.");

80:         require(token_ != address(0), "Zero addresses are not allowed.");

100:         require(tokenAddress != address(0), "Zero addresses are not allowed.");

101:         require(to != address(0), "Zero addresses are not allowed.");

136:         require(tokenAddress != address(0), "Zero addresses are not allowed.");

137:         require(to != address(0), "Zero addresses are not allowed.");

138:         require(amountIn > 0, "amountIn must be greater than 0");

166:         require(tokenAddress != address(0), "Zero addresses are not allowed.");

178:         require(spender != address(0), "Zero addresses are not allowed.");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

66:         require(p.reserve > 0 && p.maxContribution > 0 && p.feeAmt > 0 && p.duration > 0, "Invalid amt");

112:         require(addr != address(0), "Not found");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

125:         require(isFailed || isCancelled || agentTokenAddress != address(0), "Genesis not finalized yet");

137:         require(params.genesisID > 0, "Invalid genesis ID");

138:         require(params.factory != address(0), "Invalid factory address");

140:         require(bytes(params.genesisName).length > 0, "Invalid genesis name");

141:         require(bytes(params.genesisTicker).length > 0, "Invalid genesis ticker");

142:         require(params.genesisCores.length > 0, "Invalid genesis cores");

143:         require(params.tbaImplementation != address(0), "Invalid TBA implementation address");

144:         require(params.agentFactoryAddress != address(0), "Invalid agent factory address");

145:         require(params.virtualTokenAddress != address(0), "Invalid virtual token address");

146:         require(params.reserveAmount > 0, "Reserve amount must be greater than 0");

147:         require(params.maxContributionVirtualAmount > 0, "Max contribution must be greater than 0");

148:         require(params.agentTokenTotalSupply > 0, "Agent token total supply must be greater than 0");

149:         require(params.agentTokenLpSupply > 0, "Agent token lp supply must be greater than 0");

178:         require(pointAmt > 0, "Point amount must be greater than 0");

179:         require(virtualsAmt > 0, "Virtuals must be greater than 0");

182:         require(virtualsAmt <= maxContributionVirtualAmount, "Exceeds maximum virtuals per contribution");

185:         require(IERC20(virtualTokenAddress).balanceOf(msg.sender) >= virtualsAmt, "Insufficient Virtual Token balance");

212:         require(refundUserCountForFailed == 0, "OnGenesisFailed already called");

268:             require(agentToken != address(0), "Agent token creation failed");

309:         require(amount > 0, "No tokens to claim");

328:             require(participantIndexes[i] < participants.length, "Index out of bounds");

362:         require(startIndex < participants.length, "Start index out of bounds");

390:             require(participantIndexes[i] < participants.length, "Index out of bounds");

453:         require(token != address(0), "Invalid token address");

454:         require(amount <= IERC20(token).balanceOf(address(this)), "Insufficient balance to withdraw");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

20:         require(froms.length == tos.length && tos.length == values.length, "Invalid input");

30:         revert("Approve not supported");

34:         revert("Transfer not supported");

38:         revert("Transfer not supported");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/libs/Elo.sol

39:         require(ratingDiff < 1126, "Rating difference too large");

41:         if (_negative) require(ratingDiff < 800, "Rating difference too large");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/TokenSaver.sol

16:         require(hasRole(TOKEN_SAVER_ROLE, _msgSender()), "TokenSaver.onlyTokenSaver: permission denied");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/tax/AgentTax.sol

95:         require(assetToken_ != taxToken_, "Asset token cannot be same as tax token");

121:         require((feeRate_ + creatorFeeRate_) == DENOM, "Invalid fee rates");

174:         require(txhashes.length == amounts.length, "Unmatched inputs");

206:         require(balance >= amountToSwap, "Insufficient balance");

209:         require(taxRecipient.tba != address(0), "Agent does not have TBA");

224:         require(amountsOut.length > 1, "Failed to fetch token price");

264:         require(sender == recipient.creator || hasRole(ADMIN_ROLE, sender), "Only creator can update");

270:         require(slippage <= DENOM, "Invalid slippage");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

50:         require(_msgSender() == address(bondingRouter), "Only bonding router");

125:         require(amount > 0, "Nothing to be swapped");

140:         require(amountsOut.length > 1, "Failed to fetch token price");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

45:         require(txhashes.length == amounts.length, "Unmatched inputs");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

51:         require(agentIds.length == allowances.length, "Invalid input");

57:             require(allowance >= _agentPaidAmounts[agentId], "Allowance cannot be less than paid amount");

65:         require(agentId > 0, "Invalid agent ID");

66:         require(recipient != address(0), "Invalid recipient");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

67:         require(msg.sender == gov, "Only DAO can execute proposal");

80:         require(!locked, "cannot reenter");

147:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

152:         require(cores.length > 0, "Cores must be provided");

182:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

184:         require(application.status == ApplicationStatus.Active, "Application is not active");

186:         require(block.number > application.proposalEndBlock, "Application is not matured yet");

205:         require(_applications[id].status == ApplicationStatus.Active, "Application is not active");

207:         require(_tokenAdmin != address(0), "Token admin not set");

211:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

67:         require(msg.sender == gov, "Only DAO can execute proposal");

80:         require(!locked, "cannot reenter");

150:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

155:         require(cores.length > 0, "Cores must be provided");

185:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

187:         require(application.status == ApplicationStatus.Active, "Application is not active");

189:         require(block.number > application.proposalEndBlock, "Application is not matured yet");

200:         require(_applications[id].status == ApplicationStatus.Active, "Application is not active");

202:         require(_tokenAdmin != address(0), "Token admin not set");

280:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

434:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold_, "Insufficient asset token");

439:         require(cores.length > 0, "Cores must be provided");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

75:         require(!locked, "cannot reenter");

148:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

153:         require(cores.length > 0, "Cores must be provided");

183:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

185:         require(application.status == ApplicationStatus.Active, "Application is not active");

187:         require(block.number > application.proposalEndBlock, "Application is not matured yet");

206:         require(_applications[id].status == ApplicationStatus.Active, "Application is not active");

208:         require(_tokenAdmin != address(0), "Token admin not set");

304:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

461:         require(_tokenApplication[tokenAddr] == 0, "Token already exists");

463:         require(isCompatibleToken(tokenAddr), "Unsupported token");

465:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

472:         require(cores.length > 0, "Cores must be provided");

474:         require(initialLP > 0, "InitialLP must be greater than 0");

516:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

518:         require(_applicationToken[id] != address(0), "Not custom token application");

546:         require(factory.getPair(tokenAddr, assetToken) == address(0), "pool already exists");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

36:         require(!locked, "cannot reenter");

101:         require(!migratedAgents[id], "Agent already migrated");

105:         require(founder == _msgSender(), "Not founder");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

34:         require(_msgSender() == virtualInfos[virtualId].dao, "Caller is not VIRTUAL DAO");

39:         require(_msgSender() == _serviceNft, "Caller is not Service NFT");

95:         require(virtualId == _nextVirtualId, "Invalid virtualId");

175:         require(info.tba == address(0), "TBA already set");

180:         require(_msgSender() == virtualInfos[virtualId].dao, "Caller is not VIRTUAL DAO");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

33:         require(!locked, "cannot reenter");

60:         require(canStake || totalSupply() == 0, "Staking is disabled for private agent"); // Either public or first staker

63:         require(amount > 0, "Cannot stake 0");

64:         require(IERC20(assetToken).balanceOf(sender) >= amount, "Insufficient asset token balance");

65:         require(IERC20(assetToken).allowance(sender, address(this)) >= amount, "Insufficient asset token allowance");

70:         require(!registry.isBlacklisted(virtualId), "Agent Blacklisted");

85:         require(_msgSender() == founder, "Not founder");

91:         require(IAccessControl(agentNft).hasRole(ADMIN_ROLE, _msgSender()), "Not admin");

97:         require(balanceOf(sender) >= amount, "Insufficient balance");

100:             require(block.timestamp >= matureAt, "Not mature yet");

119:         revert("Transfer not supported");

123:         revert("Transfer not supported");

127:         revert("Approve not supported");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="GAS-12"></a>[GAS-12] Avoid contract existence checks by using low level calls

Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence

*Instances (33)*:

```solidity
File: contracts/AgentInference.sol

50:         require(token.balanceOf(sender) >= total, "Insufficient balance");

78:         require(token.balanceOf(sender) >= total, "Insufficient balance");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/Bonding.sol

202:         require(IERC20(assetToken).balanceOf(msg.sender) >= purchaseAmount, "Insufficient amount");

276:         token.transfer(msg.sender, token.balanceOf(address(this)));

398:         router.approval(pairAddress, agentToken, address(this), IERC20(agentToken).balanceOf(pairAddress));

414:             uint256 balance = token.balanceOf(acc);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FPair.sol

111:         return IERC20(tokenA).balanceOf(address(this));

115:         return IERC20(tokenB).balanceOf(address(this));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/genesis/Genesis.sol

185:         require(IERC20(virtualTokenAddress).balanceOf(msg.sender) >= virtualsAmt, "Insufficient Virtual Token balance");

239:             IERC20(virtualTokenAddress).balanceOf(address(this)) >= requiredVirtualsBalance,

281:             IERC20(agentTokenAddress).balanceOf(address(this)) >= totalDistributionAmount,

454:         require(amount <= IERC20(token).balanceOf(address(this)), "Insufficient balance to withdraw");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/tax/AgentTax.sol

165:         IERC20(token).safeTransfer(treasury, IERC20(token).balanceOf(address(this)));

204:         uint256 balance = IERC20(taxToken).balanceOf(address(this));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

119:         IERC20(token).safeTransfer(treasury, IERC20(token).balanceOf(address(this)));

123:         uint256 amount = IERC20(taxToken).balanceOf(address(this));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

37:         IERC20(token).safeTransfer(_msgSender(), IERC20(token).balanceOf(address(this)));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

79:         if (bonus > 0 && assetToken.balanceOf(address(this)) >= bonus) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

147:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

270:         IAgentVeToken(veToken).stake(IERC20(lp).balanceOf(address(this)), application.proposer, application.proposer);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

150:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

263:         IAgentVeToken(veToken).stake(IERC20(lp).balanceOf(address(this)), application.proposer, defaultDelegatee);

434:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold_, "Insufficient asset token");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

148:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

198:             IERC20(customToken).safeTransfer(application.proposer, IERC20(customToken).balanceOf(address(this)));

233:                 IERC20(token).balanceOf(address(this)),

287:         IAgentVeToken(veToken).stake(IERC20(lp).balanceOf(address(this)), application.proposer, defaultDelegatee);

465:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

527:                     try IAgentToken(tokenAddr).balanceOf(address(this)) returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

131:         IAgentVeToken(veToken).stake(IERC20(lp).balanceOf(address(this)), founder, founder);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

204:         uint256 total = serviceNft.balanceOf(info.tba);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

250:             IERC20(pairToken).balanceOf(address(this)),

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

64:         require(IERC20(assetToken).balanceOf(sender) >= amount, "Insufficient asset token balance");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="GAS-13"></a>[GAS-13] Stack variable used as a cheaper cache for a state variable is only used once

If the variable is only accessed once, it's cheaper to use the state variable directly that one time, and save the **3 gas** the extra stack assignment would spend

*Instances (18)*:

```solidity
File: contracts/genesis/Genesis.sol

468:         uint256 oldStartTime = startTime;

469:         uint256 oldEndTime = endTime;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/tax/AgentTax.sol

123:         address oldAsset = assetToken;

124:         uint16 oldFee = feeRate;

125:         uint16 oldCreatorFee = creatorFeeRate;

148:         uint256 oldMin = minSwapThreshold;

149:         uint256 oldMax = maxSwapThreshold;

158:         address oldTreasury = treasury;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

87:         address oldBondingRouter = bondingRouter;

88:         address oldAsset = assetToken;

102:         uint256 oldMin = minSwapThreshold;

103:         uint256 oldMax = maxSwapThreshold;

112:         address oldTreasury = treasury;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/TBABonus.sol

45:         uint16 oldBonusRate = bonusRate;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

395:         uint256 oldswapThresholdBasisPoints = swapThresholdBasisPoints;

412:         uint16 oldBuyTaxBasisPoints = projectBuyTaxBasisPoints;

413:         uint16 oldSellTaxBasisPoints = projectSellTaxBasisPoints;

848:             uint256 projectDistribution = projectTaxPendingSwap;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="GAS-14"></a>[GAS-14] State variables only set in the constructor should be declared `immutable`

Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around **20 000 gas** per variable) and replace the expensive storage-reading operations (around **2100 gas** per reading) to a less expensive value reading (**3 gas**)

*Instances (11)*:

```solidity
File: contracts/fun/FERC20.sol

29:         _name = name_;

31:         _symbol = symbol_;

33:         _totalSupply = supply * 10 ** _decimals;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FPair.sol

31:         router = router_;

32:         tokenA = token0;

33:         tokenB = token1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

36:         router = router_;

37:         tokenIn = tokenIn_;

38:         tokenOut = tokenOut_;

39:         factory = factory_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

43:         _nft = AgentNftV2(agentNft_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

### <a name="GAS-15"></a>[GAS-15] Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (91)*:

```solidity
File: contracts/contribution/ServiceNft.sol

141:     function setDatasetImpactWeight(uint16 weight) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

143:     function setInitialSupply(uint256 newSupply) public onlyOwner {

147:     function setGradThreshold(uint256 newThreshold) public onlyOwner {

151:     function setFee(uint256 newFee, address newFeeTo) public onlyOwner {

156:     function setMaxTx(uint256 maxTx_) public onlyOwner {

160:     function setAssetRate(uint256 newRate) public onlyOwner {

166:     function setDeployParams(DeployParams memory params) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

121:     function updateMaxTx(uint256 _maxTx) public onlyOwner {

125:     function excludeFromMaxTx(address user) public onlyOwner {

136:     function burnFrom(address user, uint256 amount) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

60:     function createPair(address tokenA, address tokenB) external onlyRole(CREATOR_ROLE) nonReentrant returns (address) {

74:     function setTaxParams(address newVault_, uint256 buyTax_, uint256 sellTax_) public onlyRole(ADMIN_ROLE) {

82:     function setRouter(address router_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

45:     function mint(uint256 reserve0, uint256 reserve1) public onlyRouter returns (bool) {

71:     function approval(address _user, address _token, uint256 amount) public onlyRouter returns (bool) {

82:     function transferAsset(address recipient, uint256 amount) public onlyRouter {

88:     function transferTo(address recipient, uint256 amount) public onlyRouter {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

165:     function graduate(address tokenAddress) public onlyRole(EXECUTOR_ROLE) nonReentrant {

183:     function setTaxManager(address newManager) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

53:     function setParams(Params calldata p) external onlyRole(ADMIN_ROLE) {

130:     function onGenesisFailed(uint256 id, uint256[] calldata participantIndexes) external onlyRole(OPERATION_ROLE) {

143:     function resetTime(uint256 id, uint256 newStartTime, uint256 newEndTime) external onlyRole(OPERATION_ROLE) {

147:     function cancelGenesis(uint256 id) external onlyRole(OPERATION_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

128:     function updateQuorum(uint224 newQuorum) public onlyGovernance {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/libs/TokenSaver.sol

20:     function saveToken(address _token, address _receiver, uint256 _amount) external onlyTokenSaver {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/tax/AgentTax.sol

147:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

157:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

164:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

269:     function dcaSell(uint256[] memory agentIds, uint256 slippage, uint256 maxOverride) public onlyRole(EXECUTOR_ROLE) {

287:     function updateTbaBonus(address tbaBonus_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

101:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

111:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

118:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

122:     function swapForAsset() public onlyBondingRouter returns (bool, uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

36:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

62:     function manualRefund(bytes32 txhash, address recipient, uint256 amount) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

44:     function updateBonusRate(uint16 bonusRate_) public onlyRole(ADMIN_ROLE) {

50:     function setAllowances(uint256[] memory agentIds, uint256[] memory allowances) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/token/Virtual.sol

17:     function mint(address _to, uint256 _amount) external onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

327:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

332:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

336:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

342:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

396:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

341:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

356:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

360:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

364:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

402:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

406:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

410:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

489:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

365:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

370:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

374:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

380:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

384:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

426:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

430:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

434:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

446:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

94:     function setImplementations(address token, address veToken, address dao) external onlyOwner {

183:     function pause() external onlyOwner {

187:     function unpause() external onlyOwner {

191:     function reset(uint256 id) external onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

116:     function addCoreType(string memory label) public onlyRole(DEFAULT_ADMIN_ROLE) {

163:     function setCoreTypes(uint256 virtualId, uint8[] memory coreTypes) external onlyVirtualDAO(virtualId) {

169:     function setTokenURI(uint256 virtualId, string memory newTokenURI) public onlyVirtualDAO(virtualId) {

173:     function setTBA(uint256 virtualId, address tba) external onlyRole(MINTER_ROLE) {

234:     function setBlacklist(uint256 virtualId, bool value) public onlyRole(ADMIN_ROLE) {

239:     function migrateScoreFunctions() public onlyRole(ADMIN_ROLE) {

243:     function setEloCalculator(address eloCalculator) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

214:     function addInitialLiquidity(address lpOwner) external onlyOwnerOrFactory {

300:     function addLiquidityPool(address newLiquidityPool_) public onlyOwnerOrFactory {

321:     function removeLiquidityPool(address removedLiquidityPool_) external onlyOwnerOrFactory {

357:     function addValidCaller(bytes32 newValidCallerHash_) external onlyOwnerOrFactory {

369:     function removeValidCaller(bytes32 removedValidCallerHash_) external onlyOwnerOrFactory {

382:     function setProjectTaxRecipient(address projectTaxRecipient_) external onlyOwnerOrFactory {

394:     function setSwapThresholdBasisPoints(uint16 swapThresholdBasisPoints_) external onlyOwnerOrFactory {

868:     function withdrawETH(uint256 amount_) external onlyOwnerOrFactory {

891:     function withdrawERC20(address token_, uint256 amount_) external onlyOwnerOrFactory {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

12:     function __CoreRegistry_init() internal onlyInitializing {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

57:     function setK(uint256 k_) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

31:     function __GovernorCountingSimple_init() internal onlyInitializing {}

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

### <a name="GAS-16"></a>[GAS-16] `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`)

Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name *post-increment*:

```solidity
uint i = 1;  
uint j = 2;
require(j == i++, "This will be false as i is incremented after the comparison");
```
  
However, pre-increments (or pre-decrements) return the new value:
  
```solidity
uint i = 1;  
uint j = 2;
require(j == ++i, "This will be true as i is incremented before the comparison");
```

In the pre-increment case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`.

Consider using pre-increments and pre-decrements where they are relevant (meaning: not where post-increments/decrements logic are relevant).

*Saves 5 gas per instance*

*Instances (29)*:

```solidity
File: contracts/AgentInference.sol

46:         for (uint256 i = 0; i < amounts.length; i++) {

52:         for (uint256 i = 0; i < agentIds.length; i++) {

74:         for (uint256 i = 0; i < len; i++) {

82:         for (uint256 i = 0; i < len; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/Bonding.sol

412:         for (uint i = 0; i < accounts.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/FGenesis.sol

84:         genesisID++;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

224:         for (uint256 i = 0; i < refundVirtualsTokenUserAmounts.length; i++) {

276:         for (uint256 i = 0; i < distributeAgentTokenUserAmounts.length; i++) {

286:         for (uint256 i = 0; i < refundVirtualsTokenUserAddresses.length; i++) {

298:         for (uint256 i = 0; i < distributeAgentTokenUserAddresses.length; i++) {

327:         for (uint256 i = 0; i < participantIndexes.length; i++) {

333:                 refundUserCountForFailed++;

370:         for (uint256 i = 0; i < actualPageSize; i++) {

388:         for (uint256 i = 0; i < length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

21:         for (uint256 i = 0; i < froms.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/tax/AgentTax.sol

177:         for (uint i = 0; i < txhashes.length; i++) {

272:         for (uint i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

47:         for (uint i = 0; i < txhashes.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

53:         for (uint256 i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

156:         uint256 id = _nextId++;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

159:         uint256 id = _nextId++;

443:         uint256 id = _nextId++;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

157:         uint256 id = _nextId++;

480:         uint256 id = _nextId++;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

96:         _nextVirtualId++;

206:         for (uint256 i = 0; i < total; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

20:         uint8 coreType = _nextCoreType++;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

42:         for (uint256 i = 0; i < battles.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

62:         for (uint256 i = 0; i < validatorCount(virtualId); i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="GAS-17"></a>[GAS-17] Using `private` rather than `public` for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*Instances (26)*:

```solidity
File: contracts/AgentInference.sol

16:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/Bonding.sol

27:     uint256 public constant K = 3_000_000_000_000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FFactory.sol

11:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

12:     bytes32 public constant CREATOR_ROLE = keccak256("CREATOR_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

17:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

18:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

16:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

17:     bytes32 public constant OPERATION_ROLE = keccak256("OPERATION_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

16:     bytes32 public constant FACTORY_ROLE = keccak256("FACTORY_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

14:     bytes32 public constant BONDING_ROLE = keccak256("BONDING_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

31:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/libs/TokenSaver.sol

11:     bytes32 public constant TOKEN_SAVER_ROLE = keccak256("TOKEN_SAVER_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/tax/AgentTax.sol

25:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

26:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

15:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

11:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

12:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

13:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

14:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

35:     bytes32 public constant WITHDRAW_ROLE = keccak256("WITHDRAW_ROLE"); // Able to withdraw and execute applications

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

35:     bytes32 public constant WITHDRAW_ROLE = keccak256("WITHDRAW_ROLE"); // Able to withdraw and execute applications

101:     bytes32 public constant BONDING_ROLE = keccak256("BONDING_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

37:     bytes32 public constant WITHDRAW_ROLE = keccak256("WITHDRAW_ROLE"); // Able to withdraw and execute applications

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

30:     bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");

31:     bytes32 public constant VALIDATOR_ADMIN_ROLE = keccak256("VALIDATOR_ADMIN_ROLE"); // Validator admin can manage validators for all personas

49:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

### <a name="GAS-18"></a>[GAS-18] Use shift right/left instead of division/multiplication if possible

While the `DIV` / `MUL` opcode uses 5 gas, the `SHR` / `SHL` opcode only uses 3 gas. Furthermore, beware that Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting. Eventually, overflow checks are never performed for shift operations as they are done for arithmetic operations. Instead, the result is always truncated, so the calculation can be unchecked in Solidity version `0.8+`

- Use `>> 1` instead of `/ 2`
- Use `>> 2` instead of `/ 4`
- Use `<< 3` instead of `* 8`
- ...
- Use `>> 5` instead of `/ 2^5 == / 32`
- Use `<< 6` instead of `* 2^6 == * 64`

TL;DR:

- Shifting left by N is like multiplying by 2^N (Each bits to the left is an increased power of 2)
- Shifting right by N is like dividing by 2^N (Each bits to the right is a decreased power of 2)

*Saves around 2 gas + 20 for unchecked per instance*

*Instances (3)*:

```solidity
File: contracts/fun/Bonding.sol

228:             liquidity: liquidity * 2,

296:         uint256 liquidity = newReserveB * 2;

333:         uint256 liquidity = newReserveB * 2;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

### <a name="GAS-19"></a>[GAS-19] Splitting require() statements that use && saves gas

*Instances (5)*:

```solidity
File: contracts/AgentInference.sol

44:         require(agentIds.length == amounts.length && agentIds.length == coreIds.length, "Invalid input");

72:         require(len == amounts.length && len == coreIds.length && len == promptHashes.length, "Invalid input");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/Bonding.sol

362:         require(_token.trading && !_token.tradingOnUniswap, "trading is already open");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/FGenesis.sol

66:         require(p.reserve > 0 && p.maxContribution > 0 && p.feeAmt > 0 && p.duration > 0, "Invalid amt");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

20:         require(froms.length == tos.length && tos.length == values.length, "Invalid input");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

### <a name="GAS-20"></a>[GAS-20] `uint256` to `bool` `mapping`: Utilizing Bitmaps to dramatically save on Gas
<https://soliditydeveloper.com/bitmaps>

<https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/structs/BitMaps.sol>

- [BitMaps.sol#L5-L16](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/structs/BitMaps.sol#L5-L16):

```solidity
/**
 * @dev Library for managing uint256 to bool mapping in a compact and efficient way, provided the keys are sequential.
 * Largely inspired by Uniswap's https://github.com/Uniswap/merkle-distributor/blob/master/contracts/MerkleDistributor.sol[merkle-distributor].
 *
 * BitMaps pack 256 booleans across each bit of a single 256-bit slot of `uint256` type.
 * Hence booleans corresponding to 256 _sequential_ indices would only consume a single slot,
 * unlike the regular `bool` which would consume an entire slot for a single value.
 *
 * This results in gas savings in two ways:
 *
 * - Setting a zero value to non-zero only once every 256 times
 * - Accessing the same warm slot for every 256 _sequential_ indices
 */
```

*Instances (4)*:

```solidity
File: contracts/contribution/ContributionNft.sol

28:     mapping(uint256 => bool) public modelContributions;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

24:     mapping(uint256 => bool) _earlyExecutions;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

29:     mapping(uint256 => bool) public migratedAgents;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

50:     mapping(uint256 => bool) private _blacklists;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

### <a name="GAS-21"></a>[GAS-21] Increments/decrements can be unchecked in for-loops

In Solidity 0.8+, there's a default overflow check on unsigned integers. It's possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

[ethereum/solidity#10695](https://github.com/ethereum/solidity/issues/10695)

The change would be:

```diff
- for (uint256 i; i < numIterations; i++) {
+ for (uint256 i; i < numIterations;) {
 // ...  
+   unchecked { ++i; }
}  
```

These save around **25 gas saved** per instance.

The same can be applied with decrements (which should use `break` when `i == 0`).

The risk of overflow is non-existent for `uint256`.

*Instances (21)*:

```solidity
File: contracts/AgentInference.sol

46:         for (uint256 i = 0; i < amounts.length; i++) {

52:         for (uint256 i = 0; i < agentIds.length; i++) {

74:         for (uint256 i = 0; i < len; i++) {

82:         for (uint256 i = 0; i < len; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/Bonding.sol

412:         for (uint i = 0; i < accounts.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/Genesis.sol

224:         for (uint256 i = 0; i < refundVirtualsTokenUserAmounts.length; i++) {

276:         for (uint256 i = 0; i < distributeAgentTokenUserAmounts.length; i++) {

286:         for (uint256 i = 0; i < refundVirtualsTokenUserAddresses.length; i++) {

298:         for (uint256 i = 0; i < distributeAgentTokenUserAddresses.length; i++) {

327:         for (uint256 i = 0; i < participantIndexes.length; i++) {

333:                 refundUserCountForFailed++;

370:         for (uint256 i = 0; i < actualPageSize; i++) {

388:         for (uint256 i = 0; i < length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

21:         for (uint256 i = 0; i < froms.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/tax/AgentTax.sol

177:         for (uint i = 0; i < txhashes.length; i++) {

272:         for (uint i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

47:         for (uint i = 0; i < txhashes.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

53:         for (uint256 i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

206:         for (uint256 i = 0; i < total; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

42:         for (uint256 i = 0; i < battles.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

62:         for (uint256 i = 0; i < validatorCount(virtualId); i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="GAS-22"></a>[GAS-22] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (54)*:

```solidity
File: contracts/contribution/ServiceNft.sol

100:         if (datasetId > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

161:         require(newRate > 0, "Rate err");

415:             if (balance > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

102:         require(amount > 0, "Transfer amount must be greater than zero");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FRouter.sol

138:         require(amountIn > 0, "amountIn must be greater than 0");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

66:         require(p.reserve > 0 && p.maxContribution > 0 && p.feeAmt > 0 && p.duration > 0, "Invalid amt");

69:             p.agentTokenTotalSupply > 0 && p.agentTokenLpSupply > 0 && p.agentTokenTotalSupply >= p.agentTokenLpSupply,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

137:         require(params.genesisID > 0, "Invalid genesis ID");

140:         require(bytes(params.genesisName).length > 0, "Invalid genesis name");

141:         require(bytes(params.genesisTicker).length > 0, "Invalid genesis ticker");

142:         require(params.genesisCores.length > 0, "Invalid genesis cores");

146:         require(params.reserveAmount > 0, "Reserve amount must be greater than 0");

147:         require(params.maxContributionVirtualAmount > 0, "Max contribution must be greater than 0");

148:         require(params.agentTokenTotalSupply > 0, "Agent token total supply must be greater than 0");

149:         require(params.agentTokenLpSupply > 0, "Agent token lp supply must be greater than 0");

178:         require(pointAmt > 0, "Point amount must be greater than 0");

179:         require(virtualsAmt > 0, "Virtuals must be greater than 0");

309:         require(amount > 0, "No tokens to claim");

331:             if (virtualsAmt > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/GenesisLib.sol

25:             bytes(params.genesisName).length > 0 &&

26:                 bytes(params.genesisTicker).length > 0 &&

27:                 params.genesisCores.length > 0,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/GenesisLib.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

26:         if (pos > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

2: pragma solidity >=0.8.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

34:         if (pos > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

31:         if (pos > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/pool/IUniswapV2Factory.sol

1: pragma solidity >=0.5.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Factory.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

1: pragma solidity >=0.5.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/pool/IUniswapV2Router01.sol

1: pragma solidity >=0.6.2;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router01.sol)

```solidity
File: contracts/pool/IUniswapV2Router02.sol

1: pragma solidity >=0.6.2;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router02.sol)

```solidity
File: contracts/tax/AgentTax.sol

179:             if (taxHistory[txhash].agentId > 0) {

235:             if (creatorFee > 0) {

242:             if (feeAmount > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

125:         require(amount > 0, "Nothing to be swapped");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

51:             if (refunds[txhash] > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

65:         require(agentId > 0, "Invalid agent ID");

79:         if (bonus > 0 && assetToken.balanceOf(address(this)) >= bonus) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

138:             if (params.length > 0 && support == 1) {

187:         if (coreService > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

152:         require(cores.length > 0, "Cores must be provided");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

155:         require(cores.length > 0, "Cores must be provided");

439:         require(cores.length > 0, "Cores must be provided");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

153:         require(cores.length > 0, "Cores must be provided");

472:         require(cores.length > 0, "Cores must be provided");

474:         require(initialLP > 0, "InitialLP must be greater than 0");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

177:         if (lpMint_ > 0) {

181:         if (vaultMint_ > 0) {

696:                 if (isLiquidityPool(to_) && totalSellTaxBasisPoints() > 0) {

697:                     if (projectSellTaxBasisPoints > 0) {

704:                 else if (isLiquidityPool(from_) && totalBuyTaxBasisPoints() > 0) {

705:                     if (projectBuyTaxBasisPoints > 0) {

712:                 if (tax > 0) {

847:         if (projectTaxPendingSwap > 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

63:         require(amount > 0, "Cannot stake 0");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="GAS-23"></a>[GAS-23] `internal` functions not called by the contract should be removed

If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

*Instances (24)*:

```solidity
File: contracts/fun/FERC20.sol

131:     function _burn(address user, uint256 amount) internal {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/genesis/GenesisLib.sol

9:     function validateAndDeploy(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/GenesisLib.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

19:     function push(Trace storage self, uint48 key, address value) internal {

43:     function latest(Trace storage self) internal view returns (address) {

48:     function upperLookupRecent(Trace storage self, uint48 key) internal view returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/Elo.sol

20:     function ratingChange(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

16:     function mulWadDown(uint256 x, uint256 y) internal pure returns (uint256) {

20:     function mulWadUp(uint256 x, uint256 y) internal pure returns (uint256) {

24:     function divWadDown(uint256 x, uint256 y) internal pure returns (uint256) {

28:     function divWadUp(uint256 x, uint256 y) internal pure returns (uint256) {

63:     function rpow(uint256 x, uint256 n, uint256 scalar) internal pure returns (uint256 z) {

152:     function sqrt(uint256 x) internal pure returns (uint256 z) {

217:     function unsafeMod(uint256 x, uint256 y) internal pure returns (uint256 z) {

226:     function unsafeDiv(uint256 x, uint256 y) internal pure returns (uint256 r) {

235:     function unsafeDivUp(uint256 x, uint256 y) internal pure returns (uint256 z) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

27:     function push(Trace storage self, uint32 key, RewardSettings memory value) internal {

51:     function latest(Trace storage self) internal view returns (RewardSettings memory) {

56:     function upperLookupRecent(Trace storage self, uint32 key) internal view returns (RewardSettings memory) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

24:     function push(Trace storage self, uint32 key, RewardSettings memory value) internal {

48:     function latest(Trace storage self) internal view returns (RewardSettings memory) {

53:     function upperLookupRecent(Trace storage self, uint32 key) internal view returns (RewardSettings memory) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

141:     function _validatorScoreOf(uint256 virtualId, address account) internal view returns (uint256) {

147:     function _getPastValidatorScore(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

19:     function _addCoreType(string memory label) internal {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

## Non Critical Issues

| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Replace `abi.encodeWithSignature` and `abi.encodeWithSelector` with `abi.encodeCall` which keeps the code typo/type safe | 1 |
| [NC-2](#NC-2) | Missing checks for `address(0)` when assigning values to address state variables | 85 |
| [NC-3](#NC-3) | Array indices should be referenced via `enum`s rather than via numeric literals | 27 |
| [NC-4](#NC-4) | Constants should be in CONSTANT_CASE | 1 |
| [NC-5](#NC-5) | `constant`s should be defined rather than using magic numbers | 53 |
| [NC-6](#NC-6) | Control structures do not follow the Solidity Style Guide | 33 |
| [NC-7](#NC-7) | Critical Changes Should Use Two-step Procedure | 22 |
| [NC-8](#NC-8) | Consider disabling `renounceOwnership()` | 7 |
| [NC-9](#NC-9) | Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function | 78 |
| [NC-10](#NC-10) | Event is never emitted | 8 |
| [NC-11](#NC-11) | Events should use parameters to convey information | 1 |
| [NC-12](#NC-12) | Event missing indexed field | 54 |
| [NC-13](#NC-13) | Events that mark critical parameter changes should contain both the old and the new value | 21 |
| [NC-14](#NC-14) | Function ordering does not follow the Solidity style guide | 26 |
| [NC-15](#NC-15) | Functions should not be longer than 50 lines | 451 |
| [NC-16](#NC-16) | Change int to int256 | 21 |
| [NC-17](#NC-17) | Change uint to uint256 | 105 |
| [NC-18](#NC-18) | Interfaces should be defined in separate files from their usage | 1 |
| [NC-19](#NC-19) | Lack of checks in setters | 71 |
| [NC-20](#NC-20) | Lines are too long | 3 |
| [NC-21](#NC-21) | `type(uint256).max` should be used instead of `2 ** 256 - 1` | 1 |
| [NC-22](#NC-22) | Missing Event for critical parameters change | 58 |
| [NC-23](#NC-23) | NatSpec is completely non-existent on functions that should have them | 184 |
| [NC-24](#NC-24) | File's first line is not an SPDX Identifier | 7 |
| [NC-25](#NC-25) | Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor | 14 |
| [NC-26](#NC-26) | Constant state variables defined more than once | 21 |
| [NC-27](#NC-27) | Consider using named mappings | 33 |
| [NC-28](#NC-28) | Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables | 6 |
| [NC-29](#NC-29) | Owner can renounce while system is paused | 2 |
| [NC-30](#NC-30) | Adding a `return` statement when the function defines a named return variable, is redundant | 24 |
| [NC-31](#NC-31) | `require()` / `revert()` statements should have descriptive reason strings | 1 |
| [NC-32](#NC-32) | Take advantage of Custom Error's return value property | 25 |
| [NC-33](#NC-33) | Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. `10**18`) | 1 |
| [NC-34](#NC-34) | Use scientific notation for readability reasons for large multiples of ten | 2 |
| [NC-35](#NC-35) | Avoid the use of sensitive terms | 9 |
| [NC-36](#NC-36) | Contract does not follow the Solidity style guide's suggested layout ordering | 25 |
| [NC-37](#NC-37) | Use Underscores for Number Literals (add an underscore every 3 digits) | 27 |
| [NC-38](#NC-38) | Internal and private variables and functions names should begin with an underscore | 30 |
| [NC-39](#NC-39) | Event is missing `indexed` fields | 75 |
| [NC-40](#NC-40) | Constants should be defined rather than using magic numbers | 6 |
| [NC-41](#NC-41) | `public` functions not called by the contract should be declared `external` instead | 178 |
| [NC-42](#NC-42) | Variables need not be initialized to zero | 33 |

### <a name="NC-1"></a>[NC-1] Replace `abi.encodeWithSignature` and `abi.encodeWithSelector` with `abi.encodeCall` which keeps the code typo/type safe

When using `abi.encodeWithSignature`, it is possible to include a typo for the correct function signature.
When using `abi.encodeWithSignature` or `abi.encodeWithSelector`, it is also possible to provide parameters that are not of the correct type for the function.

To avoid these pitfalls, it would be best to use [`abi.encodeCall`](https://solidity-by-example.org/abi-encode/) instead.

*Instances (1)*:

```solidity
File: contracts/contribution/ServiceNft.sol

63:         bytes memory mintCalldata = abi.encodeWithSignature("mint(uint256,bytes32)", virtualId, descHash);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

### <a name="NC-2"></a>[NC-2] Missing checks for `address(0)` when assigning values to address state variables

*Instances (85)*:

```solidity
File: contracts/contribution/ContributionNft.sol

47:         personaNft = thePersonaAddress;

105:         _admin = newAdmin;

167:         _eloCalculator = eloCalculator_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

53:         personaNft = initialAgentNft;

54:         contributionNft = initialContributionNft;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

110:         _feeTo = feeTo_;

117:         agentFactory = agentFactory_;

153:         _feeTo = newFeeTo;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FFactory.sol

36:         taxVault = taxVault_;

83:         router = router_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

184:         taxManager = newManager;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

40:         mockAgentToken = token;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

36:         router = router_;

37:         tokenIn = tokenIn_;

38:         tokenOut = tokenOut_;

39:         factory = factory_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/tax/AgentTax.sol

102:         treasury = treasury_;

127:         assetToken = assetToken_;

159:         treasury = treasury_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

68:         assetToken = assetToken_;

69:         taxToken = taxToken_;

71:         bondingRouter = bondingRouter_;

72:         treasury = treasury_;

90:         assetToken = assetToken_;

92:         bondingRouter = bondingRouter_;

113:         treasury = treasury_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

33:         taxToken = taxToken_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

54:         _agentNft = agentNft;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

120:         tokenImplementation = tokenImplementation_;

121:         veTokenImplementation = veTokenImplementation_;

122:         daoImplementation = daoImplementation_;

123:         assetToken = assetToken_;

124:         tbaRegistry = tbaRegistry_;

125:         nft = nft_;

129:         _vault = vault_;

333:         _vault = newVault;

337:         tokenImplementation = token;

338:         daoImplementation = dao;

339:         veTokenImplementation = veToken;

347:         _uniswapRouter = router;

351:         _tokenAdmin = newTokenAdmin;

389:         assetToken = newToken;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

123:         tokenImplementation = tokenImplementation_;

124:         veTokenImplementation = veTokenImplementation_;

125:         daoImplementation = daoImplementation_;

126:         assetToken = assetToken_;

127:         tbaRegistry = tbaRegistry_;

128:         nft = nft_;

132:         _vault = vault_;

347:         _vault = newVault;

351:         tokenImplementation = token;

352:         daoImplementation = dao;

353:         veTokenImplementation = veToken;

361:         _uniswapRouter = router;

365:         _tokenAdmin = newTokenAdmin;

403:         assetToken = newToken;

490:         defaultDelegatee = newDelegatee;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

121:         tokenImplementation = tokenImplementation_;

122:         veTokenImplementation = veTokenImplementation_;

123:         daoImplementation = daoImplementation_;

124:         assetToken = assetToken_;

125:         tbaRegistry = tbaRegistry_;

126:         nft = nft_;

130:         _vault = vault_;

371:         _vault = newVault;

375:         tokenImplementation = token;

376:         daoImplementation = dao;

377:         veTokenImplementation = veToken;

385:         _uniswapRouter = router;

389:         _tokenAdmin = newTokenAdmin;

427:         assetToken = newToken;

447:         defaultDelegatee = newDelegatee;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

53:         _tokenAdmin = tokenAdmin_;

54:         _assetToken = assetToken_;

55:         _uniswapRouter = uniswapRouter_;

95:         tokenImplementation = token;

96:         daoImplementation = dao;

97:         veTokenImplementation = veToken;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

77:         _contributionNft = contributionNft_;

78:         _serviceNft = serviceNft_;

244:         _eloCalculator = eloCalculator;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

383:         projectTaxRecipient = projectTaxRecipient_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

51:         founder = _founder;

53:         assetToken = _assetToken;

54:         agentNft = _agentNft;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="NC-3"></a>[NC-3] Array indices should be referenced via `enum`s rather than via numeric literals

*Instances (27)*:

```solidity
File: contracts/contribution/ServiceNft.sol

65:         targets[0] = address(this);

67:         values[0] = 0;

69:         calldatas[0] = mintCalldata;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

243:             twitter: urls[0],

244:             telegram: urls[1],

245:             youtube: urls[2],

246:             website: urls[3],

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

51:         routes[0] = IAeroRouter.Route(tokenIn, tokenOut, false, factory);

60:         routes[0] = IAeroRouter.Route(tokenIn, tokenOut, false, factory);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/tax/AgentTax.sol

220:         path[0] = taxToken;

221:         path[1] = assetToken;

229:             uint256 assetReceived = amounts[1];

248:             return (true, amounts[1]);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

136:         path[0] = taxToken;

137:         path[1] = assetToken;

142:         uint256 expectedOutput = amountsOut[1];

148:             emit SwapExecuted(amount, amounts[1]);

149:             return (true, amounts[1]);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

221:         address lp = IAgentToken(token).liquidityPools()[0];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

214:         address lp = IAgentToken(token).liquidityPools()[0];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

221:             lp = IAgentToken(token).liquidityPools()[0];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

109:         address lp = IAgentToken(token).liquidityPools()[0];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

85:         _decodeBaseParams(integrationAddresses_[0], baseParams_);

86:         _uniswapRouter = IUniswapV2Router02(integrationAddresses_[1]);

87:         pairToken = integrationAddresses_[2];

786:         path[0] = address(this);

787:         path[1] = pairToken;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="NC-4"></a>[NC-4] Constants should be in CONSTANT_CASE

For `constant` variable names, each word should use all capital letters, with underscores separating each word (CONSTANT_CASE)

*Instances (1)*:

```solidity
File: contracts/fun/FERC20.sol

8:     uint8 private constant _decimals = 18;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

### <a name="NC-5"></a>[NC-5] `constant`s should be defined rather than using magic numbers

Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*Instances (53)*:

```solidity
File: contracts/contribution/ServiceNft.sol

101:             _impacts[datasetId] = (rawImpact * datasetImpactWeight) / 10000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

111:         fee = (fee_ * 1 ether) / 1000;

215:         uint256 k = ((K * 10000) / assetRate);

216:         uint256 liquidity = (((k * 10000 ether) / supply) * 1 ether) / 10000;

228:             liquidity: liquidity * 2,

296:         uint256 liquidity = newReserveB * 2;

299:         uint256 volume = duration > 86400 ? amount1Out : tokenInfo[tokenAddress].data.volume24H + amount1Out;

300:         uint256 prevPrice = duration > 86400

311:         if (duration > 86400) {

333:         uint256 liquidity = newReserveB * 2;

336:         uint256 volume = duration > 86400 ? amount1In : tokenInfo[tokenAddress].data.volume24H + amount1In;

337:         uint256 _price = duration > 86400 ? tokenInfo[tokenAddress].data.price : tokenInfo[tokenAddress].data.prevPrice;

346:         if (duration > 86400) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

33:         _totalSupply = supply * 10 ** _decimals;

116:         _maxTxAmount = (maxTx * _totalSupply) / 100;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FRouter.sol

114:         uint256 txFee = (fee * amountOut) / 100;

143:         uint256 txFee = (fee * amountIn) / 100;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

43:         _quorumCheckpoints.push(0, 10000e18);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/VirtualProtocolDAO.sol

70:         return 10000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualProtocolDAO.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

54:         if (len > 5) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/Elo.sol

33:             _kFactor = kFactor * 10_000;

39:         require(ratingDiff < 1126, "Rating difference too large");

41:         if (_negative) require(ratingDiff < 800, "Rating difference too large");

56:             n = _negative ? 800 - ratingDiff : 800 + ratingDiff;

59:             _powered = fp.rpow(10, n / 25, 1); // divide by 25 to avoid reach uint256 max

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

79:                 switch mod(n, 2)

120:                     if mod(n, 2) {

157:             z := 181 // The "correct" value is 1, but this saves a multiplication later.

197:             z := shr(18, mul(z, add(y, 65536))) // A mul() is saved from starting z at 181.

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

62:         if (len > 5) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

59:         if (len > 5) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/tax/AgentTax.sol

108:         feeRate = 100;

109:         creatorFeeRate = 3000;

227:             router.swapExactTokensForTokens(amountToSwap, minOutput, path, address(this), block.timestamp + 300)

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

77:         _slippage = 100; // default to 1%

143:         uint256 minOutput = (expectedOutput * (10000 - _slippage)) / 10000;

145:         try router.swapExactTokensForTokens(amount, minOutput, path, treasury, block.timestamp + 300) returns (

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/TBABonus.sol

41:         bonusRate = 3500;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/token/Virtual.sol

13:     ) ERC20("Virtual Protocol", "VIRTUAL") ERC20Capped(1000000000 * 10 ** 18) Ownable(initialOwner) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

186:         uint256 maturity = 100;

207:         return 10000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

455:         return 18;

796:                 block.timestamp + 600

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

20:         k = 30;

25:             return 100;

26:         } else if (result == 2) {

27:             return 50;

28:         } else if (result == 3) {

29:             return 50;

40:         uint256 eloA = 1000;

41:         uint256 eloB = 1000;

45:             change = _roundUp(change, 100);

54:         return currentRating + eloA - 1000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="NC-6"></a>[NC-6] Control structures do not follow the Solidity Style Guide

See the [control structures](https://docs.soliditylang.org/en/latest/style-guide.html#control-structures) section of the Solidity Style Guide

*Instances (33)*:

```solidity
File: contracts/fun/Bonding.sol

13: import "./IFPair.sol";

171:         require(_checkIfProfileExists(account), "User Profile dose not exist.");

253:         bool exists = _checkIfProfileExists(creator);

286:         IFPair pair = IFPair(pairAddress);

323:         IFPair pair = IFPair(pairAddress);

370:         IFPair pair = IFPair(pairAddress);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FPair.sol

8: import "./IFPair.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

11: import "./IFPair.sol";

50:         IFPair pair = IFPair(pairAddress);

84:         IFPair pair = IFPair(pairAddress);

105:         IFPair pair = IFPair(pairAddress);

154:         IFPair(pair).transferTo(to, amountOut);

156:         IFPair(pair).swap(0, amountOut, amount, 0);

168:         uint256 assetBalance = IFPair(pair).assetBalance();

180:         IFPair(pair).approval(spender, asset, amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/libs/Elo.sol

28:         uint256 ratingDiff; // absolute value difference between `ratingA` and `ratingB`

34:             ratingDiff = _negative ? ratingA - ratingB : ratingB - ratingA;

39:         require(ratingDiff < 1126, "Rating difference too large");

41:         if (_negative) require(ratingDiff < 800, "Rating difference too large");

48:         uint256 n; // numerator of the power, with scaling, (numerator of `ratingDiff / 400`)

50:         uint256 powered; // the value of 16th root of 10 ^ numerator (fully resolved 10 ^ (ratingDiff / 400))

56:             n = _negative ? 800 - ratingDiff : 800 + ratingDiff;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

9:                     SIMPLIFIED FIXED POINT OPERATIONS

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/token/Airdrop.sol

36:             if iszero(

72:                 if iszero(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Airdrop.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

134:         if (

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/IAgentToken.sol

12:     event ExternalCallError(uint256 identifier);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentToken.sol)

```solidity
File: contracts/virtualPersona/IErrors.sol

33:     error AuctionStatusIsNotEnded(); //                       Throw if the action required the auction to be closed, and it isn't.

35:     error AuctionStatusIsNotOpen(); //                        Throw if the action requires the auction to be open, and it isn't.

47:     error BondingCurveError(BondingCurveErrorType error); //  An error of the type specified has occured in bonding curve processing.

79:     error CannotWithdrawThisToken(); //                       Cannot withdraw the specified token.

81:     error CanOnlyReduce(); //                                 The given operation can only reduce the value specified.

259:     error RoyaltyFeeWillExceedSalePrice(); //                 The ERC2981 royalty specified will exceed the sale price.

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IErrors.sol)

### <a name="NC-7"></a>[NC-7] Critical Changes Should Use Two-step Procedure

The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference: <https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure>

**Recommended Mitigation Steps**

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

*Instances (22)*:

```solidity
File: contracts/contribution/ContributionNft.sol

103:     function setAdmin(address newAdmin) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/fun/FFactory.sol

74:     function setTaxParams(address newVault_, uint256 buyTax_, uint256 sellTax_) public onlyRole(ADMIN_ROLE) {

82:     function setRouter(address router_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

183:     function setTaxManager(address newManager) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

332:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

336:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

346:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

360:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

364:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

402:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

489:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

370:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

374:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

384:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

426:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

446:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

243:     function setEloCalculator(address eloCalculator) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

### <a name="NC-8"></a>[NC-8] Consider disabling `renounceOwnership()`

If the plan for your project does not include eventually giving up all ownership control, consider overwriting OpenZeppelin's `Ownable`'s `renounceOwnership()` function in order to disable it.

*Instances (7)*:

```solidity
File: contracts/fun/Bonding.sol

18: contract Bonding is Initializable, ReentrancyGuardUpgradeable, OwnableUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

7: contract FERC20 is Context, IERC20, Ownable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

9: contract veVirtualToken is ERC20, ERC20Permit, ERC20Votes, Ownable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/token/Virtual.sol

9: contract VirtualToken is ERC20Capped, Ownable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

15: contract AgentMigrator is Ownable, Pausable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

14: contract AgentToken is ContextUpgradeable, IAgentToken, Ownable2StepUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

10: contract EloCalculator is IEloCalculator, Initializable, OwnableUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="NC-9"></a>[NC-9] Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function

*Instances (78)*:

```solidity
File: contracts/AgentInference.sol

44:         require(agentIds.length == amounts.length && agentIds.length == coreIds.length, "Invalid input");

50:         require(token.balanceOf(sender) >= total, "Insufficient balance");

72:         require(len == amounts.length && len == coreIds.length && len == promptHashes.length, "Invalid input");

78:         require(token.balanceOf(sender) >= total, "Insufficient balance");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/Bonding.sol

282:         require(tokenInfo[tokenAddress].trading, "Token not trading");

319:         require(tokenInfo[tokenAddress].trading, "Token not trading");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

132:         require(user != address(0), "Invalid address");

137:         require(user != address(0), "Invalid address");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

42:         require(tokenA != address(0), "Zero addresses are not allowed.");

43:         require(tokenB != address(0), "Zero addresses are not allowed.");

75:         require(newVault_ != address(0), "Zero addresses are not allowed.");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

27:         require(router_ != address(0), "Zero addresses are not allowed.");

28:         require(token0 != address(0), "Zero addresses are not allowed.");

29:         require(token1 != address(0), "Zero addresses are not allowed.");

72:         require(_user != address(0), "Zero addresses are not allowed.");

73:         require(_token != address(0), "Zero addresses are not allowed.");

83:         require(recipient != address(0), "Zero addresses are not allowed.");

89:         require(recipient != address(0), "Zero addresses are not allowed.");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

34:         require(factory_ != address(0), "Zero addresses are not allowed.");

35:         require(assetToken_ != address(0), "Zero addresses are not allowed.");

46:         require(token != address(0), "Zero addresses are not allowed.");

80:         require(token_ != address(0), "Zero addresses are not allowed.");

100:         require(tokenAddress != address(0), "Zero addresses are not allowed.");

101:         require(to != address(0), "Zero addresses are not allowed.");

136:         require(tokenAddress != address(0), "Zero addresses are not allowed.");

137:         require(to != address(0), "Zero addresses are not allowed.");

166:         require(tokenAddress != address(0), "Zero addresses are not allowed.");

178:         require(spender != address(0), "Zero addresses are not allowed.");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

66:         require(p.reserve > 0 && p.maxContribution > 0 && p.feeAmt > 0 && p.duration > 0, "Invalid amt");

68:         require(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

75:         require(!isStarted(), ERR_ALREADY_STARTED);

80:         require(isStarted(), ERR_NOT_STARTED);

85:         require(!isEnded(), ERR_ALREADY_ENDED);

90:         require(isEnded(), ERR_NOT_ENDED);

95:         require(!isFailed, ERR_ALREADY_FAILED);

100:         require(!isCancelled, ERR_ALREADY_CANCELLED);

105:         require(agentTokenAddress == address(0), ERR_TOKEN_LAUNCHED);

110:         require(agentTokenAddress != address(0), ERR_TOKEN_NOT_LAUNCHED);

116:         require(isStarted(), ERR_NOT_STARTED);

117:         require(!isEnded(), ERR_ALREADY_ENDED);

118:         require(!isFailed, ERR_ALREADY_FAILED);

119:         require(!isCancelled, ERR_ALREADY_CANCELLED);

124:         require(isEnded(), ERR_NOT_ENDED);

130:         require(_startTime > block.timestamp, ERR_START_TIME_FUTURE);

131:         require(_endTime > _startTime, ERR_END_AFTER_START);

185:         require(IERC20(virtualTokenAddress).balanceOf(msg.sender) >= virtualsAmt, "Insufficient Virtual Token balance");

238:         require(

328:             require(participantIndexes[i] < participants.length, "Index out of bounds");

390:             require(participantIndexes[i] < participants.length, "Index out of bounds");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/libs/Elo.sol

39:         require(ratingDiff < 1126, "Rating difference too large");

41:         if (_negative) require(ratingDiff < 800, "Rating difference too large");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

182:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

184:         require(application.status == ApplicationStatus.Active, "Application is not active");

205:         require(_applications[id].status == ApplicationStatus.Active, "Application is not active");

211:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

150:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

151:         require(

155:         require(cores.length > 0, "Cores must be provided");

185:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

187:         require(application.status == ApplicationStatus.Active, "Application is not active");

200:         require(_applications[id].status == ApplicationStatus.Active, "Application is not active");

280:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

434:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold_, "Insufficient asset token");

435:         require(

439:         require(cores.length > 0, "Cores must be provided");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

148:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

149:         require(

153:         require(cores.length > 0, "Cores must be provided");

183:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

185:         require(application.status == ApplicationStatus.Active, "Application is not active");

206:         require(_applications[id].status == ApplicationStatus.Active, "Application is not active");

304:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

465:         require(IERC20(assetToken).balanceOf(sender) >= applicationThreshold, "Insufficient asset token");

467:         require(

472:         require(cores.length > 0, "Cores must be provided");

516:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

34:         require(_msgSender() == virtualInfos[virtualId].dao, "Caller is not VIRTUAL DAO");

180:         require(_msgSender() == virtualInfos[virtualId].dao, "Caller is not VIRTUAL DAO");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

### <a name="NC-10"></a>[NC-10] Event is never emitted

The following are defined but never emitted. They can be removed to make the code cleaner.

*Instances (8)*:

```solidity
File: contracts/fun/Bonding.sol

85:     event Deployed(address indexed token, uint256 amount0, uint256 amount1);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/Genesis.sol

60:     event VirtualsWithdrawn(uint256 indexed genesisID, address indexed to, address token, uint256 amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

72:     event GovUpdated(address newGov);

73:     event ImplContractsUpdated(address token, address dao);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

72:     event GovUpdated(address newGov);

73:     event ImplContractsUpdated(address token, address dao);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

67:     event GovUpdated(address newGov);

68:     event ImplContractsUpdated(address token, address dao);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

### <a name="NC-11"></a>[NC-11] Events should use parameters to convey information

For example, rather than using `event Paused()` and `event Unpaused()`, use `event PauseState(address indexed whoChangedIt, bool wasPaused, bool isNowPaused)`

*Instances (1)*:

```solidity
File: contracts/virtualPersona/IAgentToken.sol

36:     event RevenueAutoSwap();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentToken.sol)

### <a name="NC-12"></a>[NC-12] Event missing indexed field

Index event fields make the field more quickly accessible [to off-chain tools](https://ethereum.stackexchange.com/questions/40396/can-somebody-please-explain-the-concept-of-event-indexing) that parse events. This is especially useful when it comes to filtering based on an address. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Where applicable, each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three applicable fields, all of the applicable fields should be indexed.

*Instances (54)*:

```solidity
File: contracts/contribution/ContributionNft.sol

31:     event NewContribution(uint256 tokenId, uint256 virtualId, uint256 parentId, uint256 datasetId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/IServiceNft.sol

19:     event CoreServiceUpdated(uint256 virtualId, uint8 coreType, uint256 serviceId);

21:     event NewService(uint256 tokenId, uint8 coreId, uint256 maturity, uint256 impact, bool isModel);

23:     event DatasetImpactUpdated(uint16 weight);

25:     event SetServiceScore(uint256 serviceId, uint256 eloRating, uint256 impact);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/IServiceNft.sol)

```solidity
File: contracts/fun/FERC20.sol

26:     event MaxTxUpdated(uint _maxTx);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FPair.sol

41:     event Mint(uint256 reserve0, uint256 reserve1);

43:     event Swap(uint256 amount0In, uint256 amount0Out, uint256 amount1In, uint256 amount1Out);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/genesis/Genesis.sol

51:     event TimeReset(uint256 oldStartTime, uint256 oldEndTime, uint256 newStartTime, uint256 newEndTime);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

29:     event QuorumUpdated(uint224 oldQuorum, uint224 newQuorum);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

34:     event Sync(uint112 reserve0, uint112 reserve1);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/tax/AgentTax.sol

39:     event SwapThresholdUpdated(

45:     event TreasuryUpdated(address oldTreasury, address newTreasury);

60:     event SwapParamsUpdated2(

74:     event CreatorUpdated(uint256 agentId, address oldCreator, address newCreator);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

26:     event SwapParamsUpdated(

34:     event SwapThresholdUpdated(

40:     event TreasuryUpdated(address oldTreasury, address newTreasury);

41:     event SwapExecuted(uint256 taxTokenAmount, uint256 assetTokenAmount);

42:     event SwapFailed(uint256 taxTokenAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/TBABonus.sol

24:     event BonusRateUpdated(uint16 oldBonusRate, uint16 newBonusRate);

25:     event AllowanceUpdated(uint256 agentId, uint256 newAllowance);

26:     event PaidAgent(uint256 agentId, uint256 amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

37:     event NewPersona(uint256 virtualId, address token, address dao, address tba, address veToken, address lp);

38:     event NewApplication(uint256 id);

71:     event ApplicationThresholdUpdated(uint256 newThreshold);

72:     event GovUpdated(address newGov);

73:     event ImplContractsUpdated(address token, address dao);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

37:     event NewPersona(uint256 virtualId, address token, address dao, address tba, address veToken, address lp);

38:     event NewApplication(uint256 id);

71:     event ApplicationThresholdUpdated(uint256 newThreshold);

72:     event GovUpdated(address newGov);

73:     event ImplContractsUpdated(address token, address dao);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

39:     event NewPersona(uint256 virtualId, address token, address dao, address tba, address veToken, address lp);

40:     event NewApplication(uint256 id);

66:     event ApplicationThresholdUpdated(uint256 newThreshold);

67:     event GovUpdated(address newGov);

68:     event ImplContractsUpdated(address token, address dao);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

33:     event AgentMigrated(uint256 virtualId, address dao, address token, address lp, address veToken);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

10:     event NewCoreType(uint8 coreType, string label);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/IAgentDAO.sol

26:     event ValidatorEloRating(uint256 proposalId, address voter, uint256 score, uint8[] votes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentDAO.sol)

```solidity
File: contracts/virtualPersona/IAgentNft.sol

15:     event CoresUpdated(uint256 virtualId, uint8[] coreTypes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentNft.sol)

```solidity
File: contracts/virtualPersona/IAgentToken.sol

10:     event AutoSwapThresholdUpdated(uint256 oldThreshold, uint256 newThreshold);

12:     event ExternalCallError(uint256 identifier);

14:     event InitialLiquidityAdded(uint256 tokenA, uint256 tokenB, uint256 lpToken);

16:     event LimitsUpdated(

23:     event LiquidityPoolCreated(address addedPool);

25:     event LiquidityPoolAdded(address addedPool);

27:     event LiquidityPoolRemoved(address removedPool);

29:     event ProjectTaxBasisPointsChanged(

38:     event ProjectTaxRecipientUpdated(address treasury);

40:     event ValidCallerAdded(bytes32 addedValidCaller);

42:     event ValidCallerRemoved(bytes32 removedValidCaller);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentToken.sol)

```solidity
File: contracts/virtualPersona/IValidatorRegistry.sol

5:     event NewValidator(uint256 virtualId, address account);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IValidatorRegistry.sol)

### <a name="NC-13"></a>[NC-13] Events that mark critical parameter changes should contain both the old and the new value

This should especially be done if the new value is not required to be different from the old value

*Instances (21)*:

```solidity
File: contracts/contribution/ServiceNft.sol

90:     function updateImpact(uint256 virtualId, uint256 proposalId) public {
            // Calculate impact
            // Get current service maturity
            uint256 prevServiceId = _coreServices[virtualId][_cores[proposalId]];
            uint256 rawImpact = (_maturities[proposalId] > _maturities[prevServiceId])
                ? _maturities[proposalId] - _maturities[prevServiceId]
                : 0;
            uint256 datasetId = IContributionNft(contributionNft).getDatasetId(proposalId);
    
            _impacts[proposalId] = rawImpact;
            if (datasetId > 0) {
                _impacts[datasetId] = (rawImpact * datasetImpactWeight) / 10000;
                _impacts[proposalId] = rawImpact - _impacts[datasetId];
                emit SetServiceScore(datasetId, _maturities[proposalId], _impacts[datasetId]);
                _maturities[datasetId] = _maturities[proposalId];
            }
    
            emit SetServiceScore(proposalId, _maturities[proposalId], _impacts[proposalId]);

90:     function updateImpact(uint256 virtualId, uint256 proposalId) public {
            // Calculate impact
            // Get current service maturity
            uint256 prevServiceId = _coreServices[virtualId][_cores[proposalId]];
            uint256 rawImpact = (_maturities[proposalId] > _maturities[prevServiceId])
                ? _maturities[proposalId] - _maturities[prevServiceId]
                : 0;
            uint256 datasetId = IContributionNft(contributionNft).getDatasetId(proposalId);
    
            _impacts[proposalId] = rawImpact;
            if (datasetId > 0) {
                _impacts[datasetId] = (rawImpact * datasetImpactWeight) / 10000;
                _impacts[proposalId] = rawImpact - _impacts[datasetId];
                emit SetServiceScore(datasetId, _maturities[proposalId], _impacts[datasetId]);

141:     function setDatasetImpactWeight(uint16 weight) public onlyOwner {
             datasetImpactWeight = weight;
             emit DatasetImpactUpdated(weight);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

128:     function updateQuorum(uint224 newQuorum) public onlyGovernance {
             uint224 oldQuorum = _quorumCheckpoints.latest();
             _quorumCheckpoints.push(SafeCast.toUint32(clock()), SafeCast.toUint208(newQuorum));
             emit QuorumUpdated(oldQuorum, newQuorum);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/tax/AgentTax.sol

115:     function updateSwapParams(
             address router_,
             address assetToken_,
             uint16 feeRate_,
             uint16 creatorFeeRate_
         ) public onlyRole(ADMIN_ROLE) {
             require((feeRate_ + creatorFeeRate_) == DENOM, "Invalid fee rates");
             address oldRouter = address(router);
             address oldAsset = assetToken;
             uint16 oldFee = feeRate;
             uint16 oldCreatorFee = creatorFeeRate;
     
             assetToken = assetToken_;
             router = IRouter(router_);
             feeRate = feeRate_;
             creatorFeeRate = creatorFeeRate_;
     
             IERC20(taxToken).forceApprove(oldRouter, 0);
             IERC20(taxToken).forceApprove(router_, type(uint256).max);
     
             emit SwapParamsUpdated2(

147:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {
             uint256 oldMin = minSwapThreshold;
             uint256 oldMax = maxSwapThreshold;
     
             minSwapThreshold = minSwapThreshold_;
             maxSwapThreshold = maxSwapThreshold_;
     
             emit SwapThresholdUpdated(oldMin, minSwapThreshold_, oldMax, maxSwapThreshold_);

157:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {
             address oldTreasury = treasury;
             treasury = treasury_;
     
             emit TreasuryUpdated(oldTreasury, treasury_);

255:     function updateCreator(uint256 agentId, address creator) public {
             address sender = _msgSender();
             TaxRecipient storage recipient = _agentRecipients[agentId];
             if (recipient.tba == address(0)) {
                 IAgentNft.VirtualInfo memory info = agentNft.virtualInfo(agentId);
                 recipient.tba = info.tba;
                 recipient.creator = info.founder;
             }
             address oldCreator = recipient.creator;
             require(sender == recipient.creator || hasRole(ADMIN_ROLE, sender), "Only creator can update");
             recipient.creator = creator;
             emit CreatorUpdated(agentId, oldCreator, creator);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

80:     function updateSwapParams(
            address router_,
            address bondingRouter_,
            address assetToken_,
            uint16 slippage_
        ) public onlyRole(ADMIN_ROLE) {
            address oldRouter = address(router);
            address oldBondingRouter = bondingRouter;
            address oldAsset = assetToken;
    
            assetToken = assetToken_;
            router = IRouter(router_);
            bondingRouter = bondingRouter_;
            _slippage = slippage_;
    
            IERC20(taxToken).forceApprove(router_, type(uint256).max);
            IERC20(taxToken).forceApprove(oldRouter, 0);
    
            emit SwapParamsUpdated(oldRouter, router_, oldBondingRouter, bondingRouter_, oldAsset, assetToken_);

101:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {
             uint256 oldMin = minSwapThreshold;
             uint256 oldMax = maxSwapThreshold;
     
             minSwapThreshold = minSwapThreshold_;
             maxSwapThreshold = maxSwapThreshold_;
     
             emit SwapThresholdUpdated(oldMin, minSwapThreshold_, oldMax, maxSwapThreshold_);

111:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {
             address oldTreasury = treasury;
             treasury = treasury_;
     
             emit TreasuryUpdated(oldTreasury, treasury_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/TBABonus.sol

44:     function updateBonusRate(uint16 bonusRate_) public onlyRole(ADMIN_ROLE) {
            uint16 oldBonusRate = bonusRate;
            bonusRate = bonusRate_;
            emit BonusRateUpdated(bonusRate_, oldBonusRate);

50:     function setAllowances(uint256[] memory agentIds, uint256[] memory allowances) public onlyRole(ADMIN_ROLE) {
            require(agentIds.length == allowances.length, "Invalid input");
    
            for (uint256 i = 0; i < agentIds.length; i++) {
                uint256 agentId = agentIds[i];
                uint256 allowance = allowances[i];
    
                require(allowance >= _agentPaidAmounts[agentId], "Allowance cannot be less than paid amount");
    
                _agentAllowances[agentId] = allowance;
                emit AllowanceUpdated(agentId, allowance);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

327:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {
             applicationThreshold = newThreshold;
             emit ApplicationThresholdUpdated(newThreshold);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

341:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {
             applicationThreshold = newThreshold;
             emit ApplicationThresholdUpdated(newThreshold);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

365:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {
             applicationThreshold = newThreshold;
             emit ApplicationThresholdUpdated(newThreshold);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

163:     function setCoreTypes(uint256 virtualId, uint8[] memory coreTypes) external onlyVirtualDAO(virtualId) {
             VirtualInfo storage info = virtualInfos[virtualId];
             info.coreTypes = coreTypes;
             emit CoresUpdated(virtualId, coreTypes);

234:     function setBlacklist(uint256 virtualId, bool value) public onlyRole(ADMIN_ROLE) {
             _blacklists[virtualId] = value;
             emit AgentBlacklisted(virtualId, value);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

382:     function setProjectTaxRecipient(address projectTaxRecipient_) external onlyOwnerOrFactory {
             projectTaxRecipient = projectTaxRecipient_;
             emit ProjectTaxRecipientUpdated(projectTaxRecipient_);

394:     function setSwapThresholdBasisPoints(uint16 swapThresholdBasisPoints_) external onlyOwnerOrFactory {
             uint256 oldswapThresholdBasisPoints = swapThresholdBasisPoints;
             swapThresholdBasisPoints = swapThresholdBasisPoints_;
             emit AutoSwapThresholdUpdated(oldswapThresholdBasisPoints, swapThresholdBasisPoints_);

408:     function setProjectTaxRates(
             uint16 newProjectBuyTaxBasisPoints_,
             uint16 newProjectSellTaxBasisPoints_
         ) external onlyOwnerOrFactory {
             uint16 oldBuyTaxBasisPoints = projectBuyTaxBasisPoints;
             uint16 oldSellTaxBasisPoints = projectSellTaxBasisPoints;
     
             projectBuyTaxBasisPoints = newProjectBuyTaxBasisPoints_;
             projectSellTaxBasisPoints = newProjectSellTaxBasisPoints_;
     
             emit ProjectTaxBasisPointsChanged(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="NC-14"></a>[NC-14] Function ordering does not follow the Solidity style guide

According to the [Solidity style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions), functions should be laid out in the following order :`constructor()`, `receive()`, `fallback()`, `external`, `public`, `internal`, `private`, but the cases below do not follow this pattern

*Instances (26)*:

```solidity
File: contracts/contribution/ContributionNft.sol

1: 
   Current order:
   public initialize
   public tokenVirtualId
   public getAgentDAO
   public isAccepted
   external mint
   public getAdmin
   public setAdmin
   public tokenURI
   public getChildren
   public getParentId
   public getCore
   public supportsInterface
   internal _increaseBalance
   internal _update
   public isModel
   public ownerOf
   external getDatasetId
   external getEloCalculator
   public setEloCalculator
   
   Suggested order:
   external mint
   external getDatasetId
   external getEloCalculator
   public initialize
   public tokenVirtualId
   public getAgentDAO
   public isAccepted
   public getAdmin
   public setAdmin
   public tokenURI
   public getChildren
   public getParentId
   public getCore
   public supportsInterface
   public isModel
   public ownerOf
   public setEloCalculator
   internal _increaseBalance
   internal _update

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/fun/Bonding.sol

1: 
   Current order:
   external initialize
   internal _createUserProfile
   internal _checkIfProfileExists
   internal _approval
   public setInitialSupply
   public setGradThreshold
   public setFee
   public setMaxTx
   public setAssetRate
   public setDeployParams
   public getUserTokens
   public launch
   public launchFor
   public sell
   public buy
   private _openTradingOnUniswap
   public unwrapToken
   
   Suggested order:
   external initialize
   public setInitialSupply
   public setGradThreshold
   public setFee
   public setMaxTx
   public setAssetRate
   public setDeployParams
   public getUserTokens
   public launch
   public launchFor
   public sell
   public buy
   public unwrapToken
   internal _createUserProfile
   internal _checkIfProfileExists
   internal _approval
   private _openTradingOnUniswap

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

1: 
   Current order:
   public name
   public symbol
   public decimals
   public totalSupply
   public balanceOf
   public transfer
   public allowance
   public approve
   public transferFrom
   private _approve
   private _transfer
   internal _updateMaxTx
   public updateMaxTx
   public excludeFromMaxTx
   internal _burn
   public burnFrom
   
   Suggested order:
   public name
   public symbol
   public decimals
   public totalSupply
   public balanceOf
   public transfer
   public allowance
   public approve
   public transferFrom
   public updateMaxTx
   public excludeFromMaxTx
   public burnFrom
   internal _updateMaxTx
   internal _burn
   private _approve
   private _transfer

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

1: 
   Current order:
   external initialize
   internal _createPair
   external createPair
   public getPair
   public allPairsLength
   public setTaxParams
   public setRouter
   
   Suggested order:
   external initialize
   external createPair
   public getPair
   public allPairsLength
   public setTaxParams
   public setRouter
   internal _createPair

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/genesis/FGenesis.sol

1: 
   Current order:
   external initialize
   external setParams
   internal _setParams
   external createGenesis
   internal _getGenesis
   external onGenesisSuccess
   external onGenesisFailed
   external withdrawLeftAssetsAfterFinalized
   external resetTime
   external cancelGenesis
   
   Suggested order:
   external initialize
   external setParams
   external createGenesis
   external onGenesisSuccess
   external onGenesisFailed
   external withdrawLeftAssetsAfterFinalized
   external resetTime
   external cancelGenesis
   internal _setParams
   internal _getGenesis

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

1: 
   Current order:
   internal _validateTime
   external initialize
   external participate
   external onGenesisSuccess
   external claimAgentToken
   external getClaimableAgentToken
   external onGenesisFailed
   public isEnded
   public isStarted
   external getParticipantCount
   external getParticipantsPaginated
   external getParticipantsInfo
   public getGenesisInfo
   external withdrawLeftAssetsAfterFinalized
   external resetTime
   external cancelGenesis
   
   Suggested order:
   external initialize
   external participate
   external onGenesisSuccess
   external claimAgentToken
   external getClaimableAgentToken
   external onGenesisFailed
   external getParticipantCount
   external getParticipantsPaginated
   external getParticipantsInfo
   external withdrawLeftAssetsAfterFinalized
   external resetTime
   external cancelGenesis
   public isEnded
   public isStarted
   public getGenesisInfo
   internal _validateTime

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

1: 
   Current order:
   public initialize
   external setMockAgentToken
   external setMockId
   public proposeAgent
   public withdraw
   public totalAgents
   public initFromBondingCurve
   public executeBondingCurveApplication
   
   Suggested order:
   external setMockAgentToken
   external setMockId
   public initialize
   public proposeAgent
   public withdraw
   public totalAgents
   public initFromBondingCurve
   public executeBondingCurveApplication

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

1: 
   Current order:
   public votingDelay
   public votingPeriod
   public proposalThreshold
   public propose
   internal _propose
   public quorum
   public earlyExecute
   public state
   public updateQuorum
   public supportsInterface
   
   Suggested order:
   public votingDelay
   public votingPeriod
   public proposalThreshold
   public propose
   public quorum
   public earlyExecute
   public state
   public updateQuorum
   public supportsInterface
   internal _propose

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/VirtualProtocolDAO.sol

1: 
   Current order:
   public votingDelay
   public votingPeriod
   public proposalThreshold
   public propose
   internal _propose
   public quorum
   public quorumDenominator
   
   Suggested order:
   public votingDelay
   public votingPeriod
   public proposalThreshold
   public propose
   public quorum
   public quorumDenominator
   internal _propose

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualProtocolDAO.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

1: 
   Current order:
   external oracleTransfer
   public approve
   public transfer
   public transferFrom
   internal _update
   public nonces
   
   Suggested order:
   external oracleTransfer
   public approve
   public transfer
   public transferFrom
   public nonces
   internal _update

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

1: 
   Current order:
   internal push
   private _insert
   internal latest
   internal upperLookupRecent
   private _upperBinaryLookup
   
   Suggested order:
   internal push
   internal latest
   internal upperLookupRecent
   private _insert
   private _upperBinaryLookup

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

1: 
   Current order:
   internal push
   private _insert
   internal latest
   internal upperLookupRecent
   private _upperBinaryLookup
   
   Suggested order:
   internal push
   internal latest
   internal upperLookupRecent
   private _insert
   private _upperBinaryLookup

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

1: 
   Current order:
   internal push
   private _insert
   internal latest
   internal upperLookupRecent
   private _upperBinaryLookup
   
   Suggested order:
   internal push
   internal latest
   internal upperLookupRecent
   private _insert
   private _upperBinaryLookup

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/tax/AgentTax.sol

1: 
   Current order:
   external initialize
   public updateSwapParams
   public updateSwapThresholds
   public updateTreasury
   external withdraw
   public handleAgentTaxes
   internal _getTaxRecipient
   internal _swapForAsset
   public updateCreator
   public dcaSell
   public updateTbaBonus
   
   Suggested order:
   external initialize
   external withdraw
   public updateSwapParams
   public updateSwapThresholds
   public updateTreasury
   public handleAgentTaxes
   public updateCreator
   public dcaSell
   public updateTbaBonus
   internal _getTaxRecipient
   internal _swapForAsset

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

1: 
   Current order:
   external initialize
   public updateSwapParams
   public updateSwapThresholds
   public updateTreasury
   external withdraw
   public swapForAsset
   
   Suggested order:
   external initialize
   external withdraw
   public updateSwapParams
   public updateSwapThresholds
   public updateTreasury
   public swapForAsset

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

1: 
   Current order:
   external initialize
   public votingDelay
   public votingPeriod
   public proposalThreshold
   public propose
   internal _propose
   public proposalCount
   public scoreOf
   external getPastScore
   internal _castVote
   internal _tryAutoExecute
   internal _updateMaturity
   internal _calcMaturity
   public getMaturity
   public quorum
   public quorumDenominator
   public state
   public totalScore
   
   Suggested order:
   external initialize
   external getPastScore
   public votingDelay
   public votingPeriod
   public proposalThreshold
   public propose
   public proposalCount
   public scoreOf
   public getMaturity
   public quorum
   public quorumDenominator
   public state
   public totalScore
   internal _propose
   internal _castVote
   internal _tryAutoExecute
   internal _updateMaturity
   internal _calcMaturity

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

1: 
   Current order:
   public initialize
   public getApplication
   public proposeAgent
   public withdraw
   public executeApplication
   internal _createNewDAO
   internal _createNewAgentToken
   internal _createNewAgentVeToken
   public totalAgents
   public setApplicationThreshold
   public setVault
   public setImplementations
   public setMaturityDuration
   public setUniswapRouter
   public setTokenAdmin
   public setTokenSupplyParams
   public setTokenTaxParams
   public setAssetToken
   public pause
   public unpause
   internal _msgSender
   internal _msgData
   
   Suggested order:
   public initialize
   public getApplication
   public proposeAgent
   public withdraw
   public executeApplication
   public totalAgents
   public setApplicationThreshold
   public setVault
   public setImplementations
   public setMaturityDuration
   public setUniswapRouter
   public setTokenAdmin
   public setTokenSupplyParams
   public setTokenTaxParams
   public setAssetToken
   public pause
   public unpause
   internal _createNewDAO
   internal _createNewAgentToken
   internal _createNewAgentVeToken
   internal _msgSender
   internal _msgData

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

1: 
   Current order:
   public initialize
   public getApplication
   public proposeAgent
   public withdraw
   internal _executeApplication
   public executeApplication
   internal _createNewDAO
   internal _createNewAgentToken
   internal _createNewAgentVeToken
   public totalAgents
   public setApplicationThreshold
   public setVault
   public setImplementations
   public setMaturityDuration
   public setUniswapRouter
   public setTokenAdmin
   public setTokenSupplyParams
   public setTokenTaxParams
   public setAssetToken
   public pause
   public unpause
   internal _msgSender
   internal _msgData
   public initFromBondingCurve
   public executeBondingCurveApplication
   public setDefaultDelegatee
   
   Suggested order:
   public initialize
   public getApplication
   public proposeAgent
   public withdraw
   public executeApplication
   public totalAgents
   public setApplicationThreshold
   public setVault
   public setImplementations
   public setMaturityDuration
   public setUniswapRouter
   public setTokenAdmin
   public setTokenSupplyParams
   public setTokenTaxParams
   public setAssetToken
   public pause
   public unpause
   public initFromBondingCurve
   public executeBondingCurveApplication
   public setDefaultDelegatee
   internal _executeApplication
   internal _createNewDAO
   internal _createNewAgentToken
   internal _createNewAgentVeToken
   internal _msgSender
   internal _msgData

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

1: 
   Current order:
   public initialize
   public getApplication
   public proposeAgent
   public withdraw
   internal _executeApplication
   public executeApplication
   internal _createNewDAO
   internal _createNewAgentToken
   internal _createNewAgentVeToken
   public totalAgents
   public setApplicationThreshold
   public setVault
   public setImplementations
   public setMaturityDuration
   public setUniswapRouter
   public setTokenAdmin
   public setTokenSupplyParams
   public setTokenTaxParams
   public setAssetToken
   public pause
   public unpause
   internal _msgSender
   internal _msgData
   public setDefaultDelegatee
   public initFromToken
   public executeTokenApplication
   public isCompatibleToken
   internal _createPair
   
   Suggested order:
   public initialize
   public getApplication
   public proposeAgent
   public withdraw
   public executeApplication
   public totalAgents
   public setApplicationThreshold
   public setVault
   public setImplementations
   public setMaturityDuration
   public setUniswapRouter
   public setTokenAdmin
   public setTokenSupplyParams
   public setTokenTaxParams
   public setAssetToken
   public pause
   public unpause
   public setDefaultDelegatee
   public initFromToken
   public executeTokenApplication
   public isCompatibleToken
   internal _executeApplication
   internal _createNewDAO
   internal _createNewAgentToken
   internal _createNewAgentVeToken
   internal _msgSender
   internal _msgData
   internal _createPair

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

1: 
   Current order:
   external setInitParams
   public setTokenSupplyParams
   public setTokenTaxParams
   external setImplementations
   external migrateAgent
   internal _createNewDAO
   internal _createNewAgentVeToken
   internal _createNewAgentToken
   external pause
   external unpause
   external reset
   
   Suggested order:
   external setInitParams
   external setImplementations
   external migrateAgent
   external pause
   external unpause
   external reset
   public setTokenSupplyParams
   public setTokenTaxParams
   internal _createNewDAO
   internal _createNewAgentVeToken
   internal _createNewAgentToken

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

1: 
   Current order:
   public initialize
   external setContributionService
   public nextVirtualId
   external mint
   public addCoreType
   public virtualInfo
   public virtualLP
   external stakingTokenToVirtualId
   public addValidator
   internal _validatorScoreOf
   internal _getPastValidatorScore
   public totalProposals
   external setCoreTypes
   public setTokenURI
   external setTBA
   public setDAO
   public totalStaked
   public getVotes
   public getContributionNft
   public getServiceNft
   public getAllServices
   public tokenURI
   public supportsInterface
   public totalSupply
   public isBlacklisted
   public setBlacklist
   public migrateScoreFunctions
   public setEloCalculator
   public getEloCalculator
   public migrateVirtual
   
   Suggested order:
   external setContributionService
   external mint
   external stakingTokenToVirtualId
   external setCoreTypes
   external setTBA
   public initialize
   public nextVirtualId
   public addCoreType
   public virtualInfo
   public virtualLP
   public addValidator
   public totalProposals
   public setTokenURI
   public setDAO
   public totalStaked
   public getVotes
   public getContributionNft
   public getServiceNft
   public getAllServices
   public tokenURI
   public supportsInterface
   public totalSupply
   public isBlacklisted
   public setBlacklist
   public migrateScoreFunctions
   public setEloCalculator
   public getEloCalculator
   public migrateVirtual
   internal _validatorScoreOf
   internal _getPastValidatorScore

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

1: 
   Current order:
   external initialize
   internal _decodeBaseParams
   internal _processSupplyParams
   internal _processTaxParams
   internal _mintBalances
   internal _createPair
   external addInitialLiquidity
   internal _addInitialLiquidity
   public isLiquidityPool
   external liquidityPools
   public addLiquidityPool
   external removeLiquidityPool
   public isValidCaller
   external validCallers
   external addValidCaller
   external removeValidCaller
   external setProjectTaxRecipient
   external setSwapThresholdBasisPoints
   external setProjectTaxRates
   public name
   public symbol
   public decimals
   public totalSupply
   public totalBuyTaxBasisPoints
   public totalSellTaxBasisPoints
   public balanceOf
   public transfer
   public allowance
   public approve
   public transferFrom
   public increaseAllowance
   public decreaseAllowance
   internal _transfer
   internal _pretaxValidationAndLimits
   internal _taxProcessing
   internal _autoSwap
   internal _eligibleForSwap
   internal _swapTax
   external distributeTaxTokens
   external withdrawETH
   external withdrawERC20
   internal _mint
   internal _burn
   internal _approve
   internal _spendAllowance
   public burn
   public burnFrom
   internal _beforeTokenTransfer
   internal _afterTokenTransfer
   
   Suggested order:
   external initialize
   external addInitialLiquidity
   external liquidityPools
   external removeLiquidityPool
   external validCallers
   external addValidCaller
   external removeValidCaller
   external setProjectTaxRecipient
   external setSwapThresholdBasisPoints
   external setProjectTaxRates
   external distributeTaxTokens
   external withdrawETH
   external withdrawERC20
   public isLiquidityPool
   public addLiquidityPool
   public isValidCaller
   public name
   public symbol
   public decimals
   public totalSupply
   public totalBuyTaxBasisPoints
   public totalSellTaxBasisPoints
   public balanceOf
   public transfer
   public allowance
   public approve
   public transferFrom
   public increaseAllowance
   public decreaseAllowance
   public burn
   public burnFrom
   internal _decodeBaseParams
   internal _processSupplyParams
   internal _processTaxParams
   internal _mintBalances
   internal _createPair
   internal _addInitialLiquidity
   internal _transfer
   internal _pretaxValidationAndLimits
   internal _taxProcessing
   internal _autoSwap
   internal _eligibleForSwap
   internal _swapTax
   internal _mint
   internal _burn
   internal _approve
   internal _spendAllowance
   internal _beforeTokenTransfer
   internal _afterTokenTransfer

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

1: 
   Current order:
   external initialize
   public stake
   public setCanStake
   public setMatureAt
   public withdraw
   public getPastBalanceOf
   public transfer
   public transferFrom
   public approve
   internal _update
   public getPastDelegates
   
   Suggested order:
   external initialize
   public stake
   public setCanStake
   public setMatureAt
   public withdraw
   public getPastBalanceOf
   public transfer
   public transferFrom
   public approve
   public getPastDelegates
   internal _update

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

1: 
   Current order:
   public initialize
   internal mapBattleResultToGameResult
   internal _roundUp
   public battleElo
   public setK
   
   Suggested order:
   public initialize
   public battleElo
   public setK
   internal mapBattleResultToGameResult
   internal _roundUp

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

1: 
   Current order:
   internal __GovernorCountingSimple_init
   public COUNTING_MODE
   public hasVoted
   public proposalVotes
   internal _quorumReached
   internal _voteSucceeded
   internal _countVote
   
   Suggested order:
   public COUNTING_MODE
   public hasVoted
   public proposalVotes
   internal __GovernorCountingSimple_init
   internal _quorumReached
   internal _voteSucceeded
   internal _countVote

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

1: 
   Current order:
   internal __ValidatorRegistry_init
   public isValidator
   internal _addValidator
   internal _initValidatorScore
   public validatorScore
   public getPastValidatorScore
   public validatorCount
   public validatorAt
   public totalUptimeScore
   internal _migrateScoreFunctions
   
   Suggested order:
   public isValidator
   public validatorScore
   public getPastValidatorScore
   public validatorCount
   public validatorAt
   public totalUptimeScore
   internal __ValidatorRegistry_init
   internal _addValidator
   internal _initValidatorScore
   internal _migrateScoreFunctions

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="NC-15"></a>[NC-15] Functions should not be longer than 50 lines

Overly complex code can make understanding functionality more difficult, try to further modularize your code to ensure readability

*Instances (451)*:

```solidity
File: contracts/AgentInference.sol

24:     function initialize(address defaultAdmin_, address token_, address agentNft_) external initializer {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

42:     function initialize(address thePersonaAddress) public initializer {

51:     function tokenVirtualId(uint256 tokenId) public view returns (uint256) {

55:     function getAgentDAO(uint256 virtualId) public view returns (IGovernor) {

59:     function isAccepted(uint256 tokenId) public view returns (bool) {

99:     function getAdmin() public view override returns (address) {

116:     function getChildren(uint256 tokenId) public view returns (uint256[] memory) {

120:     function getParentId(uint256 tokenId) public view returns (uint256) {

124:     function getCore(uint256 tokenId) public view returns (uint8) {

149:     function isModel(uint256 tokenId) public view returns (bool) {

153:     function ownerOf(uint256 tokenId) public view override(IERC721, ERC721Upgradeable) returns (address) {

157:     function getDatasetId(uint256 tokenId) external view returns (uint256) {

161:     function getEloCalculator() external view returns (address) {

165:     function setEloCalculator(address eloCalculator_) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/IContributionNft.sol

7:     function tokenVirtualId(uint256 tokenId) external view returns (uint256);

8:     function tokenURI(uint256 tokenId) external view returns (string memory);

9:     function getChildren(uint256 tokenId) external view returns (uint256[] memory);

10:     function getParentId(uint256 tokenId) external view returns (uint256);

11:     function getCore(uint256 tokenId) external view returns (uint8);

12:     function isModel(uint256 tokenId) external view returns (bool);

13:     function getAdmin() external view returns (address);

14:     function getDatasetId(uint256 tokenId) external view returns (uint256);

15:     function getAgentDAO(uint256 virtualId) external view returns (IGovernor);

16:     function getEloCalculator() external view returns (address);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/IContributionNft.sol)

```solidity
File: contracts/contribution/IServiceNft.sol

5:     function getCore(uint256 tokenId) external view returns (uint8);

7:     function getMaturity(uint256 tokenId) external view returns (uint256);

9:     function getImpact(uint256 tokenId) external view returns (uint256);

11:     function getCoreService(uint256 virtualId, uint8 coreType) external view returns (uint256);

13:     function getCoreDatasetAt(uint256 virtualId, uint8 coreType, uint256 index) external view returns (uint256);

15:     function totalCoreDatasets(uint256 virtualId, uint8 coreType) external view returns (uint256);

17:     function getCoreDatasets(uint256 virtualId, uint8 coreType) external view returns (uint256[] memory);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/IServiceNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

58:     function mint(uint256 virtualId, bytes32 descHash) public returns (uint256) {

90:     function updateImpact(uint256 virtualId, uint256 proposalId) public {

110:     function getCore(uint256 tokenId) public view returns (uint8) {

115:     function getMaturity(uint256 tokenId) public view returns (uint256) {

120:     function getImpact(uint256 tokenId) public view returns (uint256) {

125:     function getCoreService(uint256 virtualId, uint8 coreType) public view returns (uint256) {

129:     function getCoreDatasetAt(uint256 virtualId, uint8 coreType, uint256 index) public view returns (uint256) {

133:     function totalCoreDatasets(uint256 virtualId, uint8 coreType) public view returns (uint256) {

137:     function getCoreDatasets(uint256 virtualId, uint8 coreType) public view returns (uint256[] memory) {

141:     function setDatasetImpactWeight(uint16 weight) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

121:     function _createUserProfile(address _user) internal returns (bool) {

133:     function _checkIfProfileExists(address _user) internal view returns (bool) {

137:     function _approval(address _spender, address _token, uint256 amount) internal returns (bool) {

143:     function setInitialSupply(uint256 newSupply) public onlyOwner {

147:     function setGradThreshold(uint256 newThreshold) public onlyOwner {

151:     function setFee(uint256 newFee, address newFeeTo) public onlyOwner {

156:     function setMaxTx(uint256 maxTx_) public onlyOwner {

160:     function setAssetRate(uint256 newRate) public onlyOwner {

166:     function setDeployParams(DeployParams memory params) public onlyOwner {

170:     function getUserTokens(address account) public view returns (address[] memory) {

281:     function sell(uint256 amountIn, address tokenAddress) public returns (bool) {

318:     function buy(uint256 amountIn, address tokenAddress) public payable returns (bool) {

357:     function _openTradingOnUniswap(address tokenAddress) private {

405:     function unwrapToken(address srcTokenAddress, address[] memory accounts) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

46:     function name() public view returns (string memory) {

50:     function symbol() public view returns (string memory) {

58:     function totalSupply() public view override returns (uint256) {

62:     function balanceOf(address account) public view override returns (uint256) {

66:     function transfer(address recipient, uint256 amount) public override returns (bool) {

72:     function allowance(address owner, address spender) public view override returns (uint256) {

76:     function approve(address spender, uint256 amount) public override returns (bool) {

82:     function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {

90:     function _approve(address owner, address spender, uint256 amount) private {

99:     function _transfer(address from, address to, uint256 amount) private {

121:     function updateMaxTx(uint256 _maxTx) public onlyOwner {

125:     function excludeFromMaxTx(address user) public onlyOwner {

131:     function _burn(address user, uint256 amount) internal {

136:     function burnFrom(address user, uint256 amount) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

31:     function initialize(address taxVault_, uint256 buyTax_, uint256 sellTax_) external initializer {

41:     function _createPair(address tokenA, address tokenB) internal returns (address) {

60:     function createPair(address tokenA, address tokenB) external onlyRole(CREATOR_ROLE) nonReentrant returns (address) {

66:     function getPair(address tokenA, address tokenB) public view returns (address) {

70:     function allPairsLength() public view returns (uint) {

74:     function setTaxParams(address newVault_, uint256 buyTax_, uint256 sellTax_) public onlyRole(ADMIN_ROLE) {

82:     function setRouter(address router_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

45:     function mint(uint256 reserve0, uint256 reserve1) public onlyRouter returns (bool) {

71:     function approval(address _user, address _token, uint256 amount) public onlyRouter returns (bool) {

82:     function transferAsset(address recipient, uint256 amount) public onlyRouter {

88:     function transferTo(address recipient, uint256 amount) public onlyRouter {

94:     function getReserves() public view returns (uint256, uint256) {

102:     function priceALast() public view returns (uint256) {

106:     function priceBLast() public view returns (uint256) {

110:     function balance() public view returns (uint256) {

114:     function assetBalance() public view returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

29:     function initialize(address factory_, address assetToken_) external initializer {

165:     function graduate(address tokenAddress) public onlyRole(EXECUTOR_ROLE) nonReentrant {

183:     function setTaxManager(address newManager) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/fun/IFPair.sol

5:     function getReserves() external view returns (uint256, uint256);

7:     function assetBalance() external view returns (uint256);

9:     function balance() external view returns (uint256);

11:     function mint(uint256 reserve0, uint256 reserve1) external returns (bool);

13:     function transferAsset(address recipient, uint256 amount) external;

15:     function transferTo(address recipient, uint256 amount) external;

17:     function swap(uint256 amount0In, uint256 amount0Out, uint256 amount1In, uint256 amount1Out) external returns (bool);

21:     function approval(address _user, address _token, uint256 amount) external returns (bool);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/IFPair.sol)

```solidity
File: contracts/genesis/FGenesis.sol

46:     function initialize(Params memory p) external initializer {

53:     function setParams(Params calldata p) external onlyRole(ADMIN_ROLE) {

76:     function createGenesis(GenesisCreationParams memory gParams) external returns (address) {

110:     function _getGenesis(uint256 id) internal view returns (Genesis) {

130:     function onGenesisFailed(uint256 id, uint256[] calldata participantIndexes) external onlyRole(OPERATION_ROLE) {

143:     function resetTime(uint256 id, uint256 newStartTime, uint256 newEndTime) external onlyRole(OPERATION_ROLE) {

147:     function cancelGenesis(uint256 id) external onlyRole(OPERATION_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

129:     function _validateTime(uint256 _startTime, uint256 _endTime) internal view {

134:     function initialize(GenesisInitParams calldata params) external initializer {

177:     function participate(uint256 pointAmt, uint256 virtualsAmt) external nonReentrant whenActive {

307:     function claimAgentToken(address userAddress) external nonReentrant {

320:     function getClaimableAgentToken(address userAddress) external view returns (uint256) {

357:     function getParticipantCount() external view returns (uint256) {

361:     function getParticipantsPaginated(uint256 startIndex, uint256 pageSize) external view returns (address[] memory) {

422:     function getGenesisInfo() public view returns (GenesisInfo memory) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

39:     function setMockAgentToken(address token) external {

65:     function totalAgents() public pure returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/governance/GovernorCountingSimple.sol

35:     function COUNTING_MODE() public pure virtual override returns (string memory) {

42:     function hasVoted(uint256 proposalId, address account) public view virtual override returns (bool) {

59:     function _quorumReached(uint256 proposalId) internal view virtual override returns (bool) {

68:     function _voteSucceeded(uint256 proposalId) internal view virtual override returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/GovernorCountingSimple.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

49:     function votingDelay() public view override(Governor, GovernorSettings) returns (uint256) {

53:     function votingPeriod() public view override(Governor, GovernorSettings) returns (uint256) {

57:     function proposalThreshold() public view override(Governor, GovernorSettings) returns (uint256) {

80:     function quorum(uint256 blockNumber) public view override returns (uint256) {

95:     function earlyExecute(uint256 proposalId) public payable onlyRole(EXECUTOR_ROLE) returns (uint256) {

121:     function state(uint256 proposalId) public view override returns (ProposalState) {

128:     function updateQuorum(uint224 newQuorum) public onlyGovernance {

134:     function supportsInterface(bytes4 interfaceId) public view override(Governor, AccessControl) returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/VirtualProtocolDAO.sol

34:     function votingDelay() public view override(Governor, GovernorSettings) returns (uint256) {

38:     function votingPeriod() public view override(Governor, GovernorSettings) returns (uint256) {

42:     function proposalThreshold() public view override(Governor, GovernorSettings) returns (uint256) {

65:     function quorum(uint256 blockNumber) public view override(Governor, GovernorVotesQuorumFraction) returns (uint256) {

69:     function quorumDenominator() public pure override returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualProtocolDAO.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

29:     function approve(address /*spender*/, uint256 /*value*/) public override returns (bool) {

33:     function transfer(address /*to*/, uint256 /*value*/) public override returns (bool) {

37:     function transferFrom(address /*from*/, address /*to*/, uint256 /*value*/) public override returns (bool) {

43:     function _update(address from, address to, uint256 value) internal override(ERC20, ERC20Votes) {

47:     function nonces(address owner) public view override(ERC20Permit, Nonces) returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

19:     function push(Trace storage self, uint48 key, address value) internal {

23:     function _insert(Checkpoint[] storage self, uint48 key, address value) private {

43:     function latest(Trace storage self) internal view returns (address) {

48:     function upperLookupRecent(Trace storage self, uint48 key) internal view returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/Elo.sol

9:     function sixteenthRoot(uint256 x) internal pure returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

16:     function mulWadDown(uint256 x, uint256 y) internal pure returns (uint256) {

20:     function mulWadUp(uint256 x, uint256 y) internal pure returns (uint256) {

24:     function divWadDown(uint256 x, uint256 y) internal pure returns (uint256) {

28:     function divWadUp(uint256 x, uint256 y) internal pure returns (uint256) {

36:     function mulDivDown(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 z) {

49:     function mulDivUp(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 z) {

63:     function rpow(uint256 x, uint256 n, uint256 scalar) internal pure returns (uint256 z) {

152:     function sqrt(uint256 x) internal pure returns (uint256 z) {

217:     function unsafeMod(uint256 x, uint256 y) internal pure returns (uint256 z) {

226:     function unsafeDiv(uint256 x, uint256 y) internal pure returns (uint256 r) {

235:     function unsafeDivUp(uint256 x, uint256 y) internal pure returns (uint256 z) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

27:     function push(Trace storage self, uint32 key, RewardSettings memory value) internal {

31:     function _insert(Checkpoint[] storage self, uint32 key, RewardSettings memory value) private {

51:     function latest(Trace storage self) internal view returns (RewardSettings memory) {

56:     function upperLookupRecent(Trace storage self, uint32 key) internal view returns (RewardSettings memory) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

24:     function push(Trace storage self, uint32 key, RewardSettings memory value) internal {

28:     function _insert(Checkpoint[] storage self, uint32 key, RewardSettings memory value) private {

48:     function latest(Trace storage self) internal view returns (RewardSettings memory) {

53:     function upperLookupRecent(Trace storage self, uint32 key) internal view returns (RewardSettings memory) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/libs/TokenSaver.sol

20:     function saveToken(address _token, address _receiver, uint256 _amount) external onlyTokenSaver {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

24:     function getAmountsOut(uint256 amountIn, Route[] memory routes) external view returns (uint256[] memory amounts);

58:     function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/pool/IRouter.sol

13:     function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IRouter.sol)

```solidity
File: contracts/pool/IUniswapV2Factory.sol

7:     function feeToSetter() external view returns (address);

9:     function getPair(address tokenA, address tokenB) external view returns (address pair);

10:     function allPairs(uint) external view returns (address pair);

11:     function allPairsLength() external view returns (uint);

13:     function createPair(address tokenA, address tokenB) external returns (address pair);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Factory.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

7:     function name() external pure returns (string memory);

8:     function symbol() external pure returns (string memory);

9:     function decimals() external pure returns (uint8);

10:     function totalSupply() external view returns (uint);

11:     function balanceOf(address owner) external view returns (uint);

12:     function allowance(address owner, address spender) external view returns (uint);

14:     function approve(address spender, uint value) external returns (bool);

15:     function transfer(address to, uint value) external returns (bool);

16:     function transferFrom(address from, address to, uint value) external returns (bool);

18:     function DOMAIN_SEPARATOR() external view returns (bytes32);

19:     function PERMIT_TYPEHASH() external pure returns (bytes32);

20:     function nonces(address owner) external view returns (uint);

22:     function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

36:     function MINIMUM_LIQUIDITY() external pure returns (uint);

37:     function factory() external view returns (address);

38:     function token0() external view returns (address);

39:     function token1() external view returns (address);

40:     function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

41:     function price0CumulativeLast() external view returns (uint);

42:     function price1CumulativeLast() external view returns (uint);

45:     function mint(address to) external returns (uint liquidity);

46:     function burn(address to) external returns (uint amount0, uint amount1);

47:     function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/pool/IUniswapV2Router01.sol

4:     function factory() external pure returns (address);

108:     function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);

109:     function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);

110:     function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);

111:     function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);

112:     function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router01.sol)

```solidity
File: contracts/pool/IUniswapV2Router02.sol

6:     function removeLiquidityETHSupportingFeeOnTransferTokens(

14:     function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(

27:     function swapExactTokensForTokensSupportingFeeOnTransferTokens(

34:     function swapExactETHForTokensSupportingFeeOnTransferTokens(

40:     function swapExactTokensForETHSupportingFeeOnTransferTokens(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router02.sol)

```solidity
File: contracts/tax/AgentTax.sol

147:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

157:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

164:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

190:     function _getTaxRecipient(uint256 agentId) internal returns (TaxRecipient memory) {

200:     function _swapForAsset(uint256 agentId, uint256 minOutput, uint256 maxOverride) internal returns (bool, uint256) {

255:     function updateCreator(uint256 agentId, address creator) public {

269:     function dcaSell(uint256[] memory agentIds, uint256 slippage, uint256 maxOverride) public onlyRole(EXECUTOR_ROLE) {

287:     function updateTbaBonus(address tbaBonus_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

101:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

111:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

118:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

122:     function swapForAsset() public onlyBondingRouter returns (bool, uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/IBondingTax.sol

5:     function swapForAsset() external returns (bool, uint256);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/IBondingTax.sol)

```solidity
File: contracts/tax/ITBABonus.sol

5:     function distributeBonus(uint256 agentId, address recipient, uint256 amount) external;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/ITBABonus.sol)

```solidity
File: contracts/tax/LPRefund.sol

27:     function initialize(address defaultAdmin_, address taxToken_) external initializer {

36:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

62:     function manualRefund(bytes32 txhash, address recipient, uint256 amount) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

33:     function initialize(address defaultAdmin_, address assetToken_) external initializer {

44:     function updateBonusRate(uint16 bonusRate_) public onlyRole(ADMIN_ROLE) {

50:     function setAllowances(uint256[] memory agentIds, uint256[] memory allowances) public onlyRole(ADMIN_ROLE) {

64:     function distributeBonus(uint256 agentId, address recipient, uint256 amount) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/token/Virtual.sol

17:     function mint(address _to, uint256 _amount) external onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

59:     function votingDelay() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256) {

63:     function votingPeriod() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256) {

108:     function proposalCount() public view override(IAgentDAO, GovernorStorageUpgradeable) returns (uint256) {

112:     function scoreOf(address account) public view returns (uint256) {

116:     function getPastScore(address account, uint256 timepoint) external view returns (uint256) {

151:     function _tryAutoExecute(uint256 proposalId) internal {

158:     function _updateMaturity(address account, uint256 proposalId, uint256 weight, bytes memory params) internal {

179:     function _calcMaturity(uint256 proposalId, uint8[] memory votes) internal view returns (uint256) {

195:     function getMaturity(uint256 proposalId) public view returns (uint256) {

206:     function quorumDenominator() public pure override returns (uint256) {

210:     function state(uint256 proposalId) public view override(GovernorUpgradeable) returns (ProposalState) {

222:     function totalScore() public view override returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

132:     function getApplication(uint256 proposalId) public view returns (Application memory) {

179:     function withdraw(uint256 id) public noReentrant {

196:     function executeApplication(uint256 id, bool canStake) public noReentrant {

288:     function _createNewAgentToken(string memory name, string memory symbol) internal returns (address instance) {

323:     function totalAgents() public view returns (uint256) {

327:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

332:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

336:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

342:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

396:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

400:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

404:     function _msgData() internal view override(Context, ContextUpgradeable) returns (bytes calldata) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

135:     function getApplication(uint256 proposalId) public view returns (Application memory) {

182:     function withdraw(uint256 id) public noReentrant {

199:     function _executeApplication(uint256 id, bool canStake, bytes memory tokenSupplyParams_) internal {

268:     function executeApplication(uint256 id, bool canStake) public noReentrant {

337:     function totalAgents() public view returns (uint256) {

341:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

356:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

360:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

364:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

402:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

406:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

410:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

414:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

418:     function _msgData() internal view override(Context, ContextUpgradeable) returns (bytes calldata) {

489:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

133:     function getApplication(uint256 proposalId) public view returns (Application memory) {

180:     function withdraw(uint256 id) public noReentrant {

205:     function _executeApplication(uint256 id, bool canStake, bytes memory tokenSupplyParams_) internal {

292:     function executeApplication(uint256 id, bool canStake) public noReentrant {

361:     function totalAgents() public view returns (uint256) {

365:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

370:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

374:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

380:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

384:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

426:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

430:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

434:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

438:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

442:     function _msgData() internal view override(Context, ContextUpgradeable) returns (bytes calldata) {

446:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

505:     function executeTokenApplication(uint256 id, bool canStake) public noReentrant {

523:     function isCompatibleToken(address tokenAddr) public view returns (bool) {

543:     function _createPair(address tokenAddr) internal returns (address uniswapV2Pair_) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

94:     function setImplementations(address token, address veToken, address dao) external onlyOwner {

100:     function migrateAgent(uint256 id, string memory name, string memory symbol, bool canStake) external noReentrant {

171:     function _createNewAgentToken(string memory name, string memory symbol) internal returns (address instance) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

61:     function initialize(address defaultAdmin) public initializer {

81:     function nextVirtualId() public view returns (uint256) {

116:     function addCoreType(string memory label) public onlyRole(DEFAULT_ADMIN_ROLE) {

120:     function virtualInfo(uint256 virtualId) public view returns (VirtualInfo memory) {

124:     function virtualLP(uint256 virtualId) public view returns (VirtualLP memory) {

129:     function stakingTokenToVirtualId(address stakingToken) external view returns (uint256) {

133:     function addValidator(uint256 virtualId, address validator) public {

141:     function _validatorScoreOf(uint256 virtualId, address account) internal view returns (uint256) {

157:     function totalProposals(uint256 virtualId) public view returns (uint256) {

163:     function setCoreTypes(uint256 virtualId, uint8[] memory coreTypes) external onlyVirtualDAO(virtualId) {

169:     function setTokenURI(uint256 virtualId, string memory newTokenURI) public onlyVirtualDAO(virtualId) {

173:     function setTBA(uint256 virtualId, address tba) external onlyRole(MINTER_ROLE) {

179:     function setDAO(uint256 virtualId, address newDAO) public {

185:     function totalStaked(uint256 virtualId) public view returns (uint256) {

189:     function getVotes(uint256 virtualId, address validator) public view returns (uint256) {

193:     function getContributionNft() public view returns (address) {

197:     function getServiceNft() public view returns (address) {

201:     function getAllServices(uint256 virtualId) public view returns (uint256[] memory) {

226:     function totalSupply() public view returns (uint256) {

230:     function isBlacklisted(uint256 virtualId) public view returns (bool) {

234:     function setBlacklist(uint256 virtualId, bool value) public onlyRole(ADMIN_ROLE) {

239:     function migrateScoreFunctions() public onlyRole(ADMIN_ROLE) {

240:         _migrateScoreFunctions(_validatorScoreOf, totalProposals, _getPastValidatorScore);

243:     function setEloCalculator(address eloCalculator) public onlyRole(ADMIN_ROLE) {

247:     function getEloCalculator() public view returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

120:     function _decodeBaseParams(address projectOwner_, bytes memory encodedBaseParams_) internal {

133:     function _processSupplyParams(ERC20SupplyParameters memory erc20SupplyParameters_) internal {

154:     function _processTaxParams(ERC20TaxParameters memory erc20TaxParameters_) internal returns (bool tokenHasTax_) {

176:     function _mintBalances(uint256 lpMint_, uint256 vaultMint_) internal {

193:     function _createPair() internal returns (address uniswapV2Pair_) {

214:     function addInitialLiquidity(address lpOwner) external onlyOwnerOrFactory {

225:     function _addInitialLiquidity(address lpOwner) internal {

273:     function isLiquidityPool(address queryAddress_) public view returns (bool) {

289:     function liquidityPools() external view returns (address[] memory liquidityPools_) {

300:     function addLiquidityPool(address newLiquidityPool_) public onlyOwnerOrFactory {

321:     function removeLiquidityPool(address removedLiquidityPool_) external onlyOwnerOrFactory {

335:     function isValidCaller(bytes32 queryHash_) public view returns (bool) {

346:     function validCallers() external view returns (bytes32[] memory validCallerHashes_) {

357:     function addValidCaller(bytes32 newValidCallerHash_) external onlyOwnerOrFactory {

369:     function removeValidCaller(bytes32 removedValidCallerHash_) external onlyOwnerOrFactory {

382:     function setProjectTaxRecipient(address projectTaxRecipient_) external onlyOwnerOrFactory {

394:     function setSwapThresholdBasisPoints(uint16 swapThresholdBasisPoints_) external onlyOwnerOrFactory {

429:     function name() public view virtual override returns (string memory) {

437:     function symbol() public view virtual override returns (string memory) {

454:     function decimals() public view virtual override returns (uint8) {

461:     function totalSupply() public view virtual override returns (uint256) {

470:     function totalBuyTaxBasisPoints() public view returns (uint256) {

479:     function totalSellTaxBasisPoints() public view returns (uint256) {

486:     function balanceOf(address account) public view virtual override returns (uint256) {

498:     function transfer(address to, uint256 amount) public virtual override(IERC20) returns (bool) {

507:     function allowance(address owner, address spender) public view virtual override returns (uint256) {

521:     function approve(address spender, uint256 amount) public virtual override returns (bool) {

543:     function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {

562:     function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {

582:     function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {

609:     function _transfer(address from, address to, uint256 amount, bool applyTax) internal virtual {

731:     function _autoSwap(address from_, address to_) internal {

784:     function _swapTax(uint256 swapBalance_, uint256 contractBalance_) internal {

868:     function withdrawETH(uint256 amount_) external onlyOwnerOrFactory {

891:     function withdrawERC20(address token_, uint256 amount_) external onlyOwnerOrFactory {

907:     function _mint(address account, uint256 amount) internal virtual {

935:     function _burn(address account, uint256 amount) internal virtual {

971:     function _approve(address owner, address spender, uint256 amount) internal virtual {

992:     function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {

1025:     function burnFrom(address account, uint256 value) public virtual {

1044:     function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}

1060:     function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

59:     function stake(uint256 amount, address receiver, address delegatee) public {

95:     function withdraw(uint256 amount) public noReentrant {

109:     function getPastBalanceOf(address account, uint256 timepoint) public view returns (uint256) {

118:     function transfer(address /*to*/, uint256 /*value*/) public override returns (bool) {

122:     function transferFrom(address /*from*/, address /*to*/, uint256 /*value*/) public override returns (bool) {

126:     function approve(address /*spender*/, uint256 /*value*/) public override returns (bool) {

139:     function getPastDelegates(address account, uint256 timepoint) public view returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

12:     function __CoreRegistry_init() internal onlyInitializing {

19:     function _addCoreType(string memory label) internal {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/ERC20Votes.sol

13:     function _delegate(address account, address delegatee) internal override {

18:     function _getPastDelegates(address account, uint256 timepoint) internal view virtual returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ERC20Votes.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

18:     function initialize(address initialOwner) public initializer {

23:     function mapBattleResultToGameResult(uint8 result) internal pure returns (uint256) {

34:     function _roundUp(uint256 numerator, uint256 denominator) internal pure returns (uint256) {

39:     function battleElo(uint256 currentRating, uint8[] memory battles) public view returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

31:     function __GovernorCountingSimple_init() internal onlyInitializing {}

37:     function COUNTING_MODE() public pure virtual override returns (string memory) {

44:     function hasVoted(uint256 proposalId, address account) public view virtual override returns (bool) {

61:     function _quorumReached(uint256 proposalId) internal view virtual override returns (bool) {

70:     function _voteSucceeded(uint256 proposalId) internal view virtual override returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

```solidity
File: contracts/virtualPersona/IAgentDAO.sol

16:     function proposalCount() external view returns (uint256);

18:     function scoreOf(address account) external view returns (uint256);

20:     function totalScore() external view returns (uint256);

22:     function getPastScore(address account, uint256 timepoint) external view returns (uint256);

24:     function getMaturity(uint256 proposalId) external view returns (uint256);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentDAO.sol)

```solidity
File: contracts/virtualPersona/IAgentFactory.sol

20:     function totalAgents() external view returns (uint256);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentFactory.sol)

```solidity
File: contracts/virtualPersona/IAgentFactoryV3.sol

20:     function totalAgents() external view returns (uint256);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/IAgentFactoryV4.sol

20:     function totalAgents() external view returns (uint256);

32:     function executeTokenApplication(uint256 id, bool canStake) external;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/IAgentNft.sol

33:     function stakingTokenToVirtualId(address daoToken) external view returns (uint256);

35:     function setTBA(uint256 virtualId, address tba) external;

37:     function virtualInfo(uint256 virtualId) external view returns (VirtualInfo memory);

39:     function virtualLP(uint256 virtualId) external view returns (VirtualLP memory);

41:     function totalSupply() external view returns (uint256);

43:     function totalStaked(uint256 virtualId) external view returns (uint256);

45:     function getVotes(uint256 virtualId, address validator) external view returns (uint256);

47:     function totalProposals(uint256 virtualId) external view returns (uint256);

49:     function getContributionNft() external view returns (address);

51:     function getServiceNft() external view returns (address);

53:     function getAllServices(uint256 virtualId) external view returns (uint256[] memory);

55:     function nextVirtualId() external view returns (uint256);

57:     function isBlacklisted(uint256 virtualId) external view returns (bool);

59:     function getEloCalculator() external view returns (address);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentNft.sol)

```solidity
File: contracts/virtualPersona/IAgentToken.sol

51:     function addInitialLiquidity(address lpOwner) external;

61:     function isLiquidityPool(address queryAddress_) external view returns (bool);

70:     function liquidityPools() external view returns (address[] memory liquidityPools_);

79:     function addLiquidityPool(address newLiquidityPool_) external;

88:     function removeLiquidityPool(address removedLiquidityPool_) external;

98:     function isValidCaller(bytes32 queryHash_) external view returns (bool);

107:     function validCallers() external view returns (bytes32[] memory validCallerHashes_);

116:     function addValidCaller(bytes32 newValidCallerHash_) external;

125:     function removeValidCaller(bytes32 removedValidCallerHash_) external;

134:     function setProjectTaxRecipient(address projectTaxRecipient_) external;

143:     function setSwapThresholdBasisPoints(uint16 swapThresholdBasisPoints_) external;

153:     function setProjectTaxRates(uint16 newProjectBuyTaxBasisPoints_, uint16 newProjectSellTaxBasisPoints_) external;

160:     function totalBuyTaxBasisPoints() external view returns (uint256);

167:     function totalSellTaxBasisPoints() external view returns (uint256);

223:     function withdrawERC20(address token_, uint256 amount_) external;

243:     function burnFrom(address account, uint256 value) external;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentToken.sol)

```solidity
File: contracts/virtualPersona/IAgentVeToken.sol

15:     function stake(uint256 amount, address receiver, address delegatee) external;

19:     function getPastDelegates(address account, uint256 timepoint) external view returns (address);

21:     function getPastBalanceOf(address account, uint256 timepoint) external view returns (uint256);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentVeToken.sol)

```solidity
File: contracts/virtualPersona/IEloCalculator.sol

5:     function battleElo(uint256 currentRating, uint8[] memory battles) external view returns (uint256);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IEloCalculator.sol)

```solidity
File: contracts/virtualPersona/IErrors.sol

55:     error CallerIsNotFactory(); //                            The caller of this function must match the factory address in storage.

57:     error CallerIsNotFactoryOrProjectOwner(); //              The caller of this function must match the factory address OR project owner address.

59:     error CallerIsNotFactoryProjectOwnerOrPool(); //          The caller of this function must match the factory address, project owner or pool address.

69:     error CallerIsNotSuperAdmin(address caller); //           The caller of this function must match the superAdmin address in storage.

107:     error IncorrectPayment(); //                              The function call did not include passing the correct payment.

198:     error CallerIsNotAdminNorFactory(); //                  The caller of this function must match the factory address or be an admin.

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IErrors.sol)

```solidity
File: contracts/virtualPersona/IExecutionInterface.sol

5:     function execute(address to, uint256 value, bytes memory data, uint8 operation) external returns (bool success);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IExecutionInterface.sol)

```solidity
File: contracts/virtualPersona/IValidatorRegistry.sol

7:     function isValidator(uint256 virtualId, address account) external view returns (bool);

9:     function validatorScore(uint256 virtualId, address validator) external view returns (uint256);

17:     function validatorCount(uint256 virtualId) external view returns (uint256);

19:     function validatorAt(uint256 virtualId, uint256 index) external view returns (address);

21:     function totalUptimeScore(uint256 virtualId) external view returns (uint256);

23:     function addValidator(uint256 virtualId, address validator) external;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IValidatorRegistry.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

12:     function(uint256, address) view returns (uint256) private _getScoreOf;

13:     function(uint256) view returns (uint256) private _getMaxScore;

14:     function(uint256, address, uint256) view returns (uint256) private _getPastScore;

17:         function(uint256, address) view returns (uint256) getScoreOf_,

18:         function(uint256) view returns (uint256) getMaxScore_,

19:         function(uint256, address, uint256) view returns (uint256) getPastScore_

26:     function isValidator(uint256 virtualId, address account) public view returns (bool) {

30:     function _addValidator(uint256 virtualId, address validator) internal {

36:     function _initValidatorScore(uint256 virtualId, address validator) internal {

40:     function validatorScore(uint256 virtualId, address validator) public view virtual returns (uint256) {

52:     function validatorCount(uint256 virtualId) public view returns (uint256) {

56:     function validatorAt(uint256 virtualId, uint256 index) public view returns (address) {

60:     function totalUptimeScore(uint256 virtualId) public view returns (uint256) {

69:         function(uint256, address) view returns (uint256) getScoreOf_,

70:         function(uint256) view returns (uint256) getMaxScore_,

71:         function(uint256, address, uint256) view returns (uint256) getPastScore_

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="NC-16"></a>[NC-16] Change int to int256

Throughout the code base, some variables are declared as `int`. To favor explicitness, consider changing all instances of `int` to `int256`

*Instances (21)*:

```solidity
File: contracts/contribution/ContributionNft.sol

78:             "Only proposal proposer can mint Contribution NFT"

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/genesis/Genesis.sol

178:         require(pointAmt > 0, "Point amount must be greater than 0");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

14:     struct Checkpoint {

27:             Checkpoint memory last = self[pos - 1];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

9:                     SIMPLIFIED FIXED POINT OPERATIONS

33:                     LOW LEVEL FIXED POINT OPERATIONS

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

22:     struct Checkpoint {

35:             Checkpoint memory last = self[pos - 1];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

19:     struct Checkpoint {

32:             Checkpoint memory last = self[pos - 1];

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

117:         uint48 currentTimepoint = clock();

118:         if (timepoint >= currentTimepoint) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

110:         uint48 currentTimepoint = clock();

111:         if (timepoint >= currentTimepoint) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/ERC20Votes.sol

19:         uint48 currentTimepoint = clock();

20:         if (timepoint >= currentTimepoint) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ERC20Votes.sol)

```solidity
File: contracts/virtualPersona/IErrors.sol

119:     error InvalidEndpointCaller(); //                         The calling address is not a valid LZ endpoint. The LZ endpoint was set at contract creation

168:     error MintToZeroAddress(); //                             Cannot mint to the zero address.

220:     error PaymentMustCoverPerMintFee(); //                    The payment passed must at least cover the per mint fee for the quantity requested.

256:     ); //                                                     Number of tokens requested for this mint exceeds the remaining allocation (taking the

279:     error ThisMintIsClosed(); //                              It's over (well, this mint is, anyway).

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IErrors.sol)

### <a name="NC-17"></a>[NC-17] Change uint to uint256

Throughout the code base, some variables are declared as `uint`. To favor explicitness, consider changing all instances of `uint` to `uint256`

*Instances (105)*:

```solidity
File: contracts/fun/Bonding.sol

269:         uint n = tokenInfos.length;

412:         for (uint i = 0; i < accounts.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

16:     uint public maxTx;

26:     event MaxTxUpdated(uint _maxTx);

28:     constructor(string memory name_, string memory symbol_, uint256 supply, uint _maxTx) Ownable(msg.sender) {

114:     function _updateMaxTx(uint _maxTx) internal {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

53:         uint n = pairs.length;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

113:         uint fee = factory.sellTax();

142:         uint fee = factory.buyTax();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

44:         uint amountIn,

45:         uint amountOutMin,

48:         uint deadline

58:     function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/pool/IRouter.sol

6:         uint amountIn,

7:         uint amountOutMin,

10:         uint deadline

13:     function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IRouter.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

4:     event Approval(address indexed owner, address indexed spender, uint value);

5:     event Transfer(address indexed from, address indexed to, uint value);

14:     function approve(address spender, uint value) external returns (bool);

15:     function transfer(address to, uint value) external returns (bool);

16:     function transferFrom(address from, address to, uint value) external returns (bool);

22:     function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

24:     event Mint(address indexed sender, uint amount0, uint amount1);

25:     event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);

28:         uint amount0In,

29:         uint amount1In,

30:         uint amount0Out,

31:         uint amount1Out,

45:     function mint(address to) external returns (uint liquidity);

46:     function burn(address to) external returns (uint amount0, uint amount1);

47:     function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/pool/IUniswapV2Router01.sol

10:         uint amountADesired,

11:         uint amountBDesired,

12:         uint amountAMin,

13:         uint amountBMin,

15:         uint deadline

16:     ) external returns (uint amountA, uint amountB, uint liquidity);

19:         uint amountTokenDesired,

20:         uint amountTokenMin,

21:         uint amountETHMin,

23:         uint deadline

24:     ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

28:         uint liquidity,

29:         uint amountAMin,

30:         uint amountBMin,

32:         uint deadline

33:     ) external returns (uint amountA, uint amountB);

36:         uint liquidity,

37:         uint amountTokenMin,

38:         uint amountETHMin,

40:         uint deadline

41:     ) external returns (uint amountToken, uint amountETH);

45:         uint liquidity,

46:         uint amountAMin,

47:         uint amountBMin,

49:         uint deadline,

54:     ) external returns (uint amountA, uint amountB);

57:         uint liquidity,

58:         uint amountTokenMin,

59:         uint amountETHMin,

61:         uint deadline,

66:     ) external returns (uint amountToken, uint amountETH);

68:         uint amountIn,

69:         uint amountOutMin,

72:         uint deadline

75:         uint amountOut,

76:         uint amountInMax,

79:         uint deadline

82:         uint amountOutMin,

85:         uint deadline

88:         uint amountOut,

89:         uint amountInMax,

92:         uint deadline

95:         uint amountIn,

96:         uint amountOutMin,

99:         uint deadline

102:         uint amountOut,

105:         uint deadline

108:     function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);

109:     function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);

110:     function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);

111:     function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);

112:     function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router01.sol)

```solidity
File: contracts/pool/IUniswapV2Router02.sol

8:         uint liquidity,

9:         uint amountTokenMin,

10:         uint amountETHMin,

12:         uint deadline

13:     ) external returns (uint amountETH);

16:         uint liquidity,

17:         uint amountTokenMin,

18:         uint amountETHMin,

20:         uint deadline,

25:     ) external returns (uint amountETH);

28:         uint amountIn,

29:         uint amountOutMin,

32:         uint deadline

35:         uint amountOutMin,

38:         uint deadline

41:         uint amountIn,

42:         uint amountOutMin,

45:         uint deadline

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router02.sol)

```solidity
File: contracts/tax/AgentTax.sol

177:         for (uint i = 0; i < txhashes.length; i++) {

272:         for (uint i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

47:         for (uint i = 0; i < txhashes.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

### <a name="NC-18"></a>[NC-18] Interfaces should be defined in separate files from their usage

The interfaces below should be defined in separate files, so that it's easier for future projects to import them, and to avoid duplication later on if they need to be used elsewhere in the project

*Instances (1)*:

```solidity
File: contracts/pool/AeroAdaptor.sol

8: interface IAeroRouter {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

### <a name="NC-19"></a>[NC-19] Lack of checks in setters

Be it sanity checks (like checks against `0`-values) or initial setting checks: it's best for Setter functions to have them

*Instances (71)*:

```solidity
File: contracts/contribution/ContributionNft.sol

141:     function _update(
             address to,
             uint256 tokenId,
             address auth
         ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) returns (address) {
             return super._update(to, tokenId, auth);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

141:     function setDatasetImpactWeight(uint16 weight) public onlyOwner {
             datasetImpactWeight = weight;
             emit DatasetImpactUpdated(weight);

168:     function _update(
             address to,
             uint256 tokenId,
             address auth
         ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) returns (address) {
             return super._update(to, tokenId, auth);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

143:     function setInitialSupply(uint256 newSupply) public onlyOwner {
             initialSupply = newSupply;

147:     function setGradThreshold(uint256 newThreshold) public onlyOwner {
             gradThreshold = newThreshold;

151:     function setFee(uint256 newFee, address newFeeTo) public onlyOwner {
             fee = newFee;
             _feeTo = newFeeTo;

156:     function setMaxTx(uint256 maxTx_) public onlyOwner {
             maxTx = maxTx_;

166:     function setDeployParams(DeployParams memory params) public onlyOwner {
             _deployParams = params;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

114:     function _updateMaxTx(uint _maxTx) internal {
             maxTx = _maxTx;
             _maxTxAmount = (maxTx * _totalSupply) / 100;
     
             emit MaxTxUpdated(_maxTx);

121:     function updateMaxTx(uint256 _maxTx) public onlyOwner {
             _updateMaxTx(_maxTx);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

82:     function setRouter(address router_) public onlyRole(ADMIN_ROLE) {
            router = router_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

183:     function setTaxManager(address newManager) public onlyRole(ADMIN_ROLE) {
             taxManager = newManager;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

53:     function setParams(Params calldata p) external onlyRole(ADMIN_ROLE) {
            _setParams(p);

134:     function withdrawLeftAssetsAfterFinalized(
             uint256 id,
             address to,
             address token,
             uint256 amount
         ) external onlyRole(ADMIN_ROLE) {
             _getGenesis(id).withdrawLeftAssetsAfterFinalized(to, token, amount);

143:     function resetTime(uint256 id, uint256 newStartTime, uint256 newEndTime) external onlyRole(OPERATION_ROLE) {
             _getGenesis(id).resetTime(newStartTime, newEndTime);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

462:     function resetTime(
             uint256 newStartTime,
             uint256 newEndTime
         ) external onlyRole(FACTORY_ROLE) nonReentrant whenNotCancelled whenNotFailed whenNotStarted whenNotEnded {
             _validateTime(newStartTime, newEndTime);
     
             uint256 oldStartTime = startTime;
             uint256 oldEndTime = endTime;
     
             startTime = newStartTime;
             endTime = newEndTime;
     
             emit TimeReset(oldStartTime, oldEndTime, newStartTime, newEndTime);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

39:     function setMockAgentToken(address token) external {
            mockAgentToken = token;

43:     function setMockId(uint256 id) external {
            mockId = id;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

128:     function updateQuorum(uint224 newQuorum) public onlyGovernance {
             uint224 oldQuorum = _quorumCheckpoints.latest();
             _quorumCheckpoints.push(SafeCast.toUint32(clock()), SafeCast.toUint208(newQuorum));
             emit QuorumUpdated(oldQuorum, newQuorum);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

43:     function _update(address from, address to, uint256 value) internal override(ERC20, ERC20Votes) {
            super._update(from, to, value);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/tax/AgentTax.sol

147:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {
             uint256 oldMin = minSwapThreshold;
             uint256 oldMax = maxSwapThreshold;
     
             minSwapThreshold = minSwapThreshold_;
             maxSwapThreshold = maxSwapThreshold_;
     
             emit SwapThresholdUpdated(oldMin, minSwapThreshold_, oldMax, maxSwapThreshold_);

157:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {
             address oldTreasury = treasury;
             treasury = treasury_;
     
             emit TreasuryUpdated(oldTreasury, treasury_);

287:     function updateTbaBonus(address tbaBonus_) public onlyRole(ADMIN_ROLE) {
             tbaBonus = ITBABonus(tbaBonus_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

80:     function updateSwapParams(
            address router_,
            address bondingRouter_,
            address assetToken_,
            uint16 slippage_
        ) public onlyRole(ADMIN_ROLE) {
            address oldRouter = address(router);
            address oldBondingRouter = bondingRouter;
            address oldAsset = assetToken;
    
            assetToken = assetToken_;
            router = IRouter(router_);
            bondingRouter = bondingRouter_;
            _slippage = slippage_;
    
            IERC20(taxToken).forceApprove(router_, type(uint256).max);
            IERC20(taxToken).forceApprove(oldRouter, 0);
    
            emit SwapParamsUpdated(oldRouter, router_, oldBondingRouter, bondingRouter_, oldAsset, assetToken_);

101:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {
             uint256 oldMin = minSwapThreshold;
             uint256 oldMax = maxSwapThreshold;
     
             minSwapThreshold = minSwapThreshold_;
             maxSwapThreshold = maxSwapThreshold_;
     
             emit SwapThresholdUpdated(oldMin, minSwapThreshold_, oldMax, maxSwapThreshold_);

111:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {
             address oldTreasury = treasury;
             treasury = treasury_;
     
             emit TreasuryUpdated(oldTreasury, treasury_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/TBABonus.sol

44:     function updateBonusRate(uint16 bonusRate_) public onlyRole(ADMIN_ROLE) {
            uint16 oldBonusRate = bonusRate;
            bonusRate = bonusRate_;
            emit BonusRateUpdated(bonusRate_, oldBonusRate);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

327:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {
             applicationThreshold = newThreshold;
             emit ApplicationThresholdUpdated(newThreshold);

332:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _vault = newVault;

336:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {
             tokenImplementation = token;
             daoImplementation = dao;
             veTokenImplementation = veToken;

342:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {
             maturityDuration = newDuration;

346:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _uniswapRouter = router;

350:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenAdmin = newTokenAdmin;

354:     function setTokenSupplyParams(
             uint256 maxSupply,
             uint256 lpSupply,
             uint256 vaultSupply,
             uint256 maxTokensPerWallet,
             uint256 maxTokensPerTxn,
             uint256 botProtectionDurationInSeconds,
             address vault
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenSupplyParams = abi.encode(

374:     function setTokenTaxParams(
             uint256 projectBuyTaxBasisPoints,
             uint256 projectSellTaxBasisPoints,
             uint256 taxSwapThresholdBasisPoints,
             address projectTaxRecipient
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenTaxParams = abi.encode(

388:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {
             assetToken = newToken;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

341:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {
             applicationThreshold = newThreshold;
             emit ApplicationThresholdUpdated(newThreshold);

346:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _vault = newVault;

350:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {
             tokenImplementation = token;
             daoImplementation = dao;
             veTokenImplementation = veToken;

356:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {
             maturityDuration = newDuration;

360:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _uniswapRouter = router;

364:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenAdmin = newTokenAdmin;

368:     function setTokenSupplyParams(
             uint256 maxSupply,
             uint256 lpSupply,
             uint256 vaultSupply,
             uint256 maxTokensPerWallet,
             uint256 maxTokensPerTxn,
             uint256 botProtectionDurationInSeconds,
             address vault
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenSupplyParams = abi.encode(

388:     function setTokenTaxParams(
             uint256 projectBuyTaxBasisPoints,
             uint256 projectSellTaxBasisPoints,
             uint256 taxSwapThresholdBasisPoints,
             address projectTaxRecipient
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenTaxParams = abi.encode(

402:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {
             assetToken = newToken;

489:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {
             defaultDelegatee = newDelegatee;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

365:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {
             applicationThreshold = newThreshold;
             emit ApplicationThresholdUpdated(newThreshold);

370:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _vault = newVault;

374:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {
             tokenImplementation = token;
             daoImplementation = dao;
             veTokenImplementation = veToken;

380:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {
             maturityDuration = newDuration;

384:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _uniswapRouter = router;

388:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenAdmin = newTokenAdmin;

392:     function setTokenSupplyParams(
             uint256 maxSupply,
             uint256 lpSupply,
             uint256 vaultSupply,
             uint256 maxTokensPerWallet,
             uint256 maxTokensPerTxn,
             uint256 botProtectionDurationInSeconds,
             address vault
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenSupplyParams = abi.encode(

412:     function setTokenTaxParams(
             uint256 projectBuyTaxBasisPoints,
             uint256 projectSellTaxBasisPoints,
             uint256 taxSwapThresholdBasisPoints,
             address projectTaxRecipient
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenTaxParams = abi.encode(

426:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {
             assetToken = newToken;

446:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {
             defaultDelegatee = newDelegatee;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

46:     function setInitParams(
            address tokenAdmin_,
            address assetToken_,
            address uniswapRouter_,
            uint256 initialAmount_,
            uint256 maturityDuration_
        ) external onlyOwner {
            _tokenAdmin = tokenAdmin_;
            _assetToken = assetToken_;
            _uniswapRouter = uniswapRouter_;
            initialAmount = initialAmount_;
            maturityDuration = maturityDuration_;

60:     function setTokenSupplyParams(
            uint256 maxSupply,
            uint256 lpSupply,
            uint256 vaultSupply,
            uint256 maxTokensPerWallet,
            uint256 maxTokensPerTxn,
            uint256 botProtectionDurationInSeconds,
            address vault
        ) public onlyOwner {
            _tokenSupplyParams = abi.encode(

80:     function setTokenTaxParams(
            uint256 projectBuyTaxBasisPoints,
            uint256 projectSellTaxBasisPoints,
            uint256 taxSwapThresholdBasisPoints,
            address projectTaxRecipient
        ) public onlyOwner {
            _tokenTaxParams = abi.encode(

94:     function setImplementations(address token, address veToken, address dao) external onlyOwner {
            tokenImplementation = token;
            daoImplementation = dao;
            veTokenImplementation = veToken;

191:     function reset(uint256 id) external onlyOwner {
             migratedAgents[id] = false;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

73:     function setContributionService(
            address contributionNft_,
            address serviceNft_
        ) external onlyRole(DEFAULT_ADMIN_ROLE) {
            _contributionNft = contributionNft_;
            _serviceNft = serviceNft_;

163:     function setCoreTypes(uint256 virtualId, uint8[] memory coreTypes) external onlyVirtualDAO(virtualId) {
             VirtualInfo storage info = virtualInfos[virtualId];
             info.coreTypes = coreTypes;
             emit CoresUpdated(virtualId, coreTypes);

169:     function setTokenURI(uint256 virtualId, string memory newTokenURI) public onlyVirtualDAO(virtualId) {
             return _setTokenURI(virtualId, newTokenURI);

234:     function setBlacklist(uint256 virtualId, bool value) public onlyRole(ADMIN_ROLE) {
             _blacklists[virtualId] = value;
             emit AgentBlacklisted(virtualId, value);

243:     function setEloCalculator(address eloCalculator) public onlyRole(ADMIN_ROLE) {
             _eloCalculator = eloCalculator;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

382:     function setProjectTaxRecipient(address projectTaxRecipient_) external onlyOwnerOrFactory {
             projectTaxRecipient = projectTaxRecipient_;
             emit ProjectTaxRecipientUpdated(projectTaxRecipient_);

394:     function setSwapThresholdBasisPoints(uint16 swapThresholdBasisPoints_) external onlyOwnerOrFactory {
             uint256 oldswapThresholdBasisPoints = swapThresholdBasisPoints;
             swapThresholdBasisPoints = swapThresholdBasisPoints_;
             emit AutoSwapThresholdUpdated(oldswapThresholdBasisPoints, swapThresholdBasisPoints_);

408:     function setProjectTaxRates(
             uint16 newProjectBuyTaxBasisPoints_,
             uint16 newProjectSellTaxBasisPoints_
         ) external onlyOwnerOrFactory {
             uint16 oldBuyTaxBasisPoints = projectBuyTaxBasisPoints;
             uint16 oldSellTaxBasisPoints = projectSellTaxBasisPoints;
     
             projectBuyTaxBasisPoints = newProjectBuyTaxBasisPoints_;
             projectSellTaxBasisPoints = newProjectSellTaxBasisPoints_;
     
             emit ProjectTaxBasisPointsChanged(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

131:     function _update(
             address from,
             address to,
             uint256 value
         ) internal override(ERC20Upgradeable, ERC20VotesUpgradeable) {
             super._update(from, to, value);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

57:     function setK(uint256 k_) public onlyOwner {
            k = k_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="NC-20"></a>[NC-20] Lines are too long

Usually lines in source code are limited to [80](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over [164](https://github.com/aizatto/character-length) characters, the lines below should be split when they reach that length

*Instances (3)*:

```solidity
File: contracts/virtualPersona/IErrors.sol

37:     error AuxCallFailed(address[] modules, uint256 value, bytes data, uint256 txGas); //                                                     An auxilliary call from the drop factory failed.

152:     error MaxPublicMintAllowanceExceeded(uint256 requested, uint256 alreadyMinted, uint256 maxAllowance); //                                                     The calling address has requested a quantity that would exceed the max allowance.

293:     error TransferFromZeroAddress(); //                       Cannot transfer from the zero address. Indeed, this surely is impossible, and likely a waste to check??

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IErrors.sol)

### <a name="NC-21"></a>[NC-21] `type(uint256).max` should be used instead of `2 ** 256 - 1`

*Instances (1)*:

```solidity
File: contracts/libs/FixedPointMathLib.sol

12:     uint256 internal constant MAX_UINT256 = 2 ** 256 - 1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

### <a name="NC-22"></a>[NC-22] Missing Event for critical parameters change

Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

*Instances (58)*:

```solidity
File: contracts/contribution/ContributionNft.sol

103:     function setAdmin(address newAdmin) public {
             require(_msgSender() == _admin, "Only admin can set admin");
             _admin = newAdmin;

165:     function setEloCalculator(address eloCalculator_) public {
             require(_msgSender() == _admin, "Only admin can set elo calculator");
             _eloCalculator = eloCalculator_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/fun/Bonding.sol

143:     function setInitialSupply(uint256 newSupply) public onlyOwner {
             initialSupply = newSupply;

147:     function setGradThreshold(uint256 newThreshold) public onlyOwner {
             gradThreshold = newThreshold;

151:     function setFee(uint256 newFee, address newFeeTo) public onlyOwner {
             fee = newFee;
             _feeTo = newFeeTo;

156:     function setMaxTx(uint256 maxTx_) public onlyOwner {
             maxTx = maxTx_;

160:     function setAssetRate(uint256 newRate) public onlyOwner {
             require(newRate > 0, "Rate err");
     
             assetRate = newRate;

166:     function setDeployParams(DeployParams memory params) public onlyOwner {
             _deployParams = params;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

121:     function updateMaxTx(uint256 _maxTx) public onlyOwner {
             _updateMaxTx(_maxTx);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

74:     function setTaxParams(address newVault_, uint256 buyTax_, uint256 sellTax_) public onlyRole(ADMIN_ROLE) {
            require(newVault_ != address(0), "Zero addresses are not allowed.");
    
            taxVault = newVault_;
            buyTax = buyTax_;
            sellTax = sellTax_;

82:     function setRouter(address router_) public onlyRole(ADMIN_ROLE) {
            router = router_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

82:     function transferAsset(address recipient, uint256 amount) public onlyRouter {
            require(recipient != address(0), "Zero addresses are not allowed.");
    
            IERC20(tokenB).safeTransfer(recipient, amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

183:     function setTaxManager(address newManager) public onlyRole(ADMIN_ROLE) {
             taxManager = newManager;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

53:     function setParams(Params calldata p) external onlyRole(ADMIN_ROLE) {
            _setParams(p);

134:     function withdrawLeftAssetsAfterFinalized(
             uint256 id,
             address to,
             address token,
             uint256 amount
         ) external onlyRole(ADMIN_ROLE) {
             _getGenesis(id).withdrawLeftAssetsAfterFinalized(to, token, amount);

143:     function resetTime(uint256 id, uint256 newStartTime, uint256 newEndTime) external onlyRole(OPERATION_ROLE) {
             _getGenesis(id).resetTime(newStartTime, newEndTime);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

39:     function setMockAgentToken(address token) external {
            mockAgentToken = token;

43:     function setMockId(uint256 id) external {
            mockId = id;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/tax/AgentTax.sol

287:     function updateTbaBonus(address tbaBonus_) public onlyRole(ADMIN_ROLE) {
             tbaBonus = ITBABonus(tbaBonus_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

332:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _vault = newVault;

336:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {
             tokenImplementation = token;
             daoImplementation = dao;
             veTokenImplementation = veToken;

342:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {
             maturityDuration = newDuration;

346:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _uniswapRouter = router;

350:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenAdmin = newTokenAdmin;

354:     function setTokenSupplyParams(
             uint256 maxSupply,
             uint256 lpSupply,
             uint256 vaultSupply,
             uint256 maxTokensPerWallet,
             uint256 maxTokensPerTxn,
             uint256 botProtectionDurationInSeconds,
             address vault
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenSupplyParams = abi.encode(

374:     function setTokenTaxParams(
             uint256 projectBuyTaxBasisPoints,
             uint256 projectSellTaxBasisPoints,
             uint256 taxSwapThresholdBasisPoints,
             address projectTaxRecipient
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenTaxParams = abi.encode(

388:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {
             assetToken = newToken;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

346:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _vault = newVault;

350:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {
             tokenImplementation = token;
             daoImplementation = dao;
             veTokenImplementation = veToken;

356:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {
             maturityDuration = newDuration;

360:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _uniswapRouter = router;

364:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenAdmin = newTokenAdmin;

368:     function setTokenSupplyParams(
             uint256 maxSupply,
             uint256 lpSupply,
             uint256 vaultSupply,
             uint256 maxTokensPerWallet,
             uint256 maxTokensPerTxn,
             uint256 botProtectionDurationInSeconds,
             address vault
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenSupplyParams = abi.encode(

388:     function setTokenTaxParams(
             uint256 projectBuyTaxBasisPoints,
             uint256 projectSellTaxBasisPoints,
             uint256 taxSwapThresholdBasisPoints,
             address projectTaxRecipient
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenTaxParams = abi.encode(

402:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {
             assetToken = newToken;

489:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {
             defaultDelegatee = newDelegatee;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

370:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _vault = newVault;

374:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {
             tokenImplementation = token;
             daoImplementation = dao;
             veTokenImplementation = veToken;

380:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {
             maturityDuration = newDuration;

384:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _uniswapRouter = router;

388:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenAdmin = newTokenAdmin;

392:     function setTokenSupplyParams(
             uint256 maxSupply,
             uint256 lpSupply,
             uint256 vaultSupply,
             uint256 maxTokensPerWallet,
             uint256 maxTokensPerTxn,
             uint256 botProtectionDurationInSeconds,
             address vault
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenSupplyParams = abi.encode(

412:     function setTokenTaxParams(
             uint256 projectBuyTaxBasisPoints,
             uint256 projectSellTaxBasisPoints,
             uint256 taxSwapThresholdBasisPoints,
             address projectTaxRecipient
         ) public onlyRole(DEFAULT_ADMIN_ROLE) {
             _tokenTaxParams = abi.encode(

426:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {
             assetToken = newToken;

446:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {
             defaultDelegatee = newDelegatee;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

46:     function setInitParams(
            address tokenAdmin_,
            address assetToken_,
            address uniswapRouter_,
            uint256 initialAmount_,
            uint256 maturityDuration_
        ) external onlyOwner {
            _tokenAdmin = tokenAdmin_;
            _assetToken = assetToken_;
            _uniswapRouter = uniswapRouter_;
            initialAmount = initialAmount_;
            maturityDuration = maturityDuration_;

60:     function setTokenSupplyParams(
            uint256 maxSupply,
            uint256 lpSupply,
            uint256 vaultSupply,
            uint256 maxTokensPerWallet,
            uint256 maxTokensPerTxn,
            uint256 botProtectionDurationInSeconds,
            address vault
        ) public onlyOwner {
            _tokenSupplyParams = abi.encode(

80:     function setTokenTaxParams(
            uint256 projectBuyTaxBasisPoints,
            uint256 projectSellTaxBasisPoints,
            uint256 taxSwapThresholdBasisPoints,
            address projectTaxRecipient
        ) public onlyOwner {
            _tokenTaxParams = abi.encode(

94:     function setImplementations(address token, address veToken, address dao) external onlyOwner {
            tokenImplementation = token;
            daoImplementation = dao;
            veTokenImplementation = veToken;

191:     function reset(uint256 id) external onlyOwner {
             migratedAgents[id] = false;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

73:     function setContributionService(
            address contributionNft_,
            address serviceNft_
        ) external onlyRole(DEFAULT_ADMIN_ROLE) {
            _contributionNft = contributionNft_;
            _serviceNft = serviceNft_;

169:     function setTokenURI(uint256 virtualId, string memory newTokenURI) public onlyVirtualDAO(virtualId) {
             return _setTokenURI(virtualId, newTokenURI);

173:     function setTBA(uint256 virtualId, address tba) external onlyRole(MINTER_ROLE) {
             VirtualInfo storage info = virtualInfos[virtualId];
             require(info.tba == address(0), "TBA already set");
             info.tba = tba;

179:     function setDAO(uint256 virtualId, address newDAO) public {
             require(_msgSender() == virtualInfos[virtualId].dao, "Caller is not VIRTUAL DAO");
             VirtualInfo storage info = virtualInfos[virtualId];
             info.dao = newDAO;

243:     function setEloCalculator(address eloCalculator) public onlyRole(ADMIN_ROLE) {
             _eloCalculator = eloCalculator;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

84:     function setCanStake(bool _canStake) public {
            require(_msgSender() == founder, "Not founder");
            canStake = _canStake;

89:     function setMatureAt(uint256 _matureAt) public {
            bytes32 ADMIN_ROLE = keccak256("ADMIN_ROLE");
            require(IAccessControl(agentNft).hasRole(ADMIN_ROLE, _msgSender()), "Not admin");
            matureAt = _matureAt;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

57:     function setK(uint256 k_) public onlyOwner {
            k = k_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="NC-23"></a>[NC-23] NatSpec is completely non-existent on functions that should have them

Public and external functions that aren't view or pure should have NatSpec comments

*Instances (184)*:

```solidity
File: contracts/AgentInference.sol

24:     function initialize(address defaultAdmin_, address token_, address agentNft_) external initializer {

35:     function prompt(

62:     function promptMulti(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

42:     function initialize(address thePersonaAddress) public initializer {

65:     function mint(

103:     function setAdmin(address newAdmin) public {

165:     function setEloCalculator(address eloCalculator_) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

44:     function initialize(

58:     function mint(uint256 virtualId, bytes32 descHash) public returns (uint256) {

90:     function updateImpact(uint256 virtualId, uint256 proposalId) public {

141:     function setDatasetImpactWeight(uint16 weight) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

93:     function initialize(

143:     function setInitialSupply(uint256 newSupply) public onlyOwner {

147:     function setGradThreshold(uint256 newThreshold) public onlyOwner {

151:     function setFee(uint256 newFee, address newFeeTo) public onlyOwner {

156:     function setMaxTx(uint256 maxTx_) public onlyOwner {

160:     function setAssetRate(uint256 newRate) public onlyOwner {

166:     function setDeployParams(DeployParams memory params) public onlyOwner {

178:     function launch(

190:     function launchFor(

281:     function sell(uint256 amountIn, address tokenAddress) public returns (bool) {

318:     function buy(uint256 amountIn, address tokenAddress) public payable returns (bool) {

405:     function unwrapToken(address srcTokenAddress, address[] memory accounts) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

66:     function transfer(address recipient, uint256 amount) public override returns (bool) {

76:     function approve(address spender, uint256 amount) public override returns (bool) {

82:     function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {

121:     function updateMaxTx(uint256 _maxTx) public onlyOwner {

125:     function excludeFromMaxTx(address user) public onlyOwner {

136:     function burnFrom(address user, uint256 amount) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

31:     function initialize(address taxVault_, uint256 buyTax_, uint256 sellTax_) external initializer {

60:     function createPair(address tokenA, address tokenB) external onlyRole(CREATOR_ROLE) nonReentrant returns (address) {

74:     function setTaxParams(address newVault_, uint256 buyTax_, uint256 sellTax_) public onlyRole(ADMIN_ROLE) {

82:     function setRouter(address router_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

45:     function mint(uint256 reserve0, uint256 reserve1) public onlyRouter returns (bool) {

55:     function swap(

71:     function approval(address _user, address _token, uint256 amount) public onlyRouter returns (bool) {

82:     function transferAsset(address recipient, uint256 amount) public onlyRouter {

88:     function transferTo(address recipient, uint256 amount) public onlyRouter {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

29:     function initialize(address factory_, address assetToken_) external initializer {

75:     function addInitialLiquidity(

95:     function sell(

131:     function buy(

165:     function graduate(address tokenAddress) public onlyRole(EXECUTOR_ROLE) nonReentrant {

172:     function approval(

183:     function setTaxManager(address newManager) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

46:     function initialize(Params memory p) external initializer {

53:     function setParams(Params calldata p) external onlyRole(ADMIN_ROLE) {

76:     function createGenesis(GenesisCreationParams memory gParams) external returns (address) {

116:     function onGenesisSuccess(

130:     function onGenesisFailed(uint256 id, uint256[] calldata participantIndexes) external onlyRole(OPERATION_ROLE) {

134:     function withdrawLeftAssetsAfterFinalized(

143:     function resetTime(uint256 id, uint256 newStartTime, uint256 newEndTime) external onlyRole(OPERATION_ROLE) {

147:     function cancelGenesis(uint256 id) external onlyRole(OPERATION_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

134:     function initialize(GenesisInitParams calldata params) external initializer {

177:     function participate(uint256 pointAmt, uint256 virtualsAmt) external nonReentrant whenActive {

205:     function onGenesisSuccess(

307:     function claimAgentToken(address userAddress) external nonReentrant {

324:     function onGenesisFailed(

448:     function withdrawLeftAssetsAfterFinalized(

462:     function resetTime(

477:     function cancelGenesis()

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

21:     function initialize(

39:     function setMockAgentToken(address token) external {

43:     function setMockId(uint256 id) external {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

61:     function propose(

95:     function earlyExecute(uint256 proposalId) public payable onlyRole(EXECUTOR_ROLE) returns (uint256) {

128:     function updateQuorum(uint224 newQuorum) public onlyGovernance {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/VirtualProtocolDAO.sol

46:     function propose(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualProtocolDAO.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

15:     function oracleTransfer(

29:     function approve(address /*spender*/, uint256 /*value*/) public override returns (bool) {

33:     function transfer(address /*to*/, uint256 /*value*/) public override returns (bool) {

37:     function transferFrom(address /*from*/, address /*to*/, uint256 /*value*/) public override returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/libs/TokenSaver.sol

20:     function saveToken(address _token, address _receiver, uint256 _amount) external onlyTokenSaver {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

16:     function swapExactTokensForTokens(

43:     function swapExactTokensForTokens(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/tax/AgentTax.sol

83:     function initialize(

115:     function updateSwapParams(

147:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

157:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

164:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

168:     function handleAgentTaxes(

255:     function updateCreator(uint256 agentId, address creator) public {

269:     function dcaSell(uint256[] memory agentIds, uint256 slippage, uint256 maxOverride) public onlyRole(EXECUTOR_ROLE) {

287:     function updateTbaBonus(address tbaBonus_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

54:     function initialize(

80:     function updateSwapParams(

101:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

111:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

118:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

122:     function swapForAsset() public onlyBondingRouter returns (bool, uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

27:     function initialize(address defaultAdmin_, address taxToken_) external initializer {

36:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

40:     function refund(

62:     function manualRefund(bytes32 txhash, address recipient, uint256 amount) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

33:     function initialize(address defaultAdmin_, address assetToken_) external initializer {

44:     function updateBonusRate(uint16 bonusRate_) public onlyRole(ADMIN_ROLE) {

50:     function setAllowances(uint256[] memory agentIds, uint256[] memory allowances) public onlyRole(ADMIN_ROLE) {

64:     function distributeBonus(uint256 agentId, address recipient, uint256 amount) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/token/Virtual.sol

17:     function mint(address _to, uint256 _amount) external onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

40:     function initialize(

76:     function propose(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

108:     function initialize(

136:     function proposeAgent(

179:     function withdraw(uint256 id) public noReentrant {

196:     function executeApplication(uint256 id, bool canStake) public noReentrant {

327:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

332:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

336:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

342:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

354:     function setTokenSupplyParams(

374:     function setTokenTaxParams(

388:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

396:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

110:     function initialize(

139:     function proposeAgent(

182:     function withdraw(uint256 id) public noReentrant {

268:     function executeApplication(uint256 id, bool canStake) public noReentrant {

341:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

356:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

360:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

364:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

368:     function setTokenSupplyParams(

388:     function setTokenTaxParams(

402:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

406:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

410:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

422:     function initFromBondingCurve(

466:     function executeBondingCurveApplication(

489:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

108:     function initialize(

137:     function proposeAgent(

180:     function withdraw(uint256 id) public noReentrant {

292:     function executeApplication(uint256 id, bool canStake) public noReentrant {

365:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

370:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

374:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

380:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

384:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function setTokenSupplyParams(

412:     function setTokenTaxParams(

426:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

430:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

434:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

446:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

451:     function initFromToken(

505:     function executeTokenApplication(uint256 id, bool canStake) public noReentrant {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

46:     function setInitParams(

60:     function setTokenSupplyParams(

80:     function setTokenTaxParams(

94:     function setImplementations(address token, address veToken, address dao) external onlyOwner {

100:     function migrateAgent(uint256 id, string memory name, string memory symbol, bool canStake) external noReentrant {

183:     function pause() external onlyOwner {

187:     function unpause() external onlyOwner {

191:     function reset(uint256 id) external onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

61:     function initialize(address defaultAdmin) public initializer {

73:     function setContributionService(

85:     function mint(

116:     function addCoreType(string memory label) public onlyRole(DEFAULT_ADMIN_ROLE) {

133:     function addValidator(uint256 virtualId, address validator) public {

163:     function setCoreTypes(uint256 virtualId, uint8[] memory coreTypes) external onlyVirtualDAO(virtualId) {

169:     function setTokenURI(uint256 virtualId, string memory newTokenURI) public onlyVirtualDAO(virtualId) {

173:     function setTBA(uint256 virtualId, address tba) external onlyRole(MINTER_ROLE) {

179:     function setDAO(uint256 virtualId, address newDAO) public {

234:     function setBlacklist(uint256 virtualId, bool value) public onlyRole(ADMIN_ROLE) {

239:     function migrateScoreFunctions() public onlyRole(ADMIN_ROLE) {

243:     function setEloCalculator(address eloCalculator) public onlyRole(ADMIN_ROLE) {

251:     function migrateVirtual(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

79:     function initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

39:     function initialize(

59:     function stake(uint256 amount, address receiver, address delegatee) public {

84:     function setCanStake(bool _canStake) public {

89:     function setMatureAt(uint256 _matureAt) public {

95:     function withdraw(uint256 amount) public noReentrant {

118:     function transfer(address /*to*/, uint256 /*value*/) public override returns (bool) {

122:     function transferFrom(address /*from*/, address /*to*/, uint256 /*value*/) public override returns (bool) {

126:     function approve(address /*spender*/, uint256 /*value*/) public override returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

18:     function initialize(address initialOwner) public initializer {

57:     function setK(uint256 k_) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="NC-24"></a>[NC-24] File's first line is not an SPDX Identifier

*Instances (7)*:

```solidity
File: contracts/genesis/GenesisLib.sol

1: // GenesisLib.sol

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/GenesisLib.sol)

```solidity
File: contracts/pool/IUniswapV2Factory.sol

1: pragma solidity >=0.5.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Factory.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

1: pragma solidity >=0.5.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/pool/IUniswapV2Router01.sol

1: pragma solidity >=0.6.2;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router01.sol)

```solidity
File: contracts/pool/IUniswapV2Router02.sol

1: pragma solidity >=0.6.2;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router02.sol)

```solidity
File: contracts/token/Airdrop.sol

1: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Airdrop.sol)

```solidity
File: contracts/token/Virtual.sol

1: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

### <a name="NC-25"></a>[NC-25] Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor

If a function is supposed to be access-controlled, a `modifier` should be used instead of a `require/if` statement for more readability.

*Instances (14)*:

```solidity
File: contracts/fun/Bonding.sol

202:         require(IERC20(assetToken).balanceOf(msg.sender) >= purchaseAmount, "Insufficient amount");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FPair.sol

37:         require(router == msg.sender, "Only router can call this function");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/genesis/Genesis.sol

185:         require(IERC20(virtualTokenAddress).balanceOf(msg.sender) >= virtualsAmt, "Insufficient Virtual Token balance");

193:         if (mapAddrToVirtuals[msg.sender] == 0) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/tax/TBABonus.sol

68:         if (amount == 0 || !hasRole(EXECUTOR_ROLE, msg.sender)) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

67:         require(msg.sender == gov, "Only DAO can execute proposal");

182:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

211:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

67:         require(msg.sender == gov, "Only DAO can execute proposal");

185:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

280:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

183:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

304:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

516:         require(msg.sender == application.proposer || hasRole(WITHDRAW_ROLE, msg.sender), "Not proposer");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

### <a name="NC-26"></a>[NC-26] Constant state variables defined more than once

Rather than redefining state variable constant, consider using a library to store all constants as this will prevent data redundancy

*Instances (21)*:

```solidity
File: contracts/AgentInference.sol

16:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/FFactory.sol

11:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

17:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

18:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

16:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

14:     bytes32 public constant BONDING_ROLE = keccak256("BONDING_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

31:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/tax/AgentTax.sol

25:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

26:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

28:     uint256 internal constant DENOM = 10000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

15:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

11:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

12:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

13:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

14:     bytes32 public constant EXECUTOR_ROLE = keccak256("EXECUTOR_ROLE");

16:     uint256 internal constant DENOM = 10000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

35:     bytes32 public constant WITHDRAW_ROLE = keccak256("WITHDRAW_ROLE"); // Able to withdraw and execute applications

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

35:     bytes32 public constant WITHDRAW_ROLE = keccak256("WITHDRAW_ROLE"); // Able to withdraw and execute applications

101:     bytes32 public constant BONDING_ROLE = keccak256("BONDING_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

37:     bytes32 public constant WITHDRAW_ROLE = keccak256("WITHDRAW_ROLE"); // Able to withdraw and execute applications

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

49:     bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

### <a name="NC-27"></a>[NC-27] Consider using named mappings

Consider moving to solidity version 0.8.18 or later, and using [named mappings](https://ethereum.stackexchange.com/questions/51629/how-to-name-the-arguments-in-mapping/145555#145555) to make it easier to understand the purpose of each mapping

*Instances (33)*:

```solidity
File: contracts/contribution/ContributionNft.sol

23:     mapping(uint256 => uint256) private _contributionVirtualId;

24:     mapping(uint256 => uint256) private _parents;

25:     mapping(uint256 => uint256[]) private _children;

26:     mapping(uint256 => uint8) private _cores;

28:     mapping(uint256 => bool) public modelContributions;

29:     mapping(uint256 => uint256) public modelDatasets;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/fun/Bonding.sol

78:     mapping(address => Profile) public profile;

81:     mapping(address => Token) public tokenInfo;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

20:     mapping(address => uint256) private _balances;

22:     mapping(address => mapping(address => uint256)) private _allowances;

24:     mapping(address => bool) private isExcludedFromMaxTx;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

14:     mapping(address => mapping(address => address)) private _pair;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/genesis/FGenesis.sol

36:     mapping(uint256 => address) public genesisContracts;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

18:     mapping(address => uint256) public mapAddrToVirtuals;

19:     mapping(address => uint256) public claimableAgentTokens;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

24:     mapping(uint256 => bool) _earlyExecutions;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

27:     mapping(address => Checkpoints.Trace208) private _scores;

28:     mapping(uint256 => uint256) private _proposalMaturities;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

62:     mapping(uint256 => Application) private _applications;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

62:     mapping(uint256 => Application) private _applications;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

64:     mapping(uint256 => Application) private _applications;

98:     mapping(address => uint256) private _tokenApplication;

99:     mapping(uint256 => address) private _applicationToken;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

29:     mapping(uint256 => bool) public migratedAgents;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

28:     mapping(address => uint256) private _stakingTokenToVirtualId;

43:     mapping(uint256 => VirtualInfo) public virtualInfos;

50:     mapping(uint256 => bool) private _blacklists;

51:     mapping(uint256 => VirtualLP) public virtualLPs;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

50:     mapping(address => uint256) private _balances;

53:     mapping(address => mapping(address => uint256)) private _allowances;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

28:     mapping(address => Checkpoints.Trace208) private _balanceCheckpoints;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

7:     mapping(uint8 => string) public coreTypes;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/ERC20Votes.sol

11:     mapping(address => AddressCheckpoints.Trace) private _delegateeCheckpoints;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ERC20Votes.sol)

### <a name="NC-28"></a>[NC-28] Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables

If the variable needs to be different based on which class it comes from, a `view`/`pure` *function* should be used instead (e.g. like [this](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/76eee35971c2541585e05cbf258510dda7b2fbc6/contracts/token/ERC20/extensions/draft-IERC20Permit.sol#L59)).

*Instances (6)*:

```solidity
File: contracts/pool/IUniswapV2Factory.sol

1: pragma solidity >=0.5.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Factory.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

1: pragma solidity >=0.5.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/pool/IUniswapV2Router01.sol

1: pragma solidity >=0.6.2;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router01.sol)

```solidity
File: contracts/pool/IUniswapV2Router02.sol

1: pragma solidity >=0.6.2;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router02.sol)

```solidity
File: contracts/token/Airdrop.sol

1: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Airdrop.sol)

```solidity
File: contracts/token/Virtual.sol

1: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

### <a name="NC-29"></a>[NC-29] Owner can renounce while system is paused

The contract owner or single user with a role is not prevented from renouncing the role/ownership while the contract is paused, which would cause any user assets stored in the protocol, to be locked indefinitely.

*Instances (2)*:

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

183:     function pause() external onlyOwner {

187:     function unpause() external onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

### <a name="NC-30"></a>[NC-30] Adding a `return` statement when the function defines a named return variable, is redundant

*Instances (24)*:

```solidity
File: contracts/fun/FRouter.sol

41:     function getAmountsOut(
            address token,
            address assetToken_,
            uint256 amountIn
        ) public view returns (uint256 _amountOut) {
            require(token != address(0), "Zero addresses are not allowed.");
    
            address pairAddress = factory.getPair(token, assetToken);
    
            IFPair pair = IFPair(pairAddress);
    
            (uint256 reserveA, uint256 reserveB) = pair.getReserves();
    
            uint256 k = pair.kLast();
    
            uint256 amountOut;
    
            if (assetToken_ == assetToken) {
                uint256 newReserveB = reserveB + amountIn;
    
                uint256 newReserveA = k / newReserveB;
    
                amountOut = reserveA - newReserveA;
            } else {
                uint256 newReserveA = reserveA + amountIn;
    
                uint256 newReserveB = k / newReserveA;
    
                amountOut = reserveB - newReserveB;
            }
    
            return amountOut;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/governance/GovernorCountingSimple.sol

46:     /**
         * @dev Accessor to the internal vote counts.
         */
        function proposalVotes(
            uint256 proposalId
        ) public view virtual returns (uint256 againstVotes, uint256 forVotes, uint256 abstainVotes) {
            ProposalVote storage proposalVote = _proposalVotes[proposalId];
            return (proposalVote.againstVotes, proposalVote.forVotes, proposalVote.abstainVotes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/GovernorCountingSimple.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

58:     function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts) {
            IAeroRouter.Route[] memory routes = new IAeroRouter.Route[](1);
            routes[0] = IAeroRouter.Route(tokenIn, tokenOut, false, factory);
            return IAeroRouter(router).getAmountsOut(amountIn, routes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

275:     function _createNewDAO(
             string memory name,
             IVotes token,
             uint32 daoVotingPeriod,
             uint256 daoThreshold
         ) internal returns (address instance) {
             instance = Clones.clone(daoImplementation);
             IAgentDAO(instance).initialize(name, token, nft, daoThreshold, daoVotingPeriod);
     
             allDAOs.push(instance);
             return instance;

288:     function _createNewAgentToken(string memory name, string memory symbol) internal returns (address instance) {
             instance = Clones.clone(tokenImplementation);
             IAgentToken(instance).initialize(
                 [_tokenAdmin, _uniswapRouter, assetToken],
                 abi.encode(name, symbol),
                 _tokenSupplyParams,
                 _tokenTaxParams
             );
     
             allTradingTokens.push(instance);
             return instance;

301:     function _createNewAgentVeToken(
             string memory name,
             string memory symbol,
             address stakingAsset,
             address founder,
             bool canStake
         ) internal returns (address instance) {
             instance = Clones.clone(veTokenImplementation);
             IAgentVeToken(instance).initialize(
                 name,
                 symbol,
                 founder,
                 stakingAsset,
                 block.timestamp + maturityDuration,
                 address(nft),
                 canStake
             );
     
             allTokens.push(instance);
             return instance;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

285:     function _createNewDAO(
             string memory name,
             IVotes token,
             uint32 daoVotingPeriod,
             uint256 daoThreshold
         ) internal returns (address instance) {
             instance = Clones.clone(daoImplementation);
             IAgentDAO(instance).initialize(name, token, nft, daoThreshold, daoVotingPeriod);
     
             allDAOs.push(instance);
             return instance;

298:     function _createNewAgentToken(
             string memory name,
             string memory symbol,
             bytes memory tokenSupplyParams_
         ) internal returns (address instance) {
             instance = Clones.clone(tokenImplementation);
             IAgentToken(instance).initialize(
                 [_tokenAdmin, _uniswapRouter, assetToken],
                 abi.encode(name, symbol),
                 tokenSupplyParams_,
                 _tokenTaxParams
             );
     
             allTradingTokens.push(instance);
             return instance;

315:     function _createNewAgentVeToken(
             string memory name,
             string memory symbol,
             address stakingAsset,
             address founder,
             bool canStake
         ) internal returns (address instance) {
             instance = Clones.clone(veTokenImplementation);
             IAgentVeToken(instance).initialize(
                 name,
                 symbol,
                 founder,
                 stakingAsset,
                 block.timestamp + maturityDuration,
                 address(nft),
                 canStake
             );
     
             allTokens.push(instance);
             return instance;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

309:     function _createNewDAO(
             string memory name,
             IVotes token,
             uint32 daoVotingPeriod,
             uint256 daoThreshold
         ) internal returns (address instance) {
             instance = Clones.clone(daoImplementation);
             IAgentDAO(instance).initialize(name, token, nft, daoThreshold, daoVotingPeriod);
     
             allDAOs.push(instance);
             return instance;

322:     function _createNewAgentToken(
             string memory name,
             string memory symbol,
             bytes memory tokenSupplyParams_
         ) internal returns (address instance) {
             instance = Clones.clone(tokenImplementation);
             IAgentToken(instance).initialize(
                 [_tokenAdmin, _uniswapRouter, assetToken],
                 abi.encode(name, symbol),
                 tokenSupplyParams_,
                 _tokenTaxParams
             );
     
             allTradingTokens.push(instance);
             return instance;

339:     function _createNewAgentVeToken(
             string memory name,
             string memory symbol,
             address stakingAsset,
             address founder,
             bool canStake
         ) internal returns (address instance) {
             instance = Clones.clone(veTokenImplementation);
             IAgentVeToken(instance).initialize(
                 name,
                 symbol,
                 founder,
                 stakingAsset,
                 block.timestamp + maturityDuration,
                 address(nft),
                 canStake
             );
     
             allTokens.push(instance);
             return instance;

543:     function _createPair(address tokenAddr) internal returns (address uniswapV2Pair_) {
             IUniswapV2Factory factory = IUniswapV2Factory(IUniswapV2Router02(_uniswapRouter).factory());
     
             require(factory.getPair(tokenAddr, assetToken) == address(0), "pool already exists");
     
             uniswapV2Pair_ = factory.createPair(tokenAddr, assetToken);
     
             return (uniswapV2Pair_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

138:     function _createNewDAO(
             string memory name,
             IVotes token,
             uint32 daoVotingPeriod,
             uint256 daoThreshold
         ) internal returns (address instance) {
             instance = Clones.clone(daoImplementation);
             IAgentDAO(instance).initialize(name, token, address(_nft), daoThreshold, daoVotingPeriod);
     
             return instance;

150:     function _createNewAgentVeToken(
             string memory name,
             string memory symbol,
             address stakingAsset,
             address founder,
             bool canStake
         ) internal returns (address instance) {
             instance = Clones.clone(veTokenImplementation);
             IAgentVeToken(instance).initialize(
                 name,
                 symbol,
                 founder,
                 stakingAsset,
                 block.timestamp + maturityDuration,
                 address(_nft),
                 canStake
             );
     
             return instance;

171:     function _createNewAgentToken(string memory name, string memory symbol) internal returns (address instance) {
             instance = Clones.clone(tokenImplementation);
             IAgentToken(instance).initialize(
                 [_tokenAdmin, _uniswapRouter, _assetToken],
                 abi.encode(name, symbol),
                 _tokenSupplyParams,
                 _tokenTaxParams
             );
     
             return instance;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

147:     /**
          * @dev function {_processTaxParams}
          *
          * Process provided tax params
          *
          * @param erc20TaxParameters_ The tax params
          */
         function _processTaxParams(ERC20TaxParameters memory erc20TaxParameters_) internal returns (bool tokenHasTax_) {
             /**
              * @dev If this
              * token does NOT have tax applied then there is no need to store or read these parameters, and we can
              * avoid this simply by checking the immutable var. Pass back the value for this var from this method.
              */
             if (erc20TaxParameters_.projectBuyTaxBasisPoints == 0 && erc20TaxParameters_.projectSellTaxBasisPoints == 0) {
                 return false;
             } else {
                 projectBuyTaxBasisPoints = uint16(erc20TaxParameters_.projectBuyTaxBasisPoints);
                 projectSellTaxBasisPoints = uint16(erc20TaxParameters_.projectSellTaxBasisPoints);
                 return true;

147:     /**
          * @dev function {_processTaxParams}
          *
          * Process provided tax params
          *
          * @param erc20TaxParameters_ The tax params
          */
         function _processTaxParams(ERC20TaxParameters memory erc20TaxParameters_) internal returns (bool tokenHasTax_) {
             /**
              * @dev If this
              * token does NOT have tax applied then there is no need to store or read these parameters, and we can
              * avoid this simply by checking the immutable var. Pass back the value for this var from this method.
              */
             if (erc20TaxParameters_.projectBuyTaxBasisPoints == 0 && erc20TaxParameters_.projectSellTaxBasisPoints == 0) {
                 return false;

186:     /**
          * @dev function {_createPair}
          *
          * Create the uniswap pair
          *
          * @return uniswapV2Pair_ The pair address
          */
         function _createPair() internal returns (address uniswapV2Pair_) {
             uniswapV2Pair_ = IUniswapV2Factory(_uniswapRouter.factory()).getPair(address(this), pairToken);
     
             if (uniswapV2Pair_ == address(0)) {
                 uniswapV2Pair_ = IUniswapV2Factory(_uniswapRouter.factory()).createPair(address(this), pairToken);
     
                 emit LiquidityPoolCreated(uniswapV2Pair_);
             }
     
             _liquidityPools.add(uniswapV2Pair_);
     
             return (uniswapV2Pair_);

282:     /**
          * @dev function {liquidityPools}
          *
          * Returns a list of all liquidity pools
          *
          * @return liquidityPools_ a list of all liquidity pools
          */
         function liquidityPools() external view returns (address[] memory liquidityPools_) {
             return (_liquidityPools.values());

339:     /**
          * @dev function {validCallers}
          *
          * Returns a list of all valid caller code hashes
          *
          * @return validCallerHashes_ a list of all valid caller code hashes
          */
         function validCallers() external view returns (bytes32[] memory validCallerHashes_) {
             return (_validCallerCodeHashes.values());

629:     /**
          * @dev function {_pretaxValidationAndLimits}
          *
          * Perform validation on pre-tax amounts
          *
          * @param from_ From address for the transaction
          * @param to_ To address for the transaction
          * @param amount_ Amount of the transaction
          */
         function _pretaxValidationAndLimits(
             address from_,
             address to_,
             uint256 amount_
         ) internal view returns (uint256 fromBalance_) {
             // This can't be a transfer to the liquidity pool before the funding date
             // UNLESS the from address is this contract. This ensures that the initial
             // LP funding transaction is from this contract using the supply of tokens
             // designated for the LP pool, and therefore the initial price in the pool
             // is being set as expected.
             //
             // This protects from, for example, tokens from a team minted supply being
             // paired with ETH and added to the pool, setting the initial price, BEFORE
             // the initial liquidity is added through this contract.
             if (to_ == uniswapV2Pair && from_ != address(this) && fundedDate == 0) {
                 revert InitialLiquidityNotYetAdded();
             }
     
             if (from_ == address(0)) {
                 revert TransferFromZeroAddress();
             }
     
             if (to_ == address(0)) {
                 revert TransferToZeroAddress();
             }
     
             fromBalance_ = _balances[from_];
     
             if (fromBalance_ < amount_) {
                 revert TransferAmountExceedsBalance();
             }
     
             return (fromBalance_);

673:     /**
          * @dev function {_taxProcessing}
          *
          * Perform tax processing
          *
          * @param applyTax_ Do we apply tax to this transaction?
          * @param to_ The reciever of the token
          * @param from_ The sender of the token
          * @param sentAmount_ The amount being send
          * @return amountLessTax_ The amount that will be recieved, i.e. the send amount minus tax
          */
         function _taxProcessing(
             bool applyTax_,
             address to_,
             address from_,
             uint256 sentAmount_
         ) internal returns (uint256 amountLessTax_) {
             amountLessTax_ = sentAmount_;
             unchecked {
                 if (_tokenHasTax && applyTax_ && !_autoSwapInProgress) {
                     uint256 tax;
     
                     // on sell
                     if (isLiquidityPool(to_) && totalSellTaxBasisPoints() > 0) {
                         if (projectSellTaxBasisPoints > 0) {
                             uint256 projectTax = ((sentAmount_ * projectSellTaxBasisPoints) / BP_DENOM);
                             projectTaxPendingSwap += uint128(projectTax);
                             tax += projectTax;
                         }
                     }
                     // on buy
                     else if (isLiquidityPool(from_) && totalBuyTaxBasisPoints() > 0) {
                         if (projectBuyTaxBasisPoints > 0) {
                             uint256 projectTax = ((sentAmount_ * projectBuyTaxBasisPoints) / BP_DENOM);
                             projectTaxPendingSwap += uint128(projectTax);
                             tax += projectTax;
                         }
                     }
     
                     if (tax > 0) {
                         _balances[address(this)] += tax;
                         emit Transfer(from_, address(this), tax);
                         amountLessTax_ -= tax;
                     }
                 }
             }
             return (amountLessTax_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

48:     /**
         * @dev Accessor to the internal vote counts.
         */
        function proposalVotes(
            uint256 proposalId
        ) public view virtual returns (uint256 againstVotes, uint256 forVotes, uint256 abstainVotes) {
            ProposalVote storage proposalVote = _proposalVotes[proposalId];
            return (proposalVote.againstVotes, proposalVote.forVotes, proposalVote.abstainVotes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

### <a name="NC-31"></a>[NC-31] `require()` / `revert()` statements should have descriptive reason strings

*Instances (1)*:

```solidity
File: contracts/fun/Bonding.sol

213:         require(approved);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

### <a name="NC-32"></a>[NC-32] Take advantage of Custom Error's return value property

An important feature of Custom Error is that values such as address, tokenID, msg.value can be written inside the () sign, this kind of approach provides a serious advantage in debugging and examining the revert details of dapps such as tenderly.

*Instances (25)*:

```solidity
File: contracts/governance/GovernorCountingSimple.sol

104:             revert GovernorInvalidVoteType();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/GovernorCountingSimple.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

30:                 revert CheckpointUnorderedInsertion();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

38:                 revert CheckpointUnorderedInsertion();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

35:                 revert CheckpointUnorderedInsertion();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

70:             revert CallerIsNotAdminNorFactory();

137:             revert SupplyTotalMismatch();

141:             revert MaxSupplyTooHigh();

229:             revert InitialLiquidityAlreadyAdded();

236:             revert NoTokenForLiquidityPair();

303:             revert LiquidityPoolCannotBeAddressZero();

307:             revert LiquidityPoolMustBeAContractAddress();

586:             revert AllowanceDecreasedBelowZero();

653:             revert InitialLiquidityNotYetAdded();

657:             revert TransferFromZeroAddress();

661:             revert TransferToZeroAddress();

667:             revert TransferAmountExceedsBalance();

871:             revert TransferFailed();

893:             revert CannotWithdrawThisToken();

909:             revert MintToZeroAddress();

937:             revert BurnFromTheZeroAddress();

944:             revert BurnExceedsBalance();

973:             revert ApproveFromTheZeroAddress();

977:             revert ApproveToTheZeroAddress();

996:                 revert InsufficientAllowance();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

106:             revert GovernorInvalidVoteType();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

### <a name="NC-33"></a>[NC-33] Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. `10**18`)

While this won't save gas in the recent solidity versions, this is shorter and more readable (this is especially true in calculations).

*Instances (1)*:

```solidity
File: contracts/token/Virtual.sol

13:     ) ERC20("Virtual Protocol", "VIRTUAL") ERC20Capped(1000000000 * 10 ** 18) Ownable(initialOwner) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

### <a name="NC-34"></a>[NC-34] Use scientific notation for readability reasons for large multiples of ten

The more a number has zeros, the harder it becomes to see with the eyes if it's the intended value. To ease auditing and bug bounty hunting, consider using the scientific notation

*Instances (2)*:

```solidity
File: contracts/token/Virtual.sol

13:     ) ERC20("Virtual Protocol", "VIRTUAL") ERC20Capped(1000000000 * 10 ** 18) Ownable(initialOwner) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

20:     uint256 internal constant ROUND_DEC = 100000000000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="NC-35"></a>[NC-35] Avoid the use of sensitive terms

Use [alternative variants](https://www.zdnet.com/article/mysql-drops-master-slave-and-blacklist-whitelist-terminology/), e.g. allowlist/denylist instead of whitelist/blacklist

*Instances (9)*:

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

50:     mapping(uint256 => bool) private _blacklists;

54:     event AgentBlacklisted(uint256 indexed virtualId, bool value);

230:     function isBlacklisted(uint256 virtualId) public view returns (bool) {

231:         return _blacklists[virtualId];

234:     function setBlacklist(uint256 virtualId, bool value) public onlyRole(ADMIN_ROLE) {

235:         _blacklists[virtualId] = value;

236:         emit AgentBlacklisted(virtualId, value);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

70:         require(!registry.isBlacklisted(virtualId), "Agent Blacklisted");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/IAgentNft.sol

57:     function isBlacklisted(uint256 virtualId) external view returns (bool);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentNft.sol)

### <a name="NC-36"></a>[NC-36] Contract does not follow the Solidity style guide's suggested layout ordering

The [style guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout) says that, within a contract, the ordering should be:

1) Type declarations
2) State variables
3) Events
4) Modifiers
5) Functions

However, the contract(s) below do not follow this ordering

*Instances (25)*:

```solidity
File: contracts/contribution/ContributionNft.sol

1: 
   Current order:
   VariableDeclaration.personaNft
   VariableDeclaration._contributionVirtualId
   VariableDeclaration._parents
   VariableDeclaration._children
   VariableDeclaration._cores
   VariableDeclaration.modelContributions
   VariableDeclaration.modelDatasets
   EventDefinition.NewContribution
   VariableDeclaration._admin
   VariableDeclaration._eloCalculator
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.tokenVirtualId
   FunctionDefinition.getAgentDAO
   FunctionDefinition.isAccepted
   FunctionDefinition.mint
   FunctionDefinition.getAdmin
   FunctionDefinition.setAdmin
   FunctionDefinition.tokenURI
   FunctionDefinition.getChildren
   FunctionDefinition.getParentId
   FunctionDefinition.getCore
   FunctionDefinition.supportsInterface
   FunctionDefinition._increaseBalance
   FunctionDefinition._update
   FunctionDefinition.isModel
   FunctionDefinition.ownerOf
   FunctionDefinition.getDatasetId
   FunctionDefinition.getEloCalculator
   FunctionDefinition.setEloCalculator
   
   Suggested order:
   VariableDeclaration.personaNft
   VariableDeclaration._contributionVirtualId
   VariableDeclaration._parents
   VariableDeclaration._children
   VariableDeclaration._cores
   VariableDeclaration.modelContributions
   VariableDeclaration.modelDatasets
   VariableDeclaration._admin
   VariableDeclaration._eloCalculator
   EventDefinition.NewContribution
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.tokenVirtualId
   FunctionDefinition.getAgentDAO
   FunctionDefinition.isAccepted
   FunctionDefinition.mint
   FunctionDefinition.getAdmin
   FunctionDefinition.setAdmin
   FunctionDefinition.tokenURI
   FunctionDefinition.getChildren
   FunctionDefinition.getParentId
   FunctionDefinition.getCore
   FunctionDefinition.supportsInterface
   FunctionDefinition._increaseBalance
   FunctionDefinition._update
   FunctionDefinition.isModel
   FunctionDefinition.ownerOf
   FunctionDefinition.getDatasetId
   FunctionDefinition.getEloCalculator
   FunctionDefinition.setEloCalculator

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/IServiceNft.sol

1: 
   Current order:
   FunctionDefinition.getCore
   FunctionDefinition.getMaturity
   FunctionDefinition.getImpact
   FunctionDefinition.getCoreService
   FunctionDefinition.getCoreDatasetAt
   FunctionDefinition.totalCoreDatasets
   FunctionDefinition.getCoreDatasets
   EventDefinition.CoreServiceUpdated
   EventDefinition.NewService
   EventDefinition.DatasetImpactUpdated
   EventDefinition.SetServiceScore
   
   Suggested order:
   EventDefinition.CoreServiceUpdated
   EventDefinition.NewService
   EventDefinition.DatasetImpactUpdated
   EventDefinition.SetServiceScore
   FunctionDefinition.getCore
   FunctionDefinition.getMaturity
   FunctionDefinition.getImpact
   FunctionDefinition.getCoreService
   FunctionDefinition.getCoreDatasetAt
   FunctionDefinition.totalCoreDatasets
   FunctionDefinition.getCoreDatasets

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/IServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration._feeTo
   VariableDeclaration.factory
   VariableDeclaration.router
   VariableDeclaration.initialSupply
   VariableDeclaration.fee
   VariableDeclaration.K
   VariableDeclaration.assetRate
   VariableDeclaration.gradThreshold
   VariableDeclaration.maxTx
   VariableDeclaration.agentFactory
   StructDefinition.Profile
   StructDefinition.Token
   StructDefinition.Data
   StructDefinition.DeployParams
   VariableDeclaration._deployParams
   VariableDeclaration.profile
   VariableDeclaration.profiles
   VariableDeclaration.tokenInfo
   VariableDeclaration.tokenInfos
   EventDefinition.Launched
   EventDefinition.Deployed
   EventDefinition.Graduated
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition._createUserProfile
   FunctionDefinition._checkIfProfileExists
   FunctionDefinition._approval
   FunctionDefinition.setInitialSupply
   FunctionDefinition.setGradThreshold
   FunctionDefinition.setFee
   FunctionDefinition.setMaxTx
   FunctionDefinition.setAssetRate
   FunctionDefinition.setDeployParams
   FunctionDefinition.getUserTokens
   FunctionDefinition.launch
   FunctionDefinition.launchFor
   FunctionDefinition.sell
   FunctionDefinition.buy
   FunctionDefinition._openTradingOnUniswap
   FunctionDefinition.unwrapToken
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration._feeTo
   VariableDeclaration.factory
   VariableDeclaration.router
   VariableDeclaration.initialSupply
   VariableDeclaration.fee
   VariableDeclaration.K
   VariableDeclaration.assetRate
   VariableDeclaration.gradThreshold
   VariableDeclaration.maxTx
   VariableDeclaration.agentFactory
   VariableDeclaration._deployParams
   VariableDeclaration.profile
   VariableDeclaration.profiles
   VariableDeclaration.tokenInfo
   VariableDeclaration.tokenInfos
   StructDefinition.Profile
   StructDefinition.Token
   StructDefinition.Data
   StructDefinition.DeployParams
   EventDefinition.Launched
   EventDefinition.Deployed
   EventDefinition.Graduated
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition._createUserProfile
   FunctionDefinition._checkIfProfileExists
   FunctionDefinition._approval
   FunctionDefinition.setInitialSupply
   FunctionDefinition.setGradThreshold
   FunctionDefinition.setFee
   FunctionDefinition.setMaxTx
   FunctionDefinition.setAssetRate
   FunctionDefinition.setDeployParams
   FunctionDefinition.getUserTokens
   FunctionDefinition.launch
   FunctionDefinition.launchFor
   FunctionDefinition.sell
   FunctionDefinition.buy
   FunctionDefinition._openTradingOnUniswap
   FunctionDefinition.unwrapToken

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FPair.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration.router
   VariableDeclaration.tokenA
   VariableDeclaration.tokenB
   StructDefinition.Pool
   VariableDeclaration._pool
   FunctionDefinition.constructor
   ModifierDefinition.onlyRouter
   EventDefinition.Mint
   EventDefinition.Swap
   FunctionDefinition.mint
   FunctionDefinition.swap
   FunctionDefinition.approval
   FunctionDefinition.transferAsset
   FunctionDefinition.transferTo
   FunctionDefinition.getReserves
   FunctionDefinition.kLast
   FunctionDefinition.priceALast
   FunctionDefinition.priceBLast
   FunctionDefinition.balance
   FunctionDefinition.assetBalance
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.router
   VariableDeclaration.tokenA
   VariableDeclaration.tokenB
   VariableDeclaration._pool
   StructDefinition.Pool
   EventDefinition.Mint
   EventDefinition.Swap
   ModifierDefinition.onlyRouter
   FunctionDefinition.constructor
   FunctionDefinition.mint
   FunctionDefinition.swap
   FunctionDefinition.approval
   FunctionDefinition.transferAsset
   FunctionDefinition.transferTo
   FunctionDefinition.getReserves
   FunctionDefinition.kLast
   FunctionDefinition.priceALast
   FunctionDefinition.priceBLast
   FunctionDefinition.balance
   FunctionDefinition.assetBalance

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/genesis/FGenesis.sol

1: 
   Current order:
   UsingForDirective.GenesisLib
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration.OPERATION_ROLE
   StructDefinition.Params
   VariableDeclaration.params
   VariableDeclaration.genesisContracts
   VariableDeclaration.genesisID
   EventDefinition.GenesisCreated
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.setParams
   FunctionDefinition._setParams
   FunctionDefinition.createGenesis
   FunctionDefinition._getGenesis
   FunctionDefinition.onGenesisSuccess
   FunctionDefinition.onGenesisFailed
   FunctionDefinition.withdrawLeftAssetsAfterFinalized
   FunctionDefinition.resetTime
   FunctionDefinition.cancelGenesis
   
   Suggested order:
   UsingForDirective.GenesisLib
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration.OPERATION_ROLE
   VariableDeclaration.params
   VariableDeclaration.genesisContracts
   VariableDeclaration.genesisID
   StructDefinition.Params
   EventDefinition.GenesisCreated
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.setParams
   FunctionDefinition._setParams
   FunctionDefinition.createGenesis
   FunctionDefinition._getGenesis
   FunctionDefinition.onGenesisSuccess
   FunctionDefinition.onGenesisFailed
   FunctionDefinition.withdrawLeftAssetsAfterFinalized
   FunctionDefinition.resetTime
   FunctionDefinition.cancelGenesis

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration.FACTORY_ROLE
   VariableDeclaration.mapAddrToVirtuals
   VariableDeclaration.claimableAgentTokens
   VariableDeclaration.participants
   VariableDeclaration.refundUserCountForFailed
   VariableDeclaration.genesisId
   VariableDeclaration.factory
   VariableDeclaration.startTime
   VariableDeclaration.endTime
   VariableDeclaration.genesisName
   VariableDeclaration.genesisTicker
   VariableDeclaration.genesisCores
   VariableDeclaration.tbaSalt
   VariableDeclaration.tbaImplementation
   VariableDeclaration.daoVotingPeriod
   VariableDeclaration.daoThreshold
   VariableDeclaration.agentFactoryAddress
   VariableDeclaration.virtualTokenAddress
   VariableDeclaration.reserveAmount
   VariableDeclaration.maxContributionVirtualAmount
   VariableDeclaration.agentTokenTotalSupply
   VariableDeclaration.agentTokenLpSupply
   VariableDeclaration.agentTokenAddress
   VariableDeclaration.isFailed
   VariableDeclaration.isCancelled
   EventDefinition.AssetsWithdrawn
   EventDefinition.TimeReset
   EventDefinition.GenesisCancelled
   EventDefinition.GenesisSucceeded
   EventDefinition.GenesisFailed
   EventDefinition.Participated
   EventDefinition.RefundClaimed
   EventDefinition.AgentTokenClaimed
   EventDefinition.VirtualsWithdrawn
   VariableDeclaration.ERR_NOT_STARTED
   VariableDeclaration.ERR_ALREADY_STARTED
   VariableDeclaration.ERR_NOT_ENDED
   VariableDeclaration.ERR_ALREADY_ENDED
   VariableDeclaration.ERR_ALREADY_FAILED
   VariableDeclaration.ERR_ALREADY_CANCELLED
   VariableDeclaration.ERR_START_TIME_FUTURE
   VariableDeclaration.ERR_END_AFTER_START
   VariableDeclaration.ERR_TOKEN_LAUNCHED
   VariableDeclaration.ERR_TOKEN_NOT_LAUNCHED
   ModifierDefinition.whenNotStarted
   ModifierDefinition.whenStarted
   ModifierDefinition.whenNotEnded
   ModifierDefinition.whenEnded
   ModifierDefinition.whenNotFailed
   ModifierDefinition.whenNotCancelled
   ModifierDefinition.whenTokenNotLaunched
   ModifierDefinition.whenTokenLaunched
   ModifierDefinition.whenActive
   ModifierDefinition.whenFinalized
   FunctionDefinition._validateTime
   FunctionDefinition.initialize
   FunctionDefinition.participate
   FunctionDefinition.onGenesisSuccess
   FunctionDefinition.claimAgentToken
   FunctionDefinition.getClaimableAgentToken
   FunctionDefinition.onGenesisFailed
   FunctionDefinition.isEnded
   FunctionDefinition.isStarted
   FunctionDefinition.getParticipantCount
   FunctionDefinition.getParticipantsPaginated
   StructDefinition.ParticipantInfo
   FunctionDefinition.getParticipantsInfo
   StructDefinition.GenesisInfo
   FunctionDefinition.getGenesisInfo
   FunctionDefinition.withdrawLeftAssetsAfterFinalized
   FunctionDefinition.resetTime
   FunctionDefinition.cancelGenesis
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.FACTORY_ROLE
   VariableDeclaration.mapAddrToVirtuals
   VariableDeclaration.claimableAgentTokens
   VariableDeclaration.participants
   VariableDeclaration.refundUserCountForFailed
   VariableDeclaration.genesisId
   VariableDeclaration.factory
   VariableDeclaration.startTime
   VariableDeclaration.endTime
   VariableDeclaration.genesisName
   VariableDeclaration.genesisTicker
   VariableDeclaration.genesisCores
   VariableDeclaration.tbaSalt
   VariableDeclaration.tbaImplementation
   VariableDeclaration.daoVotingPeriod
   VariableDeclaration.daoThreshold
   VariableDeclaration.agentFactoryAddress
   VariableDeclaration.virtualTokenAddress
   VariableDeclaration.reserveAmount
   VariableDeclaration.maxContributionVirtualAmount
   VariableDeclaration.agentTokenTotalSupply
   VariableDeclaration.agentTokenLpSupply
   VariableDeclaration.agentTokenAddress
   VariableDeclaration.isFailed
   VariableDeclaration.isCancelled
   VariableDeclaration.ERR_NOT_STARTED
   VariableDeclaration.ERR_ALREADY_STARTED
   VariableDeclaration.ERR_NOT_ENDED
   VariableDeclaration.ERR_ALREADY_ENDED
   VariableDeclaration.ERR_ALREADY_FAILED
   VariableDeclaration.ERR_ALREADY_CANCELLED
   VariableDeclaration.ERR_START_TIME_FUTURE
   VariableDeclaration.ERR_END_AFTER_START
   VariableDeclaration.ERR_TOKEN_LAUNCHED
   VariableDeclaration.ERR_TOKEN_NOT_LAUNCHED
   StructDefinition.ParticipantInfo
   StructDefinition.GenesisInfo
   EventDefinition.AssetsWithdrawn
   EventDefinition.TimeReset
   EventDefinition.GenesisCancelled
   EventDefinition.GenesisSucceeded
   EventDefinition.GenesisFailed
   EventDefinition.Participated
   EventDefinition.RefundClaimed
   EventDefinition.AgentTokenClaimed
   EventDefinition.VirtualsWithdrawn
   ModifierDefinition.whenNotStarted
   ModifierDefinition.whenStarted
   ModifierDefinition.whenNotEnded
   ModifierDefinition.whenEnded
   ModifierDefinition.whenNotFailed
   ModifierDefinition.whenNotCancelled
   ModifierDefinition.whenTokenNotLaunched
   ModifierDefinition.whenTokenLaunched
   ModifierDefinition.whenActive
   ModifierDefinition.whenFinalized
   FunctionDefinition._validateTime
   FunctionDefinition.initialize
   FunctionDefinition.participate
   FunctionDefinition.onGenesisSuccess
   FunctionDefinition.claimAgentToken
   FunctionDefinition.getClaimableAgentToken
   FunctionDefinition.onGenesisFailed
   FunctionDefinition.isEnded
   FunctionDefinition.isStarted
   FunctionDefinition.getParticipantCount
   FunctionDefinition.getParticipantsPaginated
   FunctionDefinition.getParticipantsInfo
   FunctionDefinition.getGenesisInfo
   FunctionDefinition.withdrawLeftAssetsAfterFinalized
   FunctionDefinition.resetTime
   FunctionDefinition.cancelGenesis

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/GovernorCountingSimple.sol

1: 
   Current order:
   EnumDefinition.VoteType
   StructDefinition.ProposalVote
   VariableDeclaration._proposalVotes
   FunctionDefinition.COUNTING_MODE
   FunctionDefinition.hasVoted
   FunctionDefinition.proposalVotes
   FunctionDefinition._quorumReached
   FunctionDefinition._voteSucceeded
   FunctionDefinition._countVote
   
   Suggested order:
   VariableDeclaration._proposalVotes
   EnumDefinition.VoteType
   StructDefinition.ProposalVote
   FunctionDefinition.COUNTING_MODE
   FunctionDefinition.hasVoted
   FunctionDefinition.proposalVotes
   FunctionDefinition._quorumReached
   FunctionDefinition._voteSucceeded
   FunctionDefinition._countVote

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/GovernorCountingSimple.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

1: 
   Current order:
   UsingForDirective.Checkpoints.Trace224
   VariableDeclaration._earlyExecutions
   VariableDeclaration._quorumCheckpoints
   VariableDeclaration._quorum
   EventDefinition.QuorumUpdated
   VariableDeclaration.EXECUTOR_ROLE
   FunctionDefinition.constructor
   FunctionDefinition.votingDelay
   FunctionDefinition.votingPeriod
   FunctionDefinition.proposalThreshold
   FunctionDefinition.propose
   FunctionDefinition._propose
   FunctionDefinition.quorum
   FunctionDefinition.earlyExecute
   FunctionDefinition.state
   FunctionDefinition.updateQuorum
   FunctionDefinition.supportsInterface
   
   Suggested order:
   UsingForDirective.Checkpoints.Trace224
   VariableDeclaration._earlyExecutions
   VariableDeclaration._quorumCheckpoints
   VariableDeclaration._quorum
   VariableDeclaration.EXECUTOR_ROLE
   EventDefinition.QuorumUpdated
   FunctionDefinition.constructor
   FunctionDefinition.votingDelay
   FunctionDefinition.votingPeriod
   FunctionDefinition.proposalThreshold
   FunctionDefinition.propose
   FunctionDefinition._propose
   FunctionDefinition.quorum
   FunctionDefinition.earlyExecute
   FunctionDefinition.state
   FunctionDefinition.updateQuorum
   FunctionDefinition.supportsInterface

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

1: 
   Current order:
   ErrorDefinition.CheckpointUnorderedInsertion
   StructDefinition.Trace
   StructDefinition.Checkpoint
   FunctionDefinition.push
   FunctionDefinition._insert
   FunctionDefinition.latest
   FunctionDefinition.upperLookupRecent
   FunctionDefinition._upperBinaryLookup
   
   Suggested order:
   StructDefinition.Trace
   StructDefinition.Checkpoint
   ErrorDefinition.CheckpointUnorderedInsertion
   FunctionDefinition.push
   FunctionDefinition._insert
   FunctionDefinition.latest
   FunctionDefinition.upperLookupRecent
   FunctionDefinition._upperBinaryLookup

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

1: 
   Current order:
   ErrorDefinition.CheckpointUnorderedInsertion
   StructDefinition.Trace
   StructDefinition.RewardSettings
   StructDefinition.Checkpoint
   FunctionDefinition.push
   FunctionDefinition._insert
   FunctionDefinition.latest
   FunctionDefinition.upperLookupRecent
   FunctionDefinition._upperBinaryLookup
   
   Suggested order:
   StructDefinition.Trace
   StructDefinition.RewardSettings
   StructDefinition.Checkpoint
   ErrorDefinition.CheckpointUnorderedInsertion
   FunctionDefinition.push
   FunctionDefinition._insert
   FunctionDefinition.latest
   FunctionDefinition.upperLookupRecent
   FunctionDefinition._upperBinaryLookup

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

1: 
   Current order:
   ErrorDefinition.CheckpointUnorderedInsertion
   StructDefinition.Trace
   StructDefinition.RewardSettings
   StructDefinition.Checkpoint
   FunctionDefinition.push
   FunctionDefinition._insert
   FunctionDefinition.latest
   FunctionDefinition.upperLookupRecent
   FunctionDefinition._upperBinaryLookup
   
   Suggested order:
   StructDefinition.Trace
   StructDefinition.RewardSettings
   StructDefinition.Checkpoint
   ErrorDefinition.CheckpointUnorderedInsertion
   FunctionDefinition.push
   FunctionDefinition._insert
   FunctionDefinition.latest
   FunctionDefinition.upperLookupRecent
   FunctionDefinition._upperBinaryLookup

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

1: 
   Current order:
   StructDefinition.Route
   FunctionDefinition.swapExactTokensForTokens
   FunctionDefinition.getAmountsOut
   UsingForDirective.IERC20
   VariableDeclaration.router
   VariableDeclaration.tokenIn
   VariableDeclaration.tokenOut
   VariableDeclaration.factory
   FunctionDefinition.constructor
   FunctionDefinition.swapExactTokensForTokens
   FunctionDefinition.getAmountsOut
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.router
   VariableDeclaration.tokenIn
   VariableDeclaration.tokenOut
   VariableDeclaration.factory
   StructDefinition.Route
   FunctionDefinition.swapExactTokensForTokens
   FunctionDefinition.getAmountsOut
   FunctionDefinition.constructor
   FunctionDefinition.swapExactTokensForTokens
   FunctionDefinition.getAmountsOut

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

1: 
   Current order:
   EventDefinition.Approval
   EventDefinition.Transfer
   FunctionDefinition.name
   FunctionDefinition.symbol
   FunctionDefinition.decimals
   FunctionDefinition.totalSupply
   FunctionDefinition.balanceOf
   FunctionDefinition.allowance
   FunctionDefinition.approve
   FunctionDefinition.transfer
   FunctionDefinition.transferFrom
   FunctionDefinition.DOMAIN_SEPARATOR
   FunctionDefinition.PERMIT_TYPEHASH
   FunctionDefinition.nonces
   FunctionDefinition.permit
   EventDefinition.Mint
   EventDefinition.Burn
   EventDefinition.Swap
   EventDefinition.Sync
   FunctionDefinition.MINIMUM_LIQUIDITY
   FunctionDefinition.factory
   FunctionDefinition.token0
   FunctionDefinition.token1
   FunctionDefinition.getReserves
   FunctionDefinition.price0CumulativeLast
   FunctionDefinition.price1CumulativeLast
   FunctionDefinition.kLast
   FunctionDefinition.mint
   FunctionDefinition.burn
   FunctionDefinition.swap
   FunctionDefinition.skim
   FunctionDefinition.sync
   FunctionDefinition.initialize
   
   Suggested order:
   EventDefinition.Approval
   EventDefinition.Transfer
   EventDefinition.Mint
   EventDefinition.Burn
   EventDefinition.Swap
   EventDefinition.Sync
   FunctionDefinition.name
   FunctionDefinition.symbol
   FunctionDefinition.decimals
   FunctionDefinition.totalSupply
   FunctionDefinition.balanceOf
   FunctionDefinition.allowance
   FunctionDefinition.approve
   FunctionDefinition.transfer
   FunctionDefinition.transferFrom
   FunctionDefinition.DOMAIN_SEPARATOR
   FunctionDefinition.PERMIT_TYPEHASH
   FunctionDefinition.nonces
   FunctionDefinition.permit
   FunctionDefinition.MINIMUM_LIQUIDITY
   FunctionDefinition.factory
   FunctionDefinition.token0
   FunctionDefinition.token1
   FunctionDefinition.getReserves
   FunctionDefinition.price0CumulativeLast
   FunctionDefinition.price1CumulativeLast
   FunctionDefinition.kLast
   FunctionDefinition.mint
   FunctionDefinition.burn
   FunctionDefinition.swap
   FunctionDefinition.skim
   FunctionDefinition.sync
   FunctionDefinition.initialize

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/tax/AgentTax.sol

1: 
   Current order:
   UsingForDirective.IERC20
   StructDefinition.TaxHistory
   StructDefinition.TaxAmounts
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration.EXECUTOR_ROLE
   VariableDeclaration.DENOM
   VariableDeclaration.assetToken
   VariableDeclaration.taxToken
   VariableDeclaration.router
   VariableDeclaration.treasury
   VariableDeclaration.feeRate
   VariableDeclaration.minSwapThreshold
   VariableDeclaration.maxSwapThreshold
   VariableDeclaration.agentNft
   EventDefinition.SwapThresholdUpdated
   EventDefinition.TreasuryUpdated
   EventDefinition.SwapExecuted
   EventDefinition.SwapFailed
   EventDefinition.TaxCollected
   VariableDeclaration._agentTba
   VariableDeclaration.taxHistory
   VariableDeclaration.agentTaxAmounts
   ErrorDefinition.TxHashExists
   StructDefinition.TaxRecipient
   EventDefinition.SwapParamsUpdated2
   VariableDeclaration._agentRecipients
   VariableDeclaration.creatorFeeRate
   EventDefinition.CreatorUpdated
   VariableDeclaration.tbaBonus
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.updateSwapParams
   FunctionDefinition.updateSwapThresholds
   FunctionDefinition.updateTreasury
   FunctionDefinition.withdraw
   FunctionDefinition.handleAgentTaxes
   FunctionDefinition._getTaxRecipient
   FunctionDefinition._swapForAsset
   FunctionDefinition.updateCreator
   FunctionDefinition.dcaSell
   FunctionDefinition.updateTbaBonus
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration.EXECUTOR_ROLE
   VariableDeclaration.DENOM
   VariableDeclaration.assetToken
   VariableDeclaration.taxToken
   VariableDeclaration.router
   VariableDeclaration.treasury
   VariableDeclaration.feeRate
   VariableDeclaration.minSwapThreshold
   VariableDeclaration.maxSwapThreshold
   VariableDeclaration.agentNft
   VariableDeclaration._agentTba
   VariableDeclaration.taxHistory
   VariableDeclaration.agentTaxAmounts
   VariableDeclaration._agentRecipients
   VariableDeclaration.creatorFeeRate
   VariableDeclaration.tbaBonus
   StructDefinition.TaxHistory
   StructDefinition.TaxAmounts
   StructDefinition.TaxRecipient
   ErrorDefinition.TxHashExists
   EventDefinition.SwapThresholdUpdated
   EventDefinition.TreasuryUpdated
   EventDefinition.SwapExecuted
   EventDefinition.SwapFailed
   EventDefinition.TaxCollected
   EventDefinition.SwapParamsUpdated2
   EventDefinition.CreatorUpdated
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.updateSwapParams
   FunctionDefinition.updateSwapThresholds
   FunctionDefinition.updateTreasury
   FunctionDefinition.withdraw
   FunctionDefinition.handleAgentTaxes
   FunctionDefinition._getTaxRecipient
   FunctionDefinition._swapForAsset
   FunctionDefinition.updateCreator
   FunctionDefinition.dcaSell
   FunctionDefinition.updateTbaBonus

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration.assetToken
   VariableDeclaration.taxToken
   VariableDeclaration.router
   VariableDeclaration.bondingRouter
   VariableDeclaration.treasury
   VariableDeclaration.minSwapThreshold
   VariableDeclaration.maxSwapThreshold
   VariableDeclaration._slippage
   EventDefinition.SwapParamsUpdated
   EventDefinition.SwapThresholdUpdated
   EventDefinition.TreasuryUpdated
   EventDefinition.SwapExecuted
   EventDefinition.SwapFailed
   FunctionDefinition.constructor
   ModifierDefinition.onlyBondingRouter
   FunctionDefinition.initialize
   FunctionDefinition.updateSwapParams
   FunctionDefinition.updateSwapThresholds
   FunctionDefinition.updateTreasury
   FunctionDefinition.withdraw
   FunctionDefinition.swapForAsset
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration.assetToken
   VariableDeclaration.taxToken
   VariableDeclaration.router
   VariableDeclaration.bondingRouter
   VariableDeclaration.treasury
   VariableDeclaration.minSwapThreshold
   VariableDeclaration.maxSwapThreshold
   VariableDeclaration._slippage
   EventDefinition.SwapParamsUpdated
   EventDefinition.SwapThresholdUpdated
   EventDefinition.TreasuryUpdated
   EventDefinition.SwapExecuted
   EventDefinition.SwapFailed
   ModifierDefinition.onlyBondingRouter
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.updateSwapParams
   FunctionDefinition.updateSwapThresholds
   FunctionDefinition.updateTreasury
   FunctionDefinition.withdraw
   FunctionDefinition.swapForAsset

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration.EXECUTOR_ROLE
   VariableDeclaration.taxToken
   EventDefinition.TaxRefunded
   VariableDeclaration.refunds
   ErrorDefinition.TxHashExists
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.withdraw
   FunctionDefinition.refund
   FunctionDefinition.manualRefund
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration.EXECUTOR_ROLE
   VariableDeclaration.taxToken
   VariableDeclaration.refunds
   ErrorDefinition.TxHashExists
   EventDefinition.TaxRefunded
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.withdraw
   FunctionDefinition.refund
   FunctionDefinition.manualRefund

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

1: 
   Current order:
   UsingForDirective.Checkpoints.Trace208
   VariableDeclaration._scores
   VariableDeclaration._proposalMaturities
   ErrorDefinition.ERC5805FutureLookup
   VariableDeclaration._totalScore
   VariableDeclaration._agentNft
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.votingDelay
   FunctionDefinition.votingPeriod
   FunctionDefinition.proposalThreshold
   FunctionDefinition.propose
   FunctionDefinition._propose
   FunctionDefinition.proposalCount
   FunctionDefinition.scoreOf
   FunctionDefinition.getPastScore
   FunctionDefinition._castVote
   FunctionDefinition._tryAutoExecute
   FunctionDefinition._updateMaturity
   FunctionDefinition._calcMaturity
   FunctionDefinition.getMaturity
   FunctionDefinition.quorum
   FunctionDefinition.quorumDenominator
   FunctionDefinition.state
   FunctionDefinition.totalScore
   
   Suggested order:
   UsingForDirective.Checkpoints.Trace208
   VariableDeclaration._scores
   VariableDeclaration._proposalMaturities
   VariableDeclaration._totalScore
   VariableDeclaration._agentNft
   ErrorDefinition.ERC5805FutureLookup
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.votingDelay
   FunctionDefinition.votingPeriod
   FunctionDefinition.proposalThreshold
   FunctionDefinition.propose
   FunctionDefinition._propose
   FunctionDefinition.proposalCount
   FunctionDefinition.scoreOf
   FunctionDefinition.getPastScore
   FunctionDefinition._castVote
   FunctionDefinition._tryAutoExecute
   FunctionDefinition._updateMaturity
   FunctionDefinition._calcMaturity
   FunctionDefinition.getMaturity
   FunctionDefinition.quorum
   FunctionDefinition.quorumDenominator
   FunctionDefinition.state
   FunctionDefinition.totalScore

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration._nextId
   VariableDeclaration.tokenImplementation
   VariableDeclaration.daoImplementation
   VariableDeclaration.nft
   VariableDeclaration.tbaRegistry
   VariableDeclaration.applicationThreshold
   VariableDeclaration.allTokens
   VariableDeclaration.allDAOs
   VariableDeclaration.assetToken
   VariableDeclaration.maturityDuration
   VariableDeclaration.WITHDRAW_ROLE
   EventDefinition.NewPersona
   EventDefinition.NewApplication
   EnumDefinition.ApplicationStatus
   StructDefinition.Application
   VariableDeclaration._applications
   VariableDeclaration.gov
   ModifierDefinition.onlyGov
   EventDefinition.ApplicationThresholdUpdated
   EventDefinition.GovUpdated
   EventDefinition.ImplContractsUpdated
   VariableDeclaration._vault
   VariableDeclaration.locked
   ModifierDefinition.noReentrant
   VariableDeclaration.allTradingTokens
   VariableDeclaration._uniswapRouter
   VariableDeclaration.veTokenImplementation
   VariableDeclaration._minter
   VariableDeclaration._tokenAdmin
   VariableDeclaration.defaultDelegatee
   VariableDeclaration._tokenSupplyParams
   VariableDeclaration._tokenTaxParams
   VariableDeclaration._tokenMultiplier
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.getApplication
   FunctionDefinition.proposeAgent
   FunctionDefinition.withdraw
   FunctionDefinition.executeApplication
   FunctionDefinition._createNewDAO
   FunctionDefinition._createNewAgentToken
   FunctionDefinition._createNewAgentVeToken
   FunctionDefinition.totalAgents
   FunctionDefinition.setApplicationThreshold
   FunctionDefinition.setVault
   FunctionDefinition.setImplementations
   FunctionDefinition.setMaturityDuration
   FunctionDefinition.setUniswapRouter
   FunctionDefinition.setTokenAdmin
   FunctionDefinition.setTokenSupplyParams
   FunctionDefinition.setTokenTaxParams
   FunctionDefinition.setAssetToken
   FunctionDefinition.pause
   FunctionDefinition.unpause
   FunctionDefinition._msgSender
   FunctionDefinition._msgData
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration._nextId
   VariableDeclaration.tokenImplementation
   VariableDeclaration.daoImplementation
   VariableDeclaration.nft
   VariableDeclaration.tbaRegistry
   VariableDeclaration.applicationThreshold
   VariableDeclaration.allTokens
   VariableDeclaration.allDAOs
   VariableDeclaration.assetToken
   VariableDeclaration.maturityDuration
   VariableDeclaration.WITHDRAW_ROLE
   VariableDeclaration._applications
   VariableDeclaration.gov
   VariableDeclaration._vault
   VariableDeclaration.locked
   VariableDeclaration.allTradingTokens
   VariableDeclaration._uniswapRouter
   VariableDeclaration.veTokenImplementation
   VariableDeclaration._minter
   VariableDeclaration._tokenAdmin
   VariableDeclaration.defaultDelegatee
   VariableDeclaration._tokenSupplyParams
   VariableDeclaration._tokenTaxParams
   VariableDeclaration._tokenMultiplier
   EnumDefinition.ApplicationStatus
   StructDefinition.Application
   EventDefinition.NewPersona
   EventDefinition.NewApplication
   EventDefinition.ApplicationThresholdUpdated
   EventDefinition.GovUpdated
   EventDefinition.ImplContractsUpdated
   ModifierDefinition.onlyGov
   ModifierDefinition.noReentrant
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.getApplication
   FunctionDefinition.proposeAgent
   FunctionDefinition.withdraw
   FunctionDefinition.executeApplication
   FunctionDefinition._createNewDAO
   FunctionDefinition._createNewAgentToken
   FunctionDefinition._createNewAgentVeToken
   FunctionDefinition.totalAgents
   FunctionDefinition.setApplicationThreshold
   FunctionDefinition.setVault
   FunctionDefinition.setImplementations
   FunctionDefinition.setMaturityDuration
   FunctionDefinition.setUniswapRouter
   FunctionDefinition.setTokenAdmin
   FunctionDefinition.setTokenSupplyParams
   FunctionDefinition.setTokenTaxParams
   FunctionDefinition.setAssetToken
   FunctionDefinition.pause
   FunctionDefinition.unpause
   FunctionDefinition._msgSender
   FunctionDefinition._msgData

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration._nextId
   VariableDeclaration.tokenImplementation
   VariableDeclaration.daoImplementation
   VariableDeclaration.nft
   VariableDeclaration.tbaRegistry
   VariableDeclaration.applicationThreshold
   VariableDeclaration.allTokens
   VariableDeclaration.allDAOs
   VariableDeclaration.assetToken
   VariableDeclaration.maturityDuration
   VariableDeclaration.WITHDRAW_ROLE
   EventDefinition.NewPersona
   EventDefinition.NewApplication
   EnumDefinition.ApplicationStatus
   StructDefinition.Application
   VariableDeclaration._applications
   VariableDeclaration.gov
   ModifierDefinition.onlyGov
   EventDefinition.ApplicationThresholdUpdated
   EventDefinition.GovUpdated
   EventDefinition.ImplContractsUpdated
   VariableDeclaration._vault
   VariableDeclaration.locked
   ModifierDefinition.noReentrant
   VariableDeclaration.allTradingTokens
   VariableDeclaration._uniswapRouter
   VariableDeclaration.veTokenImplementation
   VariableDeclaration._minter
   VariableDeclaration._tokenAdmin
   VariableDeclaration.defaultDelegatee
   VariableDeclaration._tokenSupplyParams
   VariableDeclaration._tokenTaxParams
   VariableDeclaration._tokenMultiplier
   VariableDeclaration.BONDING_ROLE
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.getApplication
   FunctionDefinition.proposeAgent
   FunctionDefinition.withdraw
   FunctionDefinition._executeApplication
   FunctionDefinition.executeApplication
   FunctionDefinition._createNewDAO
   FunctionDefinition._createNewAgentToken
   FunctionDefinition._createNewAgentVeToken
   FunctionDefinition.totalAgents
   FunctionDefinition.setApplicationThreshold
   FunctionDefinition.setVault
   FunctionDefinition.setImplementations
   FunctionDefinition.setMaturityDuration
   FunctionDefinition.setUniswapRouter
   FunctionDefinition.setTokenAdmin
   FunctionDefinition.setTokenSupplyParams
   FunctionDefinition.setTokenTaxParams
   FunctionDefinition.setAssetToken
   FunctionDefinition.pause
   FunctionDefinition.unpause
   FunctionDefinition._msgSender
   FunctionDefinition._msgData
   FunctionDefinition.initFromBondingCurve
   FunctionDefinition.executeBondingCurveApplication
   FunctionDefinition.setDefaultDelegatee
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration._nextId
   VariableDeclaration.tokenImplementation
   VariableDeclaration.daoImplementation
   VariableDeclaration.nft
   VariableDeclaration.tbaRegistry
   VariableDeclaration.applicationThreshold
   VariableDeclaration.allTokens
   VariableDeclaration.allDAOs
   VariableDeclaration.assetToken
   VariableDeclaration.maturityDuration
   VariableDeclaration.WITHDRAW_ROLE
   VariableDeclaration._applications
   VariableDeclaration.gov
   VariableDeclaration._vault
   VariableDeclaration.locked
   VariableDeclaration.allTradingTokens
   VariableDeclaration._uniswapRouter
   VariableDeclaration.veTokenImplementation
   VariableDeclaration._minter
   VariableDeclaration._tokenAdmin
   VariableDeclaration.defaultDelegatee
   VariableDeclaration._tokenSupplyParams
   VariableDeclaration._tokenTaxParams
   VariableDeclaration._tokenMultiplier
   VariableDeclaration.BONDING_ROLE
   EnumDefinition.ApplicationStatus
   StructDefinition.Application
   EventDefinition.NewPersona
   EventDefinition.NewApplication
   EventDefinition.ApplicationThresholdUpdated
   EventDefinition.GovUpdated
   EventDefinition.ImplContractsUpdated
   ModifierDefinition.onlyGov
   ModifierDefinition.noReentrant
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.getApplication
   FunctionDefinition.proposeAgent
   FunctionDefinition.withdraw
   FunctionDefinition._executeApplication
   FunctionDefinition.executeApplication
   FunctionDefinition._createNewDAO
   FunctionDefinition._createNewAgentToken
   FunctionDefinition._createNewAgentVeToken
   FunctionDefinition.totalAgents
   FunctionDefinition.setApplicationThreshold
   FunctionDefinition.setVault
   FunctionDefinition.setImplementations
   FunctionDefinition.setMaturityDuration
   FunctionDefinition.setUniswapRouter
   FunctionDefinition.setTokenAdmin
   FunctionDefinition.setTokenSupplyParams
   FunctionDefinition.setTokenTaxParams
   FunctionDefinition.setAssetToken
   FunctionDefinition.pause
   FunctionDefinition.unpause
   FunctionDefinition._msgSender
   FunctionDefinition._msgData
   FunctionDefinition.initFromBondingCurve
   FunctionDefinition.executeBondingCurveApplication
   FunctionDefinition.setDefaultDelegatee

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration._nextId
   VariableDeclaration.tokenImplementation
   VariableDeclaration.daoImplementation
   VariableDeclaration.nft
   VariableDeclaration.tbaRegistry
   VariableDeclaration.applicationThreshold
   VariableDeclaration.allTokens
   VariableDeclaration.allDAOs
   VariableDeclaration.assetToken
   VariableDeclaration.maturityDuration
   VariableDeclaration.WITHDRAW_ROLE
   EventDefinition.NewPersona
   EventDefinition.NewApplication
   EnumDefinition.ApplicationStatus
   StructDefinition.Application
   VariableDeclaration._applications
   EventDefinition.ApplicationThresholdUpdated
   EventDefinition.GovUpdated
   EventDefinition.ImplContractsUpdated
   VariableDeclaration._vault
   VariableDeclaration.locked
   ModifierDefinition.noReentrant
   VariableDeclaration.allTradingTokens
   VariableDeclaration._uniswapRouter
   VariableDeclaration.veTokenImplementation
   VariableDeclaration._tokenAdmin
   VariableDeclaration.defaultDelegatee
   VariableDeclaration._tokenSupplyParams
   VariableDeclaration._tokenTaxParams
   VariableDeclaration._tokenApplication
   VariableDeclaration._applicationToken
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.getApplication
   FunctionDefinition.proposeAgent
   FunctionDefinition.withdraw
   FunctionDefinition._executeApplication
   FunctionDefinition.executeApplication
   FunctionDefinition._createNewDAO
   FunctionDefinition._createNewAgentToken
   FunctionDefinition._createNewAgentVeToken
   FunctionDefinition.totalAgents
   FunctionDefinition.setApplicationThreshold
   FunctionDefinition.setVault
   FunctionDefinition.setImplementations
   FunctionDefinition.setMaturityDuration
   FunctionDefinition.setUniswapRouter
   FunctionDefinition.setTokenAdmin
   FunctionDefinition.setTokenSupplyParams
   FunctionDefinition.setTokenTaxParams
   FunctionDefinition.setAssetToken
   FunctionDefinition.pause
   FunctionDefinition.unpause
   FunctionDefinition._msgSender
   FunctionDefinition._msgData
   FunctionDefinition.setDefaultDelegatee
   FunctionDefinition.initFromToken
   FunctionDefinition.executeTokenApplication
   FunctionDefinition.isCompatibleToken
   FunctionDefinition._createPair
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration._nextId
   VariableDeclaration.tokenImplementation
   VariableDeclaration.daoImplementation
   VariableDeclaration.nft
   VariableDeclaration.tbaRegistry
   VariableDeclaration.applicationThreshold
   VariableDeclaration.allTokens
   VariableDeclaration.allDAOs
   VariableDeclaration.assetToken
   VariableDeclaration.maturityDuration
   VariableDeclaration.WITHDRAW_ROLE
   VariableDeclaration._applications
   VariableDeclaration._vault
   VariableDeclaration.locked
   VariableDeclaration.allTradingTokens
   VariableDeclaration._uniswapRouter
   VariableDeclaration.veTokenImplementation
   VariableDeclaration._tokenAdmin
   VariableDeclaration.defaultDelegatee
   VariableDeclaration._tokenSupplyParams
   VariableDeclaration._tokenTaxParams
   VariableDeclaration._tokenApplication
   VariableDeclaration._applicationToken
   EnumDefinition.ApplicationStatus
   StructDefinition.Application
   EventDefinition.NewPersona
   EventDefinition.NewApplication
   EventDefinition.ApplicationThresholdUpdated
   EventDefinition.GovUpdated
   EventDefinition.ImplContractsUpdated
   ModifierDefinition.noReentrant
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.getApplication
   FunctionDefinition.proposeAgent
   FunctionDefinition.withdraw
   FunctionDefinition._executeApplication
   FunctionDefinition.executeApplication
   FunctionDefinition._createNewDAO
   FunctionDefinition._createNewAgentToken
   FunctionDefinition._createNewAgentVeToken
   FunctionDefinition.totalAgents
   FunctionDefinition.setApplicationThreshold
   FunctionDefinition.setVault
   FunctionDefinition.setImplementations
   FunctionDefinition.setMaturityDuration
   FunctionDefinition.setUniswapRouter
   FunctionDefinition.setTokenAdmin
   FunctionDefinition.setTokenSupplyParams
   FunctionDefinition.setTokenTaxParams
   FunctionDefinition.setAssetToken
   FunctionDefinition.pause
   FunctionDefinition.unpause
   FunctionDefinition._msgSender
   FunctionDefinition._msgData
   FunctionDefinition.setDefaultDelegatee
   FunctionDefinition.initFromToken
   FunctionDefinition.executeTokenApplication
   FunctionDefinition.isCompatibleToken
   FunctionDefinition._createPair

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

1: 
   Current order:
   VariableDeclaration._nextVirtualId
   VariableDeclaration._stakingTokenToVirtualId
   VariableDeclaration.MINTER_ROLE
   VariableDeclaration.VALIDATOR_ADMIN_ROLE
   ModifierDefinition.onlyVirtualDAO
   ModifierDefinition.onlyService
   VariableDeclaration.virtualInfos
   VariableDeclaration._contributionNft
   VariableDeclaration._serviceNft
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration._blacklists
   VariableDeclaration.virtualLPs
   VariableDeclaration._eloCalculator
   EventDefinition.AgentBlacklisted
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.setContributionService
   FunctionDefinition.nextVirtualId
   FunctionDefinition.mint
   FunctionDefinition.addCoreType
   FunctionDefinition.virtualInfo
   FunctionDefinition.virtualLP
   FunctionDefinition.stakingTokenToVirtualId
   FunctionDefinition.addValidator
   FunctionDefinition._validatorScoreOf
   FunctionDefinition._getPastValidatorScore
   FunctionDefinition.totalProposals
   FunctionDefinition.setCoreTypes
   FunctionDefinition.setTokenURI
   FunctionDefinition.setTBA
   FunctionDefinition.setDAO
   FunctionDefinition.totalStaked
   FunctionDefinition.getVotes
   FunctionDefinition.getContributionNft
   FunctionDefinition.getServiceNft
   FunctionDefinition.getAllServices
   FunctionDefinition.tokenURI
   FunctionDefinition.supportsInterface
   FunctionDefinition.totalSupply
   FunctionDefinition.isBlacklisted
   FunctionDefinition.setBlacklist
   FunctionDefinition.migrateScoreFunctions
   FunctionDefinition.setEloCalculator
   FunctionDefinition.getEloCalculator
   FunctionDefinition.migrateVirtual
   
   Suggested order:
   VariableDeclaration._nextVirtualId
   VariableDeclaration._stakingTokenToVirtualId
   VariableDeclaration.MINTER_ROLE
   VariableDeclaration.VALIDATOR_ADMIN_ROLE
   VariableDeclaration.virtualInfos
   VariableDeclaration._contributionNft
   VariableDeclaration._serviceNft
   VariableDeclaration.ADMIN_ROLE
   VariableDeclaration._blacklists
   VariableDeclaration.virtualLPs
   VariableDeclaration._eloCalculator
   EventDefinition.AgentBlacklisted
   ModifierDefinition.onlyVirtualDAO
   ModifierDefinition.onlyService
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.setContributionService
   FunctionDefinition.nextVirtualId
   FunctionDefinition.mint
   FunctionDefinition.addCoreType
   FunctionDefinition.virtualInfo
   FunctionDefinition.virtualLP
   FunctionDefinition.stakingTokenToVirtualId
   FunctionDefinition.addValidator
   FunctionDefinition._validatorScoreOf
   FunctionDefinition._getPastValidatorScore
   FunctionDefinition.totalProposals
   FunctionDefinition.setCoreTypes
   FunctionDefinition.setTokenURI
   FunctionDefinition.setTBA
   FunctionDefinition.setDAO
   FunctionDefinition.totalStaked
   FunctionDefinition.getVotes
   FunctionDefinition.getContributionNft
   FunctionDefinition.getServiceNft
   FunctionDefinition.getAllServices
   FunctionDefinition.tokenURI
   FunctionDefinition.supportsInterface
   FunctionDefinition.totalSupply
   FunctionDefinition.isBlacklisted
   FunctionDefinition.setBlacklist
   FunctionDefinition.migrateScoreFunctions
   FunctionDefinition.setEloCalculator
   FunctionDefinition.getEloCalculator
   FunctionDefinition.migrateVirtual

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

1: 
   Current order:
   UsingForDirective.IERC20
   UsingForDirective.Checkpoints.Trace208
   VariableDeclaration.founder
   VariableDeclaration.assetToken
   VariableDeclaration.agentNft
   VariableDeclaration.matureAt
   VariableDeclaration.canStake
   VariableDeclaration.initialLock
   FunctionDefinition.constructor
   VariableDeclaration._balanceCheckpoints
   VariableDeclaration.locked
   ModifierDefinition.noReentrant
   FunctionDefinition.initialize
   FunctionDefinition.stake
   FunctionDefinition.setCanStake
   FunctionDefinition.setMatureAt
   FunctionDefinition.withdraw
   FunctionDefinition.getPastBalanceOf
   FunctionDefinition.transfer
   FunctionDefinition.transferFrom
   FunctionDefinition.approve
   FunctionDefinition._update
   FunctionDefinition.getPastDelegates
   
   Suggested order:
   UsingForDirective.IERC20
   UsingForDirective.Checkpoints.Trace208
   VariableDeclaration.founder
   VariableDeclaration.assetToken
   VariableDeclaration.agentNft
   VariableDeclaration.matureAt
   VariableDeclaration.canStake
   VariableDeclaration.initialLock
   VariableDeclaration._balanceCheckpoints
   VariableDeclaration.locked
   ModifierDefinition.noReentrant
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.stake
   FunctionDefinition.setCanStake
   FunctionDefinition.setMatureAt
   FunctionDefinition.withdraw
   FunctionDefinition.getPastBalanceOf
   FunctionDefinition.transfer
   FunctionDefinition.transferFrom
   FunctionDefinition.approve
   FunctionDefinition._update
   FunctionDefinition.getPastDelegates

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

1: 
   Current order:
   EnumDefinition.VoteType
   StructDefinition.ProposalVote
   VariableDeclaration._proposalVotes
   FunctionDefinition.__GovernorCountingSimple_init
   FunctionDefinition.COUNTING_MODE
   FunctionDefinition.hasVoted
   FunctionDefinition.proposalVotes
   FunctionDefinition._quorumReached
   FunctionDefinition._voteSucceeded
   FunctionDefinition._countVote
   
   Suggested order:
   VariableDeclaration._proposalVotes
   EnumDefinition.VoteType
   StructDefinition.ProposalVote
   FunctionDefinition.__GovernorCountingSimple_init
   FunctionDefinition.COUNTING_MODE
   FunctionDefinition.hasVoted
   FunctionDefinition.proposalVotes
   FunctionDefinition._quorumReached
   FunctionDefinition._voteSucceeded
   FunctionDefinition._countVote

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

```solidity
File: contracts/virtualPersona/IAgentDAO.sol

1: 
   Current order:
   FunctionDefinition.initialize
   FunctionDefinition.proposalCount
   FunctionDefinition.scoreOf
   FunctionDefinition.totalScore
   FunctionDefinition.getPastScore
   FunctionDefinition.getMaturity
   EventDefinition.ValidatorEloRating
   
   Suggested order:
   EventDefinition.ValidatorEloRating
   FunctionDefinition.initialize
   FunctionDefinition.proposalCount
   FunctionDefinition.scoreOf
   FunctionDefinition.totalScore
   FunctionDefinition.getPastScore
   FunctionDefinition.getMaturity

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentDAO.sol)

```solidity
File: contracts/virtualPersona/IAgentNft.sol

1: 
   Current order:
   StructDefinition.VirtualInfo
   EventDefinition.CoresUpdated
   StructDefinition.VirtualLP
   FunctionDefinition.mint
   FunctionDefinition.stakingTokenToVirtualId
   FunctionDefinition.setTBA
   FunctionDefinition.virtualInfo
   FunctionDefinition.virtualLP
   FunctionDefinition.totalSupply
   FunctionDefinition.totalStaked
   FunctionDefinition.getVotes
   FunctionDefinition.totalProposals
   FunctionDefinition.getContributionNft
   FunctionDefinition.getServiceNft
   FunctionDefinition.getAllServices
   FunctionDefinition.nextVirtualId
   FunctionDefinition.isBlacklisted
   FunctionDefinition.getEloCalculator
   
   Suggested order:
   StructDefinition.VirtualInfo
   StructDefinition.VirtualLP
   EventDefinition.CoresUpdated
   FunctionDefinition.mint
   FunctionDefinition.stakingTokenToVirtualId
   FunctionDefinition.setTBA
   FunctionDefinition.virtualInfo
   FunctionDefinition.virtualLP
   FunctionDefinition.totalSupply
   FunctionDefinition.totalStaked
   FunctionDefinition.getVotes
   FunctionDefinition.totalProposals
   FunctionDefinition.getContributionNft
   FunctionDefinition.getServiceNft
   FunctionDefinition.getAllServices
   FunctionDefinition.nextVirtualId
   FunctionDefinition.isBlacklisted
   FunctionDefinition.getEloCalculator

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentNft.sol)

### <a name="NC-37"></a>[NC-37] Use Underscores for Number Literals (add an underscore every 3 digits)

*Instances (27)*:

```solidity
File: contracts/contribution/ServiceNft.sol

101:             _impacts[datasetId] = (rawImpact * datasetImpactWeight) / 10000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

111:         fee = (fee_ * 1 ether) / 1000;

215:         uint256 k = ((K * 10000) / assetRate);

216:         uint256 liquidity = (((k * 10000 ether) / supply) * 1 ether) / 10000;

299:         uint256 volume = duration > 86400 ? amount1Out : tokenInfo[tokenAddress].data.volume24H + amount1Out;

300:         uint256 prevPrice = duration > 86400

311:         if (duration > 86400) {

336:         uint256 volume = duration > 86400 ? amount1In : tokenInfo[tokenAddress].data.volume24H + amount1In;

337:         uint256 _price = duration > 86400 ? tokenInfo[tokenAddress].data.price : tokenInfo[tokenAddress].data.prevPrice;

346:         if (duration > 86400) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

43:         _quorumCheckpoints.push(0, 10000e18);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/VirtualProtocolDAO.sol

70:         return 10000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualProtocolDAO.sol)

```solidity
File: contracts/libs/Elo.sol

39:         require(ratingDiff < 1126, "Rating difference too large");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

197:             z := shr(18, mul(z, add(y, 65536))) // A mul() is saved from starting z at 181.

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/tax/AgentTax.sol

28:     uint256 internal constant DENOM = 10000;

109:         creatorFeeRate = 3000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

143:         uint256 minOutput = (expectedOutput * (10000 - _slippage)) / 10000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/TBABonus.sol

16:     uint256 internal constant DENOM = 10000;

41:         bonusRate = 3500;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

207:         return 10000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

19:     uint256 internal constant BP_DENOM = 10000;

20:     uint256 internal constant ROUND_DEC = 100000000000;

21:     uint256 internal constant CALL_GAS_LIMIT = 50000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

40:         uint256 eloA = 1000;

41:         uint256 eloB = 1000;

54:         return currentRating + eloA - 1000;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/IErrors.sol

105:     error IncorrectConfirmationValue(); //                    You need to enter the right confirmation value to call this funtion (usually 69420).

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IErrors.sol)

### <a name="NC-38"></a>[NC-38] Internal and private variables and functions names should begin with an underscore

According to the Solidity Style Guide, Non-`external` variable and function names should begin with an [underscore](https://docs.soliditylang.org/en/latest/style-guide.html#underscore-prefix-for-non-external-functions-and-variables)

*Instances (30)*:

```solidity
File: contracts/fun/FERC20.sol

24:     mapping(address => bool) private isExcludedFromMaxTx;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/genesis/GenesisLib.sol

9:     function validateAndDeploy(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/GenesisLib.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

19:     function push(Trace storage self, uint48 key, address value) internal {

43:     function latest(Trace storage self) internal view returns (address) {

48:     function upperLookupRecent(Trace storage self, uint48 key) internal view returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/Elo.sol

9:     function sixteenthRoot(uint256 x) internal pure returns (uint256) {

20:     function ratingChange(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

16:     function mulWadDown(uint256 x, uint256 y) internal pure returns (uint256) {

20:     function mulWadUp(uint256 x, uint256 y) internal pure returns (uint256) {

24:     function divWadDown(uint256 x, uint256 y) internal pure returns (uint256) {

28:     function divWadUp(uint256 x, uint256 y) internal pure returns (uint256) {

36:     function mulDivDown(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 z) {

49:     function mulDivUp(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 z) {

63:     function rpow(uint256 x, uint256 n, uint256 scalar) internal pure returns (uint256 z) {

152:     function sqrt(uint256 x) internal pure returns (uint256 z) {

217:     function unsafeMod(uint256 x, uint256 y) internal pure returns (uint256 z) {

226:     function unsafeDiv(uint256 x, uint256 y) internal pure returns (uint256 r) {

235:     function unsafeDivUp(uint256 x, uint256 y) internal pure returns (uint256 z) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

27:     function push(Trace storage self, uint32 key, RewardSettings memory value) internal {

51:     function latest(Trace storage self) internal view returns (RewardSettings memory) {

56:     function upperLookupRecent(Trace storage self, uint32 key) internal view returns (RewardSettings memory) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

24:     function push(Trace storage self, uint32 key, RewardSettings memory value) internal {

48:     function latest(Trace storage self) internal view returns (RewardSettings memory) {

53:     function upperLookupRecent(Trace storage self, uint32 key) internal view returns (RewardSettings memory) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

77:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

77:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

72:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

31:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

30:     bool internal locked;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

23:     function mapBattleResultToGameResult(uint8 result) internal pure returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="NC-39"></a>[NC-39] Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

*Instances (75)*:

```solidity
File: contracts/AgentInference.sol

22:     event Prompt(address indexed sender, bytes32 promptHash, uint256 agentId, uint256 cost, uint8[] coreIds);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

31:     event NewContribution(uint256 tokenId, uint256 virtualId, uint256 parentId, uint256 datasetId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/IServiceNft.sol

19:     event CoreServiceUpdated(uint256 virtualId, uint8 coreType, uint256 serviceId);

21:     event NewService(uint256 tokenId, uint8 coreId, uint256 maturity, uint256 impact, bool isModel);

23:     event DatasetImpactUpdated(uint16 weight);

25:     event SetServiceScore(uint256 serviceId, uint256 eloRating, uint256 impact);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/IServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

84:     event Launched(address indexed token, address indexed pair, uint);

85:     event Deployed(address indexed token, uint256 amount0, uint256 amount1);

86:     event Graduated(address indexed token, address agentToken);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

26:     event MaxTxUpdated(uint _maxTx);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

24:     event PairCreated(address indexed tokenA, address indexed tokenB, address pair, uint);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

41:     event Mint(uint256 reserve0, uint256 reserve1);

43:     event Swap(uint256 amount0In, uint256 amount0Out, uint256 amount1In, uint256 amount1Out);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/genesis/Genesis.sol

49:     event AssetsWithdrawn(uint256 indexed genesisID, address indexed to, address token, uint256 amount);

51:     event TimeReset(uint256 oldStartTime, uint256 oldEndTime, uint256 newStartTime, uint256 newEndTime);

57:     event Participated(uint256 indexed genesisID, address indexed user, uint256 point, uint256 virtuals);

58:     event RefundClaimed(uint256 indexed genesisID, address indexed user, uint256 amount);

59:     event AgentTokenClaimed(uint256 indexed genesisID, address indexed user, uint256 amount);

60:     event VirtualsWithdrawn(uint256 indexed genesisID, address indexed to, address token, uint256 amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

29:     event QuorumUpdated(uint224 oldQuorum, uint224 newQuorum);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/pool/IUniswapV2Factory.sol

4:     event PairCreated(address indexed token0, address indexed token1, address pair, uint);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Factory.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

4:     event Approval(address indexed owner, address indexed spender, uint value);

5:     event Transfer(address indexed from, address indexed to, uint value);

24:     event Mint(address indexed sender, uint amount0, uint amount1);

25:     event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);

26:     event Swap(

34:     event Sync(uint112 reserve0, uint112 reserve1);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/tax/AgentTax.sol

39:     event SwapThresholdUpdated(

45:     event TreasuryUpdated(address oldTreasury, address newTreasury);

46:     event SwapExecuted(uint256 indexed agentId, uint256 taxTokenAmount, uint256 assetTokenAmount);

47:     event SwapFailed(uint256 indexed agentId, uint256 taxTokenAmount);

48:     event TaxCollected(bytes32 indexed txhash, uint256 agentId, uint256 amount);

60:     event SwapParamsUpdated2(

74:     event CreatorUpdated(uint256 agentId, address oldCreator, address newCreator);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

26:     event SwapParamsUpdated(

34:     event SwapThresholdUpdated(

40:     event TreasuryUpdated(address oldTreasury, address newTreasury);

41:     event SwapExecuted(uint256 taxTokenAmount, uint256 assetTokenAmount);

42:     event SwapFailed(uint256 taxTokenAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

16:     event TaxRefunded(bytes32 indexed txhash, address recipient, uint256 amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

24:     event BonusRateUpdated(uint16 oldBonusRate, uint16 newBonusRate);

25:     event AllowanceUpdated(uint256 agentId, uint256 newAllowance);

26:     event PaidAgent(uint256 agentId, uint256 amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

37:     event NewPersona(uint256 virtualId, address token, address dao, address tba, address veToken, address lp);

38:     event NewApplication(uint256 id);

71:     event ApplicationThresholdUpdated(uint256 newThreshold);

72:     event GovUpdated(address newGov);

73:     event ImplContractsUpdated(address token, address dao);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

37:     event NewPersona(uint256 virtualId, address token, address dao, address tba, address veToken, address lp);

38:     event NewApplication(uint256 id);

71:     event ApplicationThresholdUpdated(uint256 newThreshold);

72:     event GovUpdated(address newGov);

73:     event ImplContractsUpdated(address token, address dao);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

39:     event NewPersona(uint256 virtualId, address token, address dao, address tba, address veToken, address lp);

40:     event NewApplication(uint256 id);

66:     event ApplicationThresholdUpdated(uint256 newThreshold);

67:     event GovUpdated(address newGov);

68:     event ImplContractsUpdated(address token, address dao);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

33:     event AgentMigrated(uint256 virtualId, address dao, address token, address lp, address veToken);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

54:     event AgentBlacklisted(uint256 indexed virtualId, bool value);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

10:     event NewCoreType(uint8 coreType, string label);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/IAgentDAO.sol

26:     event ValidatorEloRating(uint256 proposalId, address voter, uint256 score, uint8[] votes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentDAO.sol)

```solidity
File: contracts/virtualPersona/IAgentNft.sol

15:     event CoresUpdated(uint256 virtualId, uint8[] coreTypes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentNft.sol)

```solidity
File: contracts/virtualPersona/IAgentToken.sol

10:     event AutoSwapThresholdUpdated(uint256 oldThreshold, uint256 newThreshold);

12:     event ExternalCallError(uint256 identifier);

14:     event InitialLiquidityAdded(uint256 tokenA, uint256 tokenB, uint256 lpToken);

16:     event LimitsUpdated(

23:     event LiquidityPoolCreated(address addedPool);

25:     event LiquidityPoolAdded(address addedPool);

27:     event LiquidityPoolRemoved(address removedPool);

29:     event ProjectTaxBasisPointsChanged(

38:     event ProjectTaxRecipientUpdated(address treasury);

40:     event ValidCallerAdded(bytes32 addedValidCaller);

42:     event ValidCallerRemoved(bytes32 removedValidCaller);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentToken.sol)

```solidity
File: contracts/virtualPersona/IValidatorRegistry.sol

5:     event NewValidator(uint256 virtualId, address account);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IValidatorRegistry.sol)

### <a name="NC-40"></a>[NC-40] Constants should be defined rather than using magic numbers

*Instances (6)*:

```solidity
File: contracts/libs/FixedPointMathLib.sol

166:                 z := shl(64, z)

169:                 y := shr(64, y)

174:                 z := shl(16, z)

177:                 y := shr(16, y)

197:             z := shr(18, mul(z, add(y, 65536))) // A mul() is saved from starting z at 181.

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

51:         __GovernorVotesQuorumFraction_init(5100);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

### <a name="NC-41"></a>[NC-41] `public` functions not called by the contract should be declared `external` instead

*Instances (178)*:

```solidity
File: contracts/AgentInference.sol

35:     function prompt(

62:     function promptMulti(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

42:     function initialize(address thePersonaAddress) public initializer {

51:     function tokenVirtualId(uint256 tokenId) public view returns (uint256) {

59:     function isAccepted(uint256 tokenId) public view returns (bool) {

103:     function setAdmin(address newAdmin) public {

116:     function getChildren(uint256 tokenId) public view returns (uint256[] memory) {

120:     function getParentId(uint256 tokenId) public view returns (uint256) {

124:     function getCore(uint256 tokenId) public view returns (uint8) {

149:     function isModel(uint256 tokenId) public view returns (bool) {

165:     function setEloCalculator(address eloCalculator_) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

44:     function initialize(

58:     function mint(uint256 virtualId, bytes32 descHash) public returns (uint256) {

110:     function getCore(uint256 tokenId) public view returns (uint8) {

115:     function getMaturity(uint256 tokenId) public view returns (uint256) {

120:     function getImpact(uint256 tokenId) public view returns (uint256) {

125:     function getCoreService(uint256 virtualId, uint8 coreType) public view returns (uint256) {

129:     function getCoreDatasetAt(uint256 virtualId, uint8 coreType, uint256 index) public view returns (uint256) {

133:     function totalCoreDatasets(uint256 virtualId, uint8 coreType) public view returns (uint256) {

137:     function getCoreDatasets(uint256 virtualId, uint8 coreType) public view returns (uint256[] memory) {

141:     function setDatasetImpactWeight(uint16 weight) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

143:     function setInitialSupply(uint256 newSupply) public onlyOwner {

147:     function setGradThreshold(uint256 newThreshold) public onlyOwner {

151:     function setFee(uint256 newFee, address newFeeTo) public onlyOwner {

156:     function setMaxTx(uint256 maxTx_) public onlyOwner {

160:     function setAssetRate(uint256 newRate) public onlyOwner {

166:     function setDeployParams(DeployParams memory params) public onlyOwner {

170:     function getUserTokens(address account) public view returns (address[] memory) {

178:     function launch(

281:     function sell(uint256 amountIn, address tokenAddress) public returns (bool) {

318:     function buy(uint256 amountIn, address tokenAddress) public payable returns (bool) {

405:     function unwrapToken(address srcTokenAddress, address[] memory accounts) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

46:     function name() public view returns (string memory) {

50:     function symbol() public view returns (string memory) {

54:     function decimals() public pure returns (uint8) {

121:     function updateMaxTx(uint256 _maxTx) public onlyOwner {

125:     function excludeFromMaxTx(address user) public onlyOwner {

136:     function burnFrom(address user, uint256 amount) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

66:     function getPair(address tokenA, address tokenB) public view returns (address) {

70:     function allPairsLength() public view returns (uint) {

74:     function setTaxParams(address newVault_, uint256 buyTax_, uint256 sellTax_) public onlyRole(ADMIN_ROLE) {

82:     function setRouter(address router_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

45:     function mint(uint256 reserve0, uint256 reserve1) public onlyRouter returns (bool) {

55:     function swap(

71:     function approval(address _user, address _token, uint256 amount) public onlyRouter returns (bool) {

82:     function transferAsset(address recipient, uint256 amount) public onlyRouter {

88:     function transferTo(address recipient, uint256 amount) public onlyRouter {

94:     function getReserves() public view returns (uint256, uint256) {

98:     function kLast() public view returns (uint256) {

102:     function priceALast() public view returns (uint256) {

106:     function priceBLast() public view returns (uint256) {

110:     function balance() public view returns (uint256) {

114:     function assetBalance() public view returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

75:     function addInitialLiquidity(

95:     function sell(

131:     function buy(

165:     function graduate(address tokenAddress) public onlyRole(EXECUTOR_ROLE) nonReentrant {

172:     function approval(

183:     function setTaxManager(address newManager) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/Genesis.sol

422:     function getGenesisInfo() public view returns (GenesisInfo memory) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

21:     function initialize(

48:     function proposeAgent(

61:     function withdraw(uint256) public pure {

65:     function totalAgents() public pure returns (uint256) {

69:     function initFromBondingCurve(

83:     function executeBondingCurveApplication(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

95:     function earlyExecute(uint256 proposalId) public payable onlyRole(EXECUTOR_ROLE) returns (uint256) {

128:     function updateQuorum(uint224 newQuorum) public onlyGovernance {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/tax/AgentTax.sol

115:     function updateSwapParams(

147:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

157:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

168:     function handleAgentTaxes(

255:     function updateCreator(uint256 agentId, address creator) public {

269:     function dcaSell(uint256[] memory agentIds, uint256 slippage, uint256 maxOverride) public onlyRole(EXECUTOR_ROLE) {

287:     function updateTbaBonus(address tbaBonus_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

80:     function updateSwapParams(

101:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

111:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

122:     function swapForAsset() public onlyBondingRouter returns (bool, uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

40:     function refund(

62:     function manualRefund(bytes32 txhash, address recipient, uint256 amount) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

44:     function updateBonusRate(uint16 bonusRate_) public onlyRole(ADMIN_ROLE) {

50:     function setAllowances(uint256[] memory agentIds, uint256[] memory allowances) public onlyRole(ADMIN_ROLE) {

64:     function distributeBonus(uint256 agentId, address recipient, uint256 amount) public {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

195:     function getMaturity(uint256 proposalId) public view returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

108:     function initialize(

132:     function getApplication(uint256 proposalId) public view returns (Application memory) {

136:     function proposeAgent(

179:     function withdraw(uint256 id) public noReentrant {

196:     function executeApplication(uint256 id, bool canStake) public noReentrant {

323:     function totalAgents() public view returns (uint256) {

327:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

332:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

336:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

342:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

354:     function setTokenSupplyParams(

374:     function setTokenTaxParams(

388:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

396:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

110:     function initialize(

135:     function getApplication(uint256 proposalId) public view returns (Application memory) {

139:     function proposeAgent(

182:     function withdraw(uint256 id) public noReentrant {

268:     function executeApplication(uint256 id, bool canStake) public noReentrant {

337:     function totalAgents() public view returns (uint256) {

341:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

356:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

360:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

364:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

368:     function setTokenSupplyParams(

388:     function setTokenTaxParams(

402:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

406:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

410:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

422:     function initFromBondingCurve(

466:     function executeBondingCurveApplication(

489:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

108:     function initialize(

133:     function getApplication(uint256 proposalId) public view returns (Application memory) {

137:     function proposeAgent(

180:     function withdraw(uint256 id) public noReentrant {

292:     function executeApplication(uint256 id, bool canStake) public noReentrant {

361:     function totalAgents() public view returns (uint256) {

365:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

370:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

374:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

380:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

384:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function setTokenSupplyParams(

412:     function setTokenTaxParams(

426:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

430:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

434:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

446:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

451:     function initFromToken(

505:     function executeTokenApplication(uint256 id, bool canStake) public noReentrant {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

60:     function setTokenSupplyParams(

80:     function setTokenTaxParams(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

61:     function initialize(address defaultAdmin) public initializer {

81:     function nextVirtualId() public view returns (uint256) {

116:     function addCoreType(string memory label) public onlyRole(DEFAULT_ADMIN_ROLE) {

120:     function virtualInfo(uint256 virtualId) public view returns (VirtualInfo memory) {

124:     function virtualLP(uint256 virtualId) public view returns (VirtualLP memory) {

133:     function addValidator(uint256 virtualId, address validator) public {

157:     function totalProposals(uint256 virtualId) public view returns (uint256) {

169:     function setTokenURI(uint256 virtualId, string memory newTokenURI) public onlyVirtualDAO(virtualId) {

179:     function setDAO(uint256 virtualId, address newDAO) public {

185:     function totalStaked(uint256 virtualId) public view returns (uint256) {

189:     function getVotes(uint256 virtualId, address validator) public view returns (uint256) {

193:     function getContributionNft() public view returns (address) {

197:     function getServiceNft() public view returns (address) {

201:     function getAllServices(uint256 virtualId) public view returns (uint256[] memory) {

226:     function totalSupply() public view returns (uint256) {

230:     function isBlacklisted(uint256 virtualId) public view returns (bool) {

234:     function setBlacklist(uint256 virtualId, bool value) public onlyRole(ADMIN_ROLE) {

239:     function migrateScoreFunctions() public onlyRole(ADMIN_ROLE) {

243:     function setEloCalculator(address eloCalculator) public onlyRole(ADMIN_ROLE) {

247:     function getEloCalculator() public view returns (address) {

251:     function migrateVirtual(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

300:     function addLiquidityPool(address newLiquidityPool_) public onlyOwnerOrFactory {

335:     function isValidCaller(bytes32 queryHash_) public view returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

59:     function stake(uint256 amount, address receiver, address delegatee) public {

84:     function setCanStake(bool _canStake) public {

89:     function setMatureAt(uint256 _matureAt) public {

95:     function withdraw(uint256 amount) public noReentrant {

109:     function getPastBalanceOf(address account, uint256 timepoint) public view returns (uint256) {

139:     function getPastDelegates(address account, uint256 timepoint) public view returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

18:     function initialize(address initialOwner) public initializer {

39:     function battleElo(uint256 currentRating, uint8[] memory battles) public view returns (uint256) {

57:     function setK(uint256 k_) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

26:     function isValidator(uint256 virtualId, address account) public view returns (bool) {

60:     function totalUptimeScore(uint256 virtualId) public view returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="NC-42"></a>[NC-42] Variables need not be initialized to zero

The default value for variables is zero, so initializing them to zero is superfluous.

*Instances (33)*:

```solidity
File: contracts/AgentInference.sol

42:         uint256 total = 0;

46:         for (uint256 i = 0; i < amounts.length; i++) {

52:         for (uint256 i = 0; i < agentIds.length; i++) {

69:         uint256 total = 0;

74:         for (uint256 i = 0; i < len; i++) {

80:         uint256 prevAgentId = 0;

81:         address agentTba = address(0);

82:         for (uint256 i = 0; i < len; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/Bonding.sol

412:         for (uint i = 0; i < accounts.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/Genesis.sol

223:         uint256 totalRefundAmount = 0;

224:         for (uint256 i = 0; i < refundVirtualsTokenUserAmounts.length; i++) {

275:         uint256 totalDistributionAmount = 0;

276:         for (uint256 i = 0; i < distributeAgentTokenUserAmounts.length; i++) {

286:         for (uint256 i = 0; i < refundVirtualsTokenUserAddresses.length; i++) {

298:         for (uint256 i = 0; i < distributeAgentTokenUserAddresses.length; i++) {

327:         for (uint256 i = 0; i < participantIndexes.length; i++) {

370:         for (uint256 i = 0; i < actualPageSize; i++) {

388:         for (uint256 i = 0; i < length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

21:         for (uint256 i = 0; i < froms.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

51:         uint256 low = 0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

59:         uint256 low = 0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

56:         uint256 low = 0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/tax/AgentTax.sol

176:         uint256 totalAmount = 0;

177:         for (uint i = 0; i < txhashes.length; i++) {

272:         for (uint i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

46:         uint256 total = 0;

47:         for (uint i = 0; i < txhashes.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

53:         for (uint256 i = 0; i < agentIds.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

218:         address lp = address(0);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

206:         for (uint256 i = 0; i < total; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

42:         for (uint256 i = 0; i < battles.length; i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

61:         uint256 totalScore = 0;

62:         for (uint256 i = 0; i < validatorCount(virtualId); i++) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

## Low Issues

| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | `approve()`/`safeApprove()` may revert if the current approval is not zero | 6 |
| [L-2](#L-2) | Use a 2-step ownership transfer pattern | 6 |
| [L-3](#L-3) | Some tokens may revert when zero value transfers are made | 46 |
| [L-4](#L-4) | Missing checks for `address(0)` when assigning values to address state variables | 85 |
| [L-5](#L-5) | `decimals()` is not a part of the ERC-20 standard | 2 |
| [L-6](#L-6) | Deprecated approve() function | 6 |
| [L-7](#L-7) | Division by zero not prevented | 18 |
| [L-8](#L-8) | `domainSeparator()` isn't protected against replay attacks in case of a future chain split  | 1 |
| [L-9](#L-9) | Duplicate import statements | 10 |
| [L-10](#L-10) | Empty Function Body - Consider commenting why | 2 |
| [L-11](#L-11) | Empty `receive()/payable fallback()` function does not authenticate requests | 1 |
| [L-12](#L-12) | External call recipient may consume all transaction gas | 1 |
| [L-13](#L-13) | Initializers could be front-run | 92 |
| [L-14](#L-14) | Signature use at deadlines should be allowed | 1 |
| [L-15](#L-15) | Prevent accidentally burning tokens | 12 |
| [L-16](#L-16) | NFT ownership doesn't support hard forks | 4 |
| [L-17](#L-17) | Owner can renounce while system is paused | 2 |
| [L-18](#L-18) | Possible rounding issue | 10 |
| [L-19](#L-19) | Loss of precision | 9 |
| [L-20](#L-20) | Solidity version 0.8.20+ may not work on other chains due to `PUSH0` | 38 |
| [L-21](#L-21) | Use `Ownable2Step.transferOwnership` instead of `Ownable.transferOwnership` | 8 |
| [L-22](#L-22) | File allows a version of solidity that is susceptible to an assembly optimizer bug | 2 |
| [L-23](#L-23) | `symbol()` is not a part of the ERC-20 standard | 2 |
| [L-24](#L-24) | Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when downcasting | 9 |
| [L-25](#L-25) | Unsafe ERC20 operation(s) | 12 |
| [L-26](#L-26) | Unspecific compiler version pragma | 5 |
| [L-27](#L-27) | Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions | 126 |
| [L-28](#L-28) | Upgradeable contract not initialized | 238 |

### <a name="L-1"></a>[L-1] `approve()`/`safeApprove()` may revert if the current approval is not zero

- Some tokens (like the *very popular* USDT) do not work when changing the allowance from an existing non-zero allowance value (it will revert if the current approval is not zero to protect against front-running changes of approvals). These tokens must first be approved for zero and then the actual allowance can be approved.
- Furthermore, OZ's implementation of safeApprove would throw an error if an approve is attempted from a non-zero value (`"SafeERC20: approve from non-zero to non-zero allowance"`)

Set the allowance to zero immediately before each of the existing allowance calls

*Instances (6)*:

```solidity
File: contracts/genesis/Genesis.sol

246:             IERC20(virtualTokenAddress).approve(agentFactoryAddress, reserveAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

269:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

262:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

286:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

130:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

244:         IERC20(pairToken).approve(address(_uniswapRouter), type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="L-2"></a>[L-2] Use a 2-step ownership transfer pattern

Recommend considering implementing a two step process where the owner or admin nominates an account and the nominated account needs to call an `acceptOwnership()` function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account. Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

*Instances (6)*:

```solidity
File: contracts/fun/Bonding.sol

18: contract Bonding is Initializable, ReentrancyGuardUpgradeable, OwnableUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

7: contract FERC20 is Context, IERC20, Ownable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

9: contract veVirtualToken is ERC20, ERC20Permit, ERC20Votes, Ownable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/token/Virtual.sol

9: contract VirtualToken is ERC20Capped, Ownable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

15: contract AgentMigrator is Ownable, Pausable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

10: contract EloCalculator is IEloCalculator, Initializable, OwnableUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="L-3"></a>[L-3] Some tokens may revert when zero value transfers are made

Example: <https://github.com/d-xo/weird-erc20#revert-on-zero-value-transfers>.

In spite of the fact that EIP-20 [states](https://github.com/ethereum/EIPs/blob/46b9b698815abbfa628cd1097311deee77dd45c5/EIPS/eip-20.md?plain=1#L116) that zero-valued transfers must be accepted, some tokens, such as LEND will revert if this is attempted, which may cause transactions that involve other tokens (such as batch operations) to fully revert. Consider skipping the transfer if the amount is zero, which will also save gas.

*Instances (46)*:

```solidity
File: contracts/AgentInference.sol

55:             token.safeTransferFrom(sender, agentTba, amounts[i]);

87:             token.safeTransferFrom(sender, agentTba, amounts[i]);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/fun/Bonding.sol

204:         IERC20(assetToken).safeTransferFrom(msg.sender, _feeTo, fee);

205:         IERC20(assetToken).safeTransferFrom(msg.sender, address(this), initialPurchase);

276:         token.transfer(msg.sender, token.balanceOf(address(this)));

417:                 agentToken.transferFrom(pairAddress, acc, balance);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FPair.sol

85:         IERC20(tokenB).safeTransfer(recipient, amount);

91:         IERC20(tokenA).safeTransfer(recipient, amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

88:         token.safeTransferFrom(msg.sender, pairAddress, amountToken_);

111:         token.safeTransferFrom(to, pairAddress, amountIn);

148:         IERC20(assetToken).safeTransferFrom(to, pair, amount);

150:         IERC20(assetToken).safeTransferFrom(to, feeTo, txFee);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

78:             IERC20(params.virtualToken).transferFrom(msg.sender, params.feeAddr, params.feeAmt),

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

200:         IERC20(virtualTokenAddress).safeTransferFrom(msg.sender, address(this), virtualsAmt);

290:             IERC20(virtualTokenAddress).safeTransfer(

315:         IERC20(agentTokenAddress).safeTransfer(userAddress, amount);

337:                 IERC20(virtualTokenAddress).safeTransfer(participant, virtualsAmt);

456:         IERC20(token).safeTransfer(to, amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/libs/TokenSaver.sol

21:         IERC20(_token).safeTransfer(_receiver, _amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

53:         IERC20(tokenIn).safeTransferFrom(msg.sender, address(this), amountIn);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/tax/AgentTax.sol

165:         IERC20(token).safeTransfer(treasury, IERC20(token).balanceOf(address(this)));

236:                 IERC20(assetToken).safeTransfer(taxRecipient.creator, creatorFee);

243:                 IERC20(assetToken).safeTransfer(treasury, feeAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

119:         IERC20(token).safeTransfer(treasury, IERC20(token).balanceOf(address(this)));

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

37:         IERC20(token).safeTransfer(_msgSender(), IERC20(token).balanceOf(address(this)));

59:         IERC20(taxToken).safeTransfer(recipient, total);

64:         IERC20(taxToken).safeTransfer(recipient, amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

81:             assetToken.safeTransfer(recipient, bonus);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

154:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

193:         IERC20(assetToken).safeTransfer(application.proposer, withdrawableAmount);

222:         IERC20(assetToken).transfer(token, initialAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

157:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

196:         IERC20(assetToken).safeTransfer(application.proposer, withdrawableAmount);

215:         IERC20(assetToken).safeTransfer(token, initialAmount);

441:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

155:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

194:         IERC20(assetToken).safeTransfer(application.proposer, withdrawableAmount);

198:             IERC20(customToken).safeTransfer(application.proposer, IERC20(customToken).balanceOf(address(this)));

222:             IERC20(assetToken).safeTransfer(token, initialAmount);

476:         IERC20(tokenAddr).safeTransferFrom(sender, address(this), initialLP);

478:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

110:         IERC20(_assetToken).transferFrom(founder, token, initialAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

262:         IERC20(uniswapV2Pair).transfer(lpOwner, lpTokens);

895:         IERC20(token_).safeTransfer(_msgSender(), amount_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

78:         IERC20(assetToken).safeTransferFrom(sender, address(this), amount);

106:         IERC20(assetToken).safeTransfer(sender, amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="L-4"></a>[L-4] Missing checks for `address(0)` when assigning values to address state variables

*Instances (85)*:

```solidity
File: contracts/contribution/ContributionNft.sol

47:         personaNft = thePersonaAddress;

105:         _admin = newAdmin;

167:         _eloCalculator = eloCalculator_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

53:         personaNft = initialAgentNft;

54:         contributionNft = initialContributionNft;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

110:         _feeTo = feeTo_;

117:         agentFactory = agentFactory_;

153:         _feeTo = newFeeTo;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FFactory.sol

36:         taxVault = taxVault_;

83:         router = router_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

184:         taxManager = newManager;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

40:         mockAgentToken = token;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

36:         router = router_;

37:         tokenIn = tokenIn_;

38:         tokenOut = tokenOut_;

39:         factory = factory_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/tax/AgentTax.sol

102:         treasury = treasury_;

127:         assetToken = assetToken_;

159:         treasury = treasury_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

68:         assetToken = assetToken_;

69:         taxToken = taxToken_;

71:         bondingRouter = bondingRouter_;

72:         treasury = treasury_;

90:         assetToken = assetToken_;

92:         bondingRouter = bondingRouter_;

113:         treasury = treasury_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

33:         taxToken = taxToken_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

54:         _agentNft = agentNft;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

120:         tokenImplementation = tokenImplementation_;

121:         veTokenImplementation = veTokenImplementation_;

122:         daoImplementation = daoImplementation_;

123:         assetToken = assetToken_;

124:         tbaRegistry = tbaRegistry_;

125:         nft = nft_;

129:         _vault = vault_;

333:         _vault = newVault;

337:         tokenImplementation = token;

338:         daoImplementation = dao;

339:         veTokenImplementation = veToken;

347:         _uniswapRouter = router;

351:         _tokenAdmin = newTokenAdmin;

389:         assetToken = newToken;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

123:         tokenImplementation = tokenImplementation_;

124:         veTokenImplementation = veTokenImplementation_;

125:         daoImplementation = daoImplementation_;

126:         assetToken = assetToken_;

127:         tbaRegistry = tbaRegistry_;

128:         nft = nft_;

132:         _vault = vault_;

347:         _vault = newVault;

351:         tokenImplementation = token;

352:         daoImplementation = dao;

353:         veTokenImplementation = veToken;

361:         _uniswapRouter = router;

365:         _tokenAdmin = newTokenAdmin;

403:         assetToken = newToken;

490:         defaultDelegatee = newDelegatee;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

121:         tokenImplementation = tokenImplementation_;

122:         veTokenImplementation = veTokenImplementation_;

123:         daoImplementation = daoImplementation_;

124:         assetToken = assetToken_;

125:         tbaRegistry = tbaRegistry_;

126:         nft = nft_;

130:         _vault = vault_;

371:         _vault = newVault;

375:         tokenImplementation = token;

376:         daoImplementation = dao;

377:         veTokenImplementation = veToken;

385:         _uniswapRouter = router;

389:         _tokenAdmin = newTokenAdmin;

427:         assetToken = newToken;

447:         defaultDelegatee = newDelegatee;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

53:         _tokenAdmin = tokenAdmin_;

54:         _assetToken = assetToken_;

55:         _uniswapRouter = uniswapRouter_;

95:         tokenImplementation = token;

96:         daoImplementation = dao;

97:         veTokenImplementation = veToken;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

77:         _contributionNft = contributionNft_;

78:         _serviceNft = serviceNft_;

244:         _eloCalculator = eloCalculator;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

383:         projectTaxRecipient = projectTaxRecipient_;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

51:         founder = _founder;

53:         assetToken = _assetToken;

54:         agentNft = _agentNft;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="L-5"></a>[L-5] `decimals()` is not a part of the ERC-20 standard

The `decimals()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*Instances (2)*:

```solidity
File: contracts/fun/Bonding.sol

392:             _token.data.supply / (10 ** token_.decimals()),

393:             tokenBalance / (10 ** token_.decimals()),

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

### <a name="L-6"></a>[L-6] Deprecated approve() function

Due to the inheritance of ERC20's approve function, there's a vulnerability to the ERC20 approve and double spend front running attack. Briefly, an authorized spender could spend both allowances by front running an allowance-changing transaction. Consider implementing OpenZeppelin's `.safeApprove()` function to help mitigate this.

*Instances (6)*:

```solidity
File: contracts/genesis/Genesis.sol

246:             IERC20(virtualTokenAddress).approve(agentFactoryAddress, reserveAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

269:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

262:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

286:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

130:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

244:         IERC20(pairToken).approve(address(_uniswapRouter), type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="L-7"></a>[L-7] Division by zero not prevented

The divisions below take an input parameter which does not have any zero-value checks, which may lead to the functions reverting when zero is passed.

*Instances (18)*:

```solidity
File: contracts/fun/Bonding.sol

215:         uint256 k = ((K * 10000) / assetRate);

216:         uint256 liquidity = (((k * 10000 ether) / supply) * 1 ether) / 10000;

226:             price: supply / liquidity,

231:             prevPrice: supply / liquidity,

297:         uint256 mCap = (tokenInfo[tokenAddress].data.supply * newReserveB) / newReserveA;

298:         uint256 price = newReserveA / newReserveB;

334:         uint256 mCap = (tokenInfo[tokenAddress].data.supply * newReserveB) / newReserveA;

335:         uint256 price = newReserveA / newReserveB;

392:             _token.data.supply / (10 ** token_.decimals()),

393:             tokenBalance / (10 ** token_.decimals()),

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FPair.sol

103:         return _pool.reserve1 / _pool.reserve0;

107:         return _pool.reserve0 / _pool.reserve1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

61:             uint256 newReserveA = k / newReserveB;

67:             uint256 newReserveB = k / newReserveA;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/libs/Elo.sol

63:             kExpectedScore = _kFactor / (100 + powered); // both numerator and denominator scaled up by 100

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

197:         return Math.min(10000, _proposalMaturities[proposalId] / forVotes);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

815:                 projectTaxPendingSwap -= uint128((projectTaxPendingSwap * swapBalance_) / contractBalance_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

35:         return (numerator + denominator - 1) / denominator;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="L-8"></a>[L-8] `domainSeparator()` isn't protected against replay attacks in case of a future chain split

Severity: Low.
Description: See <https://eips.ethereum.org/EIPS/eip-2612#security-considerations>.
Remediation: Consider using the [implementation](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/cryptography/EIP712.sol#L77-L90) from OpenZeppelin, which recalculates the domain separator if the current `block.chainid` is not the cached chain ID.
Past occurrences of this issue:

- [Reality Cards Contest](https://github.com/code-423n4/2021-06-realitycards-findings/issues/166)
- [Swivel Contest](https://github.com/code-423n4/2021-09-swivel-findings/issues/98)
- [Malt Finance Contest](https://github.com/code-423n4/2021-11-malt-findings/issues/349)

*Instances (1)*:

```solidity
File: contracts/pool/IUniswapV2Pair.sol

18:     function DOMAIN_SEPARATOR() external view returns (bytes32);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

### <a name="L-9"></a>[L-9] Duplicate import statements

*Instances (10)*:

```solidity
File: contracts/AgentInference.sol

6: import "@openzeppelin/contracts/utils/math/Math.sol";

9: import "@openzeppelin/contracts/utils/math/Math.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/genesis/FGenesis.sol

8: import "../virtualPersona/AgentFactoryV3.sol";

11: import "../virtualPersona/AgentFactoryV3.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/tax/AgentTax.sol

5: import "@openzeppelin/contracts/utils/math/Math.sol";

8: import "@openzeppelin/contracts/utils/math/Math.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

5: import "@openzeppelin/contracts/utils/math/Math.sol";

8: import "@openzeppelin/contracts/utils/math/Math.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

6: import {IERC5805} from "@openzeppelin/contracts/interfaces/IERC5805.sol";

11: import "@openzeppelin/contracts/interfaces/IERC5805.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

### <a name="L-10"></a>[L-10] Empty Function Body - Consider commenting why

*Instances (2)*:

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

61:     function withdraw(uint256) public pure {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

31:     function __GovernorCountingSimple_init() internal onlyInitializing {}

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

### <a name="L-11"></a>[L-11] Empty `receive()/payable fallback()` function does not authenticate requests

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds. If the concern is having to spend a small amount of gas to check the sender against an immutable address, the code should at least have a function to rescue unused Ether.

*Instances (1)*:

```solidity
File: contracts/virtualPersona/AgentToken.sol

1062:     receive() external payable {}

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="L-12"></a>[L-12] External call recipient may consume all transaction gas

There is no limit specified on the amount of gas used, so the recipient can use up all of the transaction's gas, causing it to revert. Use `addr.call{gas: <amount>}("")` or [this](https://github.com/nomad-xyz/ExcessivelySafeCall) library instead.

*Instances (1)*:

```solidity
File: contracts/virtualPersona/AgentToken.sol

869:         (bool success, ) = _msgSender().call{value: amount_}("");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="L-13"></a>[L-13] Initializers could be front-run

Initializers could be front-run, allowing an attacker to either set their own values, take ownership of the contract, and in the best case forcing a re-deployment

*Instances (92)*:

```solidity
File: contracts/AgentInference.sol

24:     function initialize(address defaultAdmin_, address token_, address agentNft_) external initializer {

25:         __AccessControl_init();

26:         __ReentrancyGuard_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

42:     function initialize(address thePersonaAddress) public initializer {

43:         __ERC721_init("Contribution", "VC");

44:         __ERC721Enumerable_init();

45:         __ERC721URIStorage_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

44:     function initialize(

48:     ) public initializer {

49:         __ERC721_init("Service", "VS");

50:         __ERC721Enumerable_init();

51:         __ERC721URIStorage_init();

52:         __Ownable_init(_msgSender());

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

93:     function initialize(

103:     ) external initializer {

104:         __Ownable_init(msg.sender);

105:         __ReentrancyGuard_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FFactory.sol

31:     function initialize(address taxVault_, uint256 buyTax_, uint256 sellTax_) external initializer {

32:         __AccessControl_init();

33:         __ReentrancyGuard_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

29:     function initialize(address factory_, address assetToken_) external initializer {

30:         __ReentrancyGuard_init();

31:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

46:     function initialize(Params memory p) external initializer {

47:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

134:     function initialize(GenesisInitParams calldata params) external initializer {

135:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/GenesisLib.sol

53:         newGenesis.initialize(initParams);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/GenesisLib.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

21:     function initialize(

31:     ) public initializer {

32:         __AccessControl_init();

33:         __Pausable_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

51:     function initialize(address, address) external;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/tax/AgentTax.sol

83:     function initialize(

92:     ) external initializer {

93:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

54:     function initialize(

63:     ) external initializer {

64:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

27:     function initialize(address defaultAdmin_, address taxToken_) external initializer {

28:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

33:     function initialize(address defaultAdmin_, address assetToken_) external initializer {

34:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

40:     function initialize(

46:     ) external initializer {

47:         __Governor_init(name);

48:         __GovernorSettings_init(0, votingPeriod_, threshold);

49:         __GovernorCountingSimple_init();

50:         __GovernorVotes_init(token);

51:         __GovernorVotesQuorumFraction_init(5100);

52:         __GovernorStorage_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

108:     function initialize(

117:     ) public initializer {

118:         __Pausable_init();

282:         IAgentDAO(instance).initialize(name, token, nft, daoThreshold, daoVotingPeriod);

290:         IAgentToken(instance).initialize(

309:         IAgentVeToken(instance).initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

110:     function initialize(

120:     ) public initializer {

121:         __Pausable_init();

292:         IAgentDAO(instance).initialize(name, token, nft, daoThreshold, daoVotingPeriod);

304:         IAgentToken(instance).initialize(

323:         IAgentVeToken(instance).initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

108:     function initialize(

118:     ) public initializer {

119:         __Pausable_init();

316:         IAgentDAO(instance).initialize(name, token, nft, daoThreshold, daoVotingPeriod);

328:         IAgentToken(instance).initialize(

347:         IAgentVeToken(instance).initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

145:         IAgentDAO(instance).initialize(name, token, address(_nft), daoThreshold, daoVotingPeriod);

158:         IAgentVeToken(instance).initialize(

173:         IAgentToken(instance).initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

61:     function initialize(address defaultAdmin) public initializer {

62:         __ERC721_init("Agent", "AGENT");

63:         __ERC721URIStorage_init();

64:         __CoreRegistry_init();

65:         __ValidatorRegistry_init(_validatorScoreOf, totalProposals, _getPastValidatorScore);

66:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

79:     function initialize(

84:     ) external initializer {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

39:     function initialize(

47:     ) external initializer {

48:         __ERC20_init(_name, _symbol);

49:         __ERC20Votes_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

12:     function __CoreRegistry_init() internal onlyInitializing {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

18:     function initialize(address initialOwner) public initializer {

19:         __Ownable_init(initialOwner);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

31:     function __GovernorCountingSimple_init() internal onlyInitializing {}

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

```solidity
File: contracts/virtualPersona/IAgentDAO.sol

8:     function initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentDAO.sol)

```solidity
File: contracts/virtualPersona/IAgentToken.sol

255:     function initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentToken.sol)

```solidity
File: contracts/virtualPersona/IAgentVeToken.sol

5:     function initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentVeToken.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

16:     function __ValidatorRegistry_init(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="L-14"></a>[L-14] Signature use at deadlines should be allowed

According to [EIP-2612](https://github.com/ethereum/EIPs/blob/71dc97318013bf2ac572ab63fab530ac9ef419ca/EIPS/eip-2612.md?plain=1#L58), signatures used on exactly the deadline timestamp are supposed to be allowed. While the signature may or may not be used for the exact EIP-2612 use case (transfer approvals), for consistency's sake, all deadlines should follow this semantic. If the timestamp is an expiration rather than a deadline, consider whether it makes more sense to include the expiration timestamp as a valid timestamp, as is done for deadlines.

*Instances (1)*:

```solidity
File: contracts/genesis/Genesis.sol

130:         require(_startTime > block.timestamp, ERR_START_TIME_FUTURE);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

### <a name="L-15"></a>[L-15] Prevent accidentally burning tokens

Minting and burning tokens to address(0) prevention

*Instances (12)*:

```solidity
File: contracts/contribution/ContributionNft.sol

82:         _mint(to, proposalId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

71:         _mint(info.tba, proposalId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/genesis/MockERC20.sol

13:         _mint(initialAccount, initialBalance);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockERC20.sol)

```solidity
File: contracts/token/Virtual.sol

18:         _mint(_to, _amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

97:         _mint(to, virtualId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

104:         _mintBalances(lpSupply, vaultSupply);

178:             _mint(address(this), lpMint_);

182:             _mint(vault, vaultMint_);

1011:         _burn(_msgSender(), value);

1027:         _burn(account, value);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

79:         _mint(receiver, amount);

103:         _burn(sender, amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="L-16"></a>[L-16] NFT ownership doesn't support hard forks

To ensure clarity regarding the ownership of the NFT on a specific chain, it is recommended to add `require(block.chainid == 1, "Invalid Chain")` or the desired chain ID in the functions below.

Alternatively, consider including the chain ID in the URI itself. By doing so, any confusion regarding the chain responsible for owning the NFT will be eliminated.

*Instances (4)*:

```solidity
File: contracts/contribution/ContributionNft.sol

110:     function tokenURI(
             uint256 tokenId
         ) public view override(IContributionNft, ERC721Upgradeable, ERC721URIStorageUpgradeable) returns (string memory) {
             return super.tokenURI(tokenId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/IContributionNft.sol

8:     function tokenURI(uint256 tokenId) external view returns (string memory);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/IContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

148:     function tokenURI(
             uint256 tokenId
         ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable) returns (string memory) {
             // Service NFT is a mirror of Contribution NFT
             return IContributionNft(contributionNft).tokenURI(tokenId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

214:     function tokenURI(
             uint256 tokenId
         ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable) returns (string memory) {
             return super.tokenURI(tokenId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

### <a name="L-17"></a>[L-17] Owner can renounce while system is paused

The contract owner or single user with a role is not prevented from renouncing the role/ownership while the contract is paused, which would cause any user assets stored in the protocol, to be locked indefinitely.

*Instances (2)*:

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

183:     function pause() external onlyOwner {

187:     function unpause() external onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

### <a name="L-18"></a>[L-18] Possible rounding issue

Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator. Also, there is indication of multiplication and division without the use of parenthesis which could result in issues.

*Instances (10)*:

```solidity
File: contracts/fun/Bonding.sol

216:         uint256 liquidity = (((k * 10000 ether) / supply) * 1 ether) / 10000;

297:         uint256 mCap = (tokenInfo[tokenAddress].data.supply * newReserveB) / newReserveA;

298:         uint256 price = newReserveA / newReserveB;

334:         uint256 mCap = (tokenInfo[tokenAddress].data.supply * newReserveB) / newReserveA;

335:         uint256 price = newReserveA / newReserveB;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FPair.sol

103:         return _pool.reserve1 / _pool.reserve0;

107:         return _pool.reserve0 / _pool.reserve1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

61:             uint256 newReserveA = k / newReserveB;

67:             uint256 newReserveB = k / newReserveA;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

815:                 projectTaxPendingSwap -= uint128((projectTaxPendingSwap * swapBalance_) / contractBalance_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="L-19"></a>[L-19] Loss of precision

Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator

*Instances (9)*:

```solidity
File: contracts/fun/FPair.sol

103:         return _pool.reserve1 / _pool.reserve0;

107:         return _pool.reserve0 / _pool.reserve1;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/libs/Elo.sol

59:             _powered = fp.rpow(10, n / 25, 1); // divide by 25 to avoid reach uint256 max

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/tax/AgentTax.sol

232:             uint256 feeAmount = (assetReceived * feeRate) / DENOM;

282:             uint256 minOutput = ((amountToSwap * (DENOM - slippage)) / DENOM);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/TBABonus.sol

73:         uint256 bonus = (amount * bonusRate) / DENOM;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

698:                         uint256 projectTax = ((sentAmount_ * projectSellTaxBasisPoints) / BP_DENOM);

706:                         uint256 projectTax = ((sentAmount_ * projectBuyTaxBasisPoints) / BP_DENOM);

736:             uint256 swapThresholdInTokens = (_totalSupply * swapThresholdBasisPoints) / BP_DENOM;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="L-20"></a>[L-20] Solidity version 0.8.20+ may not work on other chains due to `PUSH0`

The compiler for Solidity 0.8.20 switches the default target EVM version to [Shanghai](https://blog.soliditylang.org/2023/05/10/solidity-0.8.20-release-announcement/#important-note), which includes the new `PUSH0` op code. This op code may not yet be implemented on all L2s, so deployment on these chains will fail. To work around this issue, use an earlier [EVM](https://docs.soliditylang.org/en/v0.8.20/using-the-compiler.html?ref=zaryabs.com#setting-the-evm-version-to-target) [version](https://book.getfoundry.sh/reference/config/solidity-compiler#evm_version). While the project itself may or may not compile with 0.8.20, other projects with which it integrates, or which extend this project may, and those projects will have problems deploying these contracts/libraries.

*Instances (38)*:

```solidity
File: contracts/AgentInference.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

3: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FPair.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FPair.sol)

```solidity
File: contracts/fun/FRouter.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

2: pragma solidity ^0.8.26;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

2: pragma solidity ^0.8.26;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/GenesisLib.sol

3: pragma solidity ^0.8.26;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/GenesisLib.sol)

```solidity
File: contracts/genesis/GenesisTypes.sol

2: pragma solidity ^0.8.26;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/GenesisTypes.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

2: pragma solidity ^0.8.26;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/genesis/MockERC20.sol

2: pragma solidity ^0.8.26;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockERC20.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

3: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/VirtualProtocolDAO.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualProtocolDAO.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/libs/AddressCheckpoints.sol

3: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/AddressCheckpoints.sol)

```solidity
File: contracts/libs/Elo.sol

2: pragma solidity ^0.8.13;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

2: pragma solidity >=0.8.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpoints.sol

3: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpoints.sol)

```solidity
File: contracts/libs/RewardSettingsCheckpointsV2.sol

3: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/RewardSettingsCheckpointsV2.sol)

```solidity
File: contracts/libs/TokenSaver.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/tax/AgentTax.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/token/Airdrop.sol

1: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Airdrop.sol)

```solidity
File: contracts/token/Virtual.sol

1: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

2: pragma solidity ^0.8.20;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="L-21"></a>[L-21] Use `Ownable2Step.transferOwnership` instead of `Ownable.transferOwnership`

Use [Ownable2Step.transferOwnership](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol) which is safer. Use it as it is more secure due to 2-stage ownership transfer.

**Recommended Mitigation Steps**

Use <a href="https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol">Ownable2Step.sol</a>
  
  ```solidity
      function acceptOwnership() external {
          address sender = _msgSender();
          require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
          _transferOwnership(sender);
      }
```

*Instances (8)*:

```solidity
File: contracts/contribution/ServiceNft.sol

9: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

7: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

4: import "@openzeppelin/contracts/access/Ownable.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

5: import "@openzeppelin/contracts/access/Ownable.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/token/Virtual.sol

7: import "@openzeppelin/contracts/access/Ownable.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

7: import "@openzeppelin/contracts/access/Ownable.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

121:         _transferOwnership(projectOwner_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

6: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="L-22"></a>[L-22] File allows a version of solidity that is susceptible to an assembly optimizer bug

In solidity versions 0.8.13 and 0.8.14, there is an [optimizer bug](https://github.com/ethereum/solidity-blog/blob/499ab8abc19391be7b7b34f88953a067029a5b45/_posts/2022-06-15-inline-assembly-memory-side-effects-bug.md) where, if the use of a variable is in a separate `assembly` block from the block in which it was stored, the `mstore` operation is optimized out, leading to uninitialized memory. The code currently does not have such a pattern of execution, but it does use `mstore`s in `assembly` blocks, so it is a risk for future changes. The affected solidity versions should be avoided if at all possible.

*Instances (2)*:

```solidity
File: contracts/libs/Elo.sol

2: pragma solidity ^0.8.13;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/Elo.sol)

```solidity
File: contracts/libs/FixedPointMathLib.sol

2: pragma solidity >=0.8.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

### <a name="L-23"></a>[L-23] `symbol()` is not a part of the ERC-20 standard

The `symbol()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*Instances (2)*:

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

486:             IAgentToken(tokenAddr).symbol(),

525:             try IAgentToken(tokenAddr).symbol() returns (string memory) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

### <a name="L-24"></a>[L-24] Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when downcasting

Downcasting from `uint256`/`int256` in Solidity does not revert on overflow. This can result in undesired exploitation or bugs, since developers usually assume that overflows raise errors. [OpenZeppelin's SafeCast library](https://docs.openzeppelin.com/contracts/3.x/api/utils#SafeCast) restores this intuition by reverting the transaction when such an operation overflows. Using this library eliminates an entire class of bugs, so it's recommended to use it always. Some exceptions are acceptable like with the classic `uint256(uint160(address(variable)))`

*Instances (9)*:

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

125:             _createNewDAO(oldDAO.name(), IVotes(veToken), uint32(oldDAO.votingPeriod()), oldDAO.proposalThreshold())

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

101:         swapThresholdBasisPoints = uint16(taxParams.taxSwapThresholdBasisPoints);

163:             projectBuyTaxBasisPoints = uint16(erc20TaxParameters_.projectBuyTaxBasisPoints);

164:             projectSellTaxBasisPoints = uint16(erc20TaxParameters_.projectSellTaxBasisPoints);

699:                         projectTaxPendingSwap += uint128(projectTax);

707:                         projectTaxPendingSwap += uint128(projectTax);

815:                 projectTaxPendingSwap -= uint128((projectTaxPendingSwap * swapBalance_) / contractBalance_);

914:         _totalSupply += uint128(amount);

950:             _totalSupply -= uint128(amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="L-25"></a>[L-25] Unsafe ERC20 operation(s)

*Instances (12)*:

```solidity
File: contracts/fun/Bonding.sol

276:         token.transfer(msg.sender, token.balanceOf(address(this)));

417:                 agentToken.transferFrom(pairAddress, acc, balance);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/FGenesis.sol

78:             IERC20(params.virtualToken).transferFrom(msg.sender, params.feeAddr, params.feeAmt),

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

246:             IERC20(virtualTokenAddress).approve(agentFactoryAddress, reserveAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

222:         IERC20(assetToken).transfer(token, initialAmount);

269:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

262:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

286:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

110:         IERC20(_assetToken).transferFrom(founder, token, initialAmount);

130:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

244:         IERC20(pairToken).approve(address(_uniswapRouter), type(uint256).max);

262:         IERC20(uniswapV2Pair).transfer(lpOwner, lpTokens);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="L-26"></a>[L-26] Unspecific compiler version pragma

*Instances (5)*:

```solidity
File: contracts/libs/FixedPointMathLib.sol

2: pragma solidity >=0.8.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/FixedPointMathLib.sol)

```solidity
File: contracts/pool/IUniswapV2Factory.sol

1: pragma solidity >=0.5.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Factory.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

1: pragma solidity >=0.5.0;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/pool/IUniswapV2Router01.sol

1: pragma solidity >=0.6.2;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router01.sol)

```solidity
File: contracts/pool/IUniswapV2Router02.sol

1: pragma solidity >=0.6.2;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Router02.sol)

### <a name="L-27"></a>[L-27] Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions

See [this](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps) link for a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

*Instances (126)*:

```solidity
File: contracts/AgentInference.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

4: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

12: contract AgentInference is Initializable, AccessControlUpgradeable, ReentrancyGuardUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

5: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol";

10: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

17:     ERC721Upgradeable,

18:     ERC721EnumerableUpgradeable,

19:     ERC721URIStorageUpgradeable

112:     ) public view override(IContributionNft, ERC721Upgradeable, ERC721URIStorageUpgradeable) returns (string memory) {

130:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable, ERC721EnumerableUpgradeable) returns (bool) {

137:     ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) {

145:     ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) returns (address) {

153:     function ownerOf(uint256 tokenId) public view override(IERC721, ERC721Upgradeable) returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

6: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol";

9: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

11: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

20:     ERC721Upgradeable,

21:     ERC721EnumerableUpgradeable,

22:     ERC721URIStorageUpgradeable,

23:     OwnableUpgradeable

150:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable) returns (string memory) {

157:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable, ERC721EnumerableUpgradeable) returns (bool) {

164:     ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) {

172:     ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

6: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

18: contract Bonding is Initializable, ReentrancyGuardUpgradeable, OwnableUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FFactory.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

10: contract FFactory is Initializable, AccessControlUpgradeable, ReentrancyGuardUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

14: contract FRouter is Initializable, AccessControlUpgradeable, ReentrancyGuardUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

13: contract FGenesis is Initializable, AccessControlUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

13: contract Genesis is ReentrancyGuard, AccessControlUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

6: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

10: contract MockAgentFactoryV3 is IAgentFactoryV3, Initializable, AccessControlUpgradeable, PausableUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/tax/AgentTax.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

13: contract AgentTax is Initializable, AccessControlUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

12: contract BondingTax is Initializable, AccessControlUpgradeable, IBondingTax {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

6: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: contract LPRefund is Initializable, AccessControlUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

10: contract TBABonus is ITBABonus, Initializable, AccessControlUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

4: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorSettingsUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorStorageUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesQuorumFractionUpgradeable.sol";

9: import "./GovernorCountingSimpleUpgradeable.sol";

19:     GovernorSettingsUpgradeable,

20:     GovernorCountingSimpleUpgradeable,

21:     GovernorStorageUpgradeable,

22:     GovernorVotesUpgradeable,

23:     GovernorVotesQuorumFractionUpgradeable

59:     function votingDelay() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256) {

63:     function votingPeriod() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256) {

70:         override(GovernorUpgradeable, GovernorSettingsUpgradeable)

81:     ) public override(GovernorUpgradeable) returns (uint256) {

104:     ) internal override(GovernorUpgradeable, GovernorStorageUpgradeable) returns (uint256) {

108:     function proposalCount() public view override(IAgentDAO, GovernorStorageUpgradeable) returns (uint256) {

202:     ) public view override(GovernorUpgradeable, GovernorVotesQuorumFractionUpgradeable) returns (uint256) {

210:     function state(uint256 proposalId) public view override(GovernorUpgradeable) returns (ProposalState) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

8: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

19: contract AgentFactoryV2 is IAgentFactory, Initializable, AccessControl, PausableUpgradeable {

400:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

401:         sender = ContextUpgradeable._msgSender();

404:     function _msgData() internal view override(Context, ContextUpgradeable) returns (bytes calldata) {

405:         return ContextUpgradeable._msgData();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

8: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

19: contract AgentFactoryV3 is IAgentFactoryV3, Initializable, AccessControl, PausableUpgradeable {

414:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

415:         sender = ContextUpgradeable._msgSender();

418:     function _msgData() internal view override(Context, ContextUpgradeable) returns (bytes calldata) {

419:         return ContextUpgradeable._msgData();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

8: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

21: contract AgentFactoryV4 is IAgentFactoryV4, Initializable, AccessControl, PausableUpgradeable {

438:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

439:         sender = ContextUpgradeable._msgSender();

442:     function _msgData() internal view override(Context, ContextUpgradeable) returns (bytes calldata) {

443:         return ContextUpgradeable._msgData();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

7: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol";

10: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

12: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

21:     ERC721Upgradeable,

22:     ERC721URIStorageUpgradeable,

23:     AccessControlUpgradeable,

216:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable) returns (string memory) {

222:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable, AccessControlUpgradeable) returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/access/Ownable2StepUpgradeable.sol";

14: contract AgentToken is ContextUpgradeable, IAgentToken, Ownable2StepUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";

13: contract AgentVeToken is IAgentVeToken, ERC20Upgradeable, ERC20Votes {

135:     ) internal override(ERC20Upgradeable, ERC20VotesUpgradeable) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/ERC20Votes.sol

5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol";

9: abstract contract ERC20Votes is ERC20VotesUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ERC20Votes.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

4: import "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

6: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

10: contract EloCalculator is IEloCalculator, Initializable, OwnableUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

6: import "@openzeppelin/contracts-upgradeable/governance/GovernorUpgradeable.sol";

11: abstract contract GovernorCountingSimpleUpgradeable is GovernorUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

### <a name="L-28"></a>[L-28] Upgradeable contract not initialized

Upgradeable contracts are initialized via an initializer function rather than by a constructor. Leaving such a contract uninitialized may lead to it being taken over by a malicious user

*Instances (238)*:

```solidity
File: contracts/AgentInference.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

4: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

12: contract AgentInference is Initializable, AccessControlUpgradeable, ReentrancyGuardUpgradeable {

24:     function initialize(address defaultAdmin_, address token_, address agentNft_) external initializer {

25:         __AccessControl_init();

26:         __ReentrancyGuard_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/AgentInference.sol)

```solidity
File: contracts/contribution/ContributionNft.sol

5: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol";

10: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

17:     ERC721Upgradeable,

18:     ERC721EnumerableUpgradeable,

19:     ERC721URIStorageUpgradeable

39:         _disableInitializers();

42:     function initialize(address thePersonaAddress) public initializer {

43:         __ERC721_init("Contribution", "VC");

44:         __ERC721Enumerable_init();

45:         __ERC721URIStorage_init();

112:     ) public view override(IContributionNft, ERC721Upgradeable, ERC721URIStorageUpgradeable) returns (string memory) {

130:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable, ERC721EnumerableUpgradeable) returns (bool) {

137:     ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) {

145:     ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) returns (address) {

153:     function ownerOf(uint256 tokenId) public view override(IERC721, ERC721Upgradeable) returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

6: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721EnumerableUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol";

9: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

11: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

20:     ERC721Upgradeable,

21:     ERC721EnumerableUpgradeable,

22:     ERC721URIStorageUpgradeable,

23:     OwnableUpgradeable

41:         _disableInitializers();

44:     function initialize(

48:     ) public initializer {

49:         __ERC721_init("Service", "VS");

50:         __ERC721Enumerable_init();

51:         __ERC721URIStorage_init();

52:         __Ownable_init(_msgSender());

150:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable) returns (string memory) {

157:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable, ERC721EnumerableUpgradeable) returns (bool) {

164:     ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) {

172:     ) internal override(ERC721Upgradeable, ERC721EnumerableUpgradeable) returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

6: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

18: contract Bonding is Initializable, ReentrancyGuardUpgradeable, OwnableUpgradeable {

90:         _disableInitializers();

93:     function initialize(

103:     ) external initializer {

104:         __Ownable_init(msg.sender);

105:         __ReentrancyGuard_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FFactory.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

10: contract FFactory is Initializable, AccessControlUpgradeable, ReentrancyGuardUpgradeable {

28:         _disableInitializers();

31:     function initialize(address taxVault_, uint256 buyTax_, uint256 sellTax_) external initializer {

32:         __AccessControl_init();

33:         __ReentrancyGuard_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";

14: contract FRouter is Initializable, AccessControlUpgradeable, ReentrancyGuardUpgradeable {

26:         _disableInitializers();

29:     function initialize(address factory_, address assetToken_) external initializer {

30:         __ReentrancyGuard_init();

31:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

13: contract FGenesis is Initializable, AccessControlUpgradeable {

43:         _disableInitializers();

46:     function initialize(Params memory p) external initializer {

47:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

5: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

13: contract Genesis is ReentrancyGuard, AccessControlUpgradeable {

134:     function initialize(GenesisInitParams calldata params) external initializer {

135:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/GenesisLib.sol

53:         newGenesis.initialize(initParams);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/GenesisLib.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

6: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

10: contract MockAgentFactoryV3 is IAgentFactoryV3, Initializable, AccessControlUpgradeable, PausableUpgradeable {

18:         _disableInitializers();

21:     function initialize(

31:     ) public initializer {

32:         __AccessControl_init();

33:         __Pausable_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/pool/IUniswapV2Pair.sol

51:     function initialize(address, address) external;

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/IUniswapV2Pair.sol)

```solidity
File: contracts/tax/AgentTax.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

13: contract AgentTax is Initializable, AccessControlUpgradeable {

80:         _disableInitializers();

83:     function initialize(

92:     ) external initializer {

93:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

12: contract BondingTax is Initializable, AccessControlUpgradeable, IBondingTax {

46:         _disableInitializers();

54:     function initialize(

63:     ) external initializer {

64:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

3: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

6: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

8: contract LPRefund is Initializable, AccessControlUpgradeable {

24:         _disableInitializers();

27:     function initialize(address defaultAdmin_, address taxToken_) external initializer {

28:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

7: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

10: contract TBABonus is ITBABonus, Initializable, AccessControlUpgradeable {

30:         _disableInitializers();

33:     function initialize(address defaultAdmin_, address assetToken_) external initializer {

34:         __AccessControl_init();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/virtualPersona/AgentDAO.sol

4: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorSettingsUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorStorageUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/governance/extensions/GovernorVotesQuorumFractionUpgradeable.sol";

9: import "./GovernorCountingSimpleUpgradeable.sol";

19:     GovernorSettingsUpgradeable,

20:     GovernorCountingSimpleUpgradeable,

21:     GovernorStorageUpgradeable,

22:     GovernorVotesUpgradeable,

23:     GovernorVotesQuorumFractionUpgradeable

37:         _disableInitializers();

40:     function initialize(

46:     ) external initializer {

47:         __Governor_init(name);

48:         __GovernorSettings_init(0, votingPeriod_, threshold);

49:         __GovernorCountingSimple_init();

50:         __GovernorVotes_init(token);

51:         __GovernorVotesQuorumFraction_init(5100);

52:         __GovernorStorage_init();

59:     function votingDelay() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256) {

63:     function votingPeriod() public view override(GovernorUpgradeable, GovernorSettingsUpgradeable) returns (uint256) {

70:         override(GovernorUpgradeable, GovernorSettingsUpgradeable)

81:     ) public override(GovernorUpgradeable) returns (uint256) {

104:     ) internal override(GovernorUpgradeable, GovernorStorageUpgradeable) returns (uint256) {

108:     function proposalCount() public view override(IAgentDAO, GovernorStorageUpgradeable) returns (uint256) {

202:     ) public view override(GovernorUpgradeable, GovernorVotesQuorumFractionUpgradeable) returns (uint256) {

210:     function state(uint256 proposalId) public view override(GovernorUpgradeable) returns (ProposalState) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

8: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

19: contract AgentFactoryV2 is IAgentFactory, Initializable, AccessControl, PausableUpgradeable {

105:         _disableInitializers();

108:     function initialize(

117:     ) public initializer {

118:         __Pausable_init();

282:         IAgentDAO(instance).initialize(name, token, nft, daoThreshold, daoVotingPeriod);

290:         IAgentToken(instance).initialize(

309:         IAgentVeToken(instance).initialize(

400:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

401:         sender = ContextUpgradeable._msgSender();

404:     function _msgData() internal view override(Context, ContextUpgradeable) returns (bytes calldata) {

405:         return ContextUpgradeable._msgData();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

8: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

19: contract AgentFactoryV3 is IAgentFactoryV3, Initializable, AccessControl, PausableUpgradeable {

107:         _disableInitializers();

110:     function initialize(

120:     ) public initializer {

121:         __Pausable_init();

292:         IAgentDAO(instance).initialize(name, token, nft, daoThreshold, daoVotingPeriod);

304:         IAgentToken(instance).initialize(

323:         IAgentVeToken(instance).initialize(

414:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

415:         sender = ContextUpgradeable._msgSender();

418:     function _msgData() internal view override(Context, ContextUpgradeable) returns (bytes calldata) {

419:         return ContextUpgradeable._msgData();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

8: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

10: import "@openzeppelin/contracts-upgradeable/utils/PausableUpgradeable.sol";

21: contract AgentFactoryV4 is IAgentFactoryV4, Initializable, AccessControl, PausableUpgradeable {

105:         _disableInitializers();

108:     function initialize(

118:     ) public initializer {

119:         __Pausable_init();

316:         IAgentDAO(instance).initialize(name, token, nft, daoThreshold, daoVotingPeriod);

328:         IAgentToken(instance).initialize(

347:         IAgentVeToken(instance).initialize(

438:     function _msgSender() internal view override(Context, ContextUpgradeable) returns (address sender) {

439:         sender = ContextUpgradeable._msgSender();

442:     function _msgData() internal view override(Context, ContextUpgradeable) returns (bytes calldata) {

443:         return ContextUpgradeable._msgData();

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

145:         IAgentDAO(instance).initialize(name, token, address(_nft), daoThreshold, daoVotingPeriod);

158:         IAgentVeToken(instance).initialize(

173:         IAgentToken(instance).initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

7: import "@openzeppelin/contracts-upgradeable/token/ERC721/ERC721Upgradeable.sol";

8: import "@openzeppelin/contracts-upgradeable/token/ERC721/extensions/ERC721URIStorageUpgradeable.sol";

10: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

12: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

21:     ERC721Upgradeable,

22:     ERC721URIStorageUpgradeable,

23:     AccessControlUpgradeable,

58:         _disableInitializers();

61:     function initialize(address defaultAdmin) public initializer {

62:         __ERC721_init("Agent", "AGENT");

63:         __ERC721URIStorage_init();

64:         __CoreRegistry_init();

65:         __ValidatorRegistry_init(_validatorScoreOf, totalProposals, _getPastValidatorScore);

66:         __AccessControl_init();

216:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable) returns (string memory) {

222:     ) public view override(ERC721Upgradeable, ERC721URIStorageUpgradeable, AccessControlUpgradeable) returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

5: import "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/access/Ownable2StepUpgradeable.sol";

14: contract AgentToken is ContextUpgradeable, IAgentToken, Ownable2StepUpgradeable {

76:         _disableInitializers();

79:     function initialize(

84:     ) external initializer {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";

13: contract AgentVeToken is IAgentVeToken, ERC20Upgradeable, ERC20Votes {

25:         _disableInitializers();

39:     function initialize(

47:     ) external initializer {

48:         __ERC20_init(_name, _symbol);

49:         __ERC20Votes_init();

135:     ) internal override(ERC20Upgradeable, ERC20VotesUpgradeable) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/CoreRegistry.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

12:     function __CoreRegistry_init() internal onlyInitializing {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/CoreRegistry.sol)

```solidity
File: contracts/virtualPersona/ERC20Votes.sol

5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20VotesUpgradeable.sol";

9: abstract contract ERC20Votes is ERC20VotesUpgradeable {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ERC20Votes.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

4: import "@openzeppelin/contracts-upgradeable/utils/ContextUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

6: import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

10: contract EloCalculator is IEloCalculator, Initializable, OwnableUpgradeable {

15:         _disableInitializers();

18:     function initialize(address initialOwner) public initializer {

19:         __Ownable_init(initialOwner);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

```solidity
File: contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol

6: import "@openzeppelin/contracts-upgradeable/governance/GovernorUpgradeable.sol";

11: abstract contract GovernorCountingSimpleUpgradeable is GovernorUpgradeable {

31:     function __GovernorCountingSimple_init() internal onlyInitializing {}

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/GovernorCountingSimpleUpgradeable.sol)

```solidity
File: contracts/virtualPersona/IAgentDAO.sol

8:     function initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentDAO.sol)

```solidity
File: contracts/virtualPersona/IAgentToken.sol

255:     function initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentToken.sol)

```solidity
File: contracts/virtualPersona/IAgentVeToken.sol

5:     function initialize(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IAgentVeToken.sol)

```solidity
File: contracts/virtualPersona/IErrors.sol

194:     error OwnershipNotInitializedForExtraData(); //           The `extraData` cannot be set on an uninitialized ownership slot.

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/IErrors.sol)

```solidity
File: contracts/virtualPersona/ValidatorRegistry.sol

4: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

16:     function __ValidatorRegistry_init(

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/ValidatorRegistry.sol)

## Medium Issues

| |Issue|Instances|
|-|:-|:-:|
| [M-1](#M-1) | Contracts are vulnerable to fee-on-transfer accounting-related issues | 10 |
| [M-2](#M-2) | `block.number` means different things on different L2s | 9 |
| [M-3](#M-3) | Centralization Risk for trusted owners | 120 |
| [M-4](#M-4) | `_safeMint()` should be used rather than `_mint()` wherever possible | 3 |
| [M-5](#M-5) | `increaseAllowance/decreaseAllowance` won't work on mainnet for USDT | 2 |
| [M-6](#M-6) | Direct `supportsInterface()` calls may cause caller to revert | 4 |
| [M-7](#M-7) | Return values of `transfer()`/`transferFrom()` not checked | 6 |
| [M-8](#M-8) | Unsafe use of `transfer()`/`transferFrom()`/`approve()`/ with `IERC20` | 12 |

### <a name="M-1"></a>[M-1] Contracts are vulnerable to fee-on-transfer accounting-related issues

Consistently check account balance before and after transfers for Fee-On-Transfer discrepancies. As arbitrary ERC20 tokens can be used, the amount here should be calculated every time to take into consideration a possible fee-on-transfer or deflation.
Also, it's a good practice for the future of the solution.

Use the balance before and after the transfer to calculate the received amount instead of assuming that it would be equal to the amount passed as a parameter. Or explicitly document that such tokens shouldn't be used and won't be supported

*Instances (10)*:

```solidity
File: contracts/fun/Bonding.sol

205:         IERC20(assetToken).safeTransferFrom(msg.sender, address(this), initialPurchase);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/Genesis.sol

200:         IERC20(virtualTokenAddress).safeTransferFrom(msg.sender, address(this), virtualsAmt);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/pool/AeroAdaptor.sol

53:         IERC20(tokenIn).safeTransferFrom(msg.sender, address(this), amountIn);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/pool/AeroAdaptor.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

154:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

157:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

441:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold_);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

155:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

476:         IERC20(tokenAddr).safeTransferFrom(sender, address(this), initialLP);

478:         IERC20(assetToken).safeTransferFrom(sender, address(this), applicationThreshold);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

78:         IERC20(assetToken).safeTransferFrom(sender, address(this), amount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

### <a name="M-2"></a>[M-2] `block.number` means different things on different L2s

On Optimism, `block.number` is the L2 block number, but on Arbitrum, it's the L1 block number, and `ArbSys(address(100)).arbBlockNumber()` must be used. Furthermore, L2 block numbers often occur much more frequently than L1 block numbers (any may even occur on a per-transaction basis), so using block numbers for timing results in inconsistencies, especially when voting is involved across multiple chains. As of version 4.9, OpenZeppelin has [modified](https://blog.openzeppelin.com/introducing-openzeppelin-contracts-v4.9#governor) their governor code to use a clock rather than block numbers, to avoid these sorts of issues, but this still requires that the project [implement](https://docs.openzeppelin.com/contracts/4.x/governance#token_2) a [clock](https://eips.ethereum.org/EIPS/eip-6372) for each L2.

*Instances (9)*:

```solidity
File: contracts/virtualPersona/AgentDAO.sol

137:             _scores[account].push(SafeCast.toUint48(block.number), SafeCast.toUint208(scoreOf(account)) + 1);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentDAO.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

157:         uint256 proposalEndBlock = block.number; // No longer required in v2

186:         require(block.number > application.proposalEndBlock, "Application is not matured yet");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

160:         uint256 proposalEndBlock = block.number; // No longer required in v2

189:         require(block.number > application.proposalEndBlock, "Application is not matured yet");

444:         uint256 proposalEndBlock = block.number; // No longer required in v2

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

158:         uint256 proposalEndBlock = block.number; // No longer required in v2

187:         require(block.number > application.proposalEndBlock, "Application is not matured yet");

492:             block.number,

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

### <a name="M-3"></a>[M-3] Centralization Risk for trusted owners

#### Impact

Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

*Instances (120)*:

```solidity
File: contracts/contribution/ServiceNft.sol

111:         _requireOwned(tokenId);

116:         _requireOwned(tokenId);

121:         _requireOwned(tokenId);

141:     function setDatasetImpactWeight(uint16 weight) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/fun/Bonding.sol

143:     function setInitialSupply(uint256 newSupply) public onlyOwner {

147:     function setGradThreshold(uint256 newThreshold) public onlyOwner {

151:     function setFee(uint256 newFee, address newFeeTo) public onlyOwner {

156:     function setMaxTx(uint256 maxTx_) public onlyOwner {

160:     function setAssetRate(uint256 newRate) public onlyOwner {

166:     function setDeployParams(DeployParams memory params) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/fun/FERC20.sol

7: contract FERC20 is Context, IERC20, Ownable {

28:     constructor(string memory name_, string memory symbol_, uint256 supply, uint _maxTx) Ownable(msg.sender) {

121:     function updateMaxTx(uint256 _maxTx) public onlyOwner {

125:     function excludeFromMaxTx(address user) public onlyOwner {

136:     function burnFrom(address user, uint256 amount) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FERC20.sol)

```solidity
File: contracts/fun/FFactory.sol

60:     function createPair(address tokenA, address tokenB) external onlyRole(CREATOR_ROLE) nonReentrant returns (address) {

74:     function setTaxParams(address newVault_, uint256 buyTax_, uint256 sellTax_) public onlyRole(ADMIN_ROLE) {

82:     function setRouter(address router_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FFactory.sol)

```solidity
File: contracts/fun/FRouter.sol

79:     ) public onlyRole(EXECUTOR_ROLE) returns (uint256, uint256) {

99:     ) public nonReentrant onlyRole(EXECUTOR_ROLE) returns (uint256, uint256) {

135:     ) public onlyRole(EXECUTOR_ROLE) nonReentrant returns (uint256, uint256) {

165:     function graduate(address tokenAddress) public onlyRole(EXECUTOR_ROLE) nonReentrant {

177:     ) public onlyRole(EXECUTOR_ROLE) nonReentrant {

183:     function setTaxManager(address newManager) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/FRouter.sol)

```solidity
File: contracts/genesis/FGenesis.sol

53:     function setParams(Params calldata p) external onlyRole(ADMIN_ROLE) {

119:     ) external onlyRole(OPERATION_ROLE) returns (address) {

130:     function onGenesisFailed(uint256 id, uint256[] calldata participantIndexes) external onlyRole(OPERATION_ROLE) {

139:     ) external onlyRole(ADMIN_ROLE) {

143:     function resetTime(uint256 id, uint256 newStartTime, uint256 newEndTime) external onlyRole(OPERATION_ROLE) {

147:     function cancelGenesis(uint256 id) external onlyRole(OPERATION_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

211:     ) external onlyRole(FACTORY_ROLE) nonReentrant whenNotCancelled whenNotFailed whenEnded returns (address) {

326:     ) external onlyRole(FACTORY_ROLE) nonReentrant whenNotCancelled whenNotFailed whenTokenNotLaunched whenEnded {

452:     ) external onlyRole(DEFAULT_ADMIN_ROLE) nonReentrant whenEnded whenFinalized {

465:     ) external onlyRole(FACTORY_ROLE) nonReentrant whenNotCancelled whenNotFailed whenNotStarted whenNotEnded {

479:         onlyRole(FACTORY_ROLE)

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/genesis/MockAgentFactoryV3.sol

79:     ) public view whenNotPaused onlyRole(BONDING_ROLE) returns (uint256) {

88:     ) public view onlyRole(BONDING_ROLE) returns (address) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/MockAgentFactoryV3.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

95:     function earlyExecute(uint256 proposalId) public payable onlyRole(EXECUTOR_ROLE) returns (uint256) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/governance/veVirtualToken.sol

9: contract veVirtualToken is ERC20, ERC20Permit, ERC20Votes, Ownable {

12:     ) ERC20("Virtual Protocol Voting", "veVIRTUAL") ERC20Permit("Virtual Protocol Voting") Ownable(initialOwner) {}

19:     ) external onlyOwner returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/veVirtualToken.sol)

```solidity
File: contracts/libs/TokenSaver.sol

8: contract TokenSaver is AccessControl {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/libs/TokenSaver.sol)

```solidity
File: contracts/tax/AgentTax.sol

120:     ) public onlyRole(ADMIN_ROLE) {

147:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

157:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

164:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

173:     ) public onlyRole(EXECUTOR_ROLE) {

269:     function dcaSell(uint256[] memory agentIds, uint256 slippage, uint256 maxOverride) public onlyRole(EXECUTOR_ROLE) {

287:     function updateTbaBonus(address tbaBonus_) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/AgentTax.sol)

```solidity
File: contracts/tax/BondingTax.sol

85:     ) public onlyRole(ADMIN_ROLE) {

101:     function updateSwapThresholds(uint256 minSwapThreshold_, uint256 maxSwapThreshold_) public onlyRole(ADMIN_ROLE) {

111:     function updateTreasury(address treasury_) public onlyRole(ADMIN_ROLE) {

118:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/BondingTax.sol)

```solidity
File: contracts/tax/LPRefund.sol

36:     function withdraw(address token) external onlyRole(ADMIN_ROLE) {

44:     ) public onlyRole(EXECUTOR_ROLE) {

62:     function manualRefund(bytes32 txhash, address recipient, uint256 amount) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/LPRefund.sol)

```solidity
File: contracts/tax/TBABonus.sol

44:     function updateBonusRate(uint16 bonusRate_) public onlyRole(ADMIN_ROLE) {

50:     function setAllowances(uint256[] memory agentIds, uint256[] memory allowances) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/tax/TBABonus.sol)

```solidity
File: contracts/token/Virtual.sol

9: contract VirtualToken is ERC20Capped, Ownable {

13:     ) ERC20("Virtual Protocol", "VIRTUAL") ERC20Capped(1000000000 * 10 ** 18) Ownable(initialOwner) {

17:     function mint(address _to, uint256 _amount) external onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/token/Virtual.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

19: contract AgentFactoryV2 is IAgentFactory, Initializable, AccessControl, PausableUpgradeable {

327:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

332:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

336:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

342:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

362:     ) public onlyRole(DEFAULT_ADMIN_ROLE) {

379:     ) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

396:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

19: contract AgentFactoryV3 is IAgentFactoryV3, Initializable, AccessControl, PausableUpgradeable {

341:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

346:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

350:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

356:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

360:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

364:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

376:     ) public onlyRole(DEFAULT_ADMIN_ROLE) {

393:     ) public onlyRole(DEFAULT_ADMIN_ROLE) {

402:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

406:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

410:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

432:     ) public whenNotPaused onlyRole(BONDING_ROLE) returns (uint256) {

471:     ) public onlyRole(BONDING_ROLE) noReentrant returns (address) {

489:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

21: contract AgentFactoryV4 is IAgentFactoryV4, Initializable, AccessControl, PausableUpgradeable {

365:     function setApplicationThreshold(uint256 newThreshold) public onlyRole(DEFAULT_ADMIN_ROLE) {

370:     function setVault(address newVault) public onlyRole(DEFAULT_ADMIN_ROLE) {

374:     function setImplementations(address token, address veToken, address dao) public onlyRole(DEFAULT_ADMIN_ROLE) {

380:     function setMaturityDuration(uint256 newDuration) public onlyRole(DEFAULT_ADMIN_ROLE) {

384:     function setUniswapRouter(address router) public onlyRole(DEFAULT_ADMIN_ROLE) {

388:     function setTokenAdmin(address newTokenAdmin) public onlyRole(DEFAULT_ADMIN_ROLE) {

400:     ) public onlyRole(DEFAULT_ADMIN_ROLE) {

417:     ) public onlyRole(DEFAULT_ADMIN_ROLE) {

426:     function setAssetToken(address newToken) public onlyRole(DEFAULT_ADMIN_ROLE) {

430:     function pause() public onlyRole(DEFAULT_ADMIN_ROLE) {

434:     function unpause() public onlyRole(DEFAULT_ADMIN_ROLE) {

446:     function setDefaultDelegatee(address newDelegatee) public onlyRole(DEFAULT_ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

15: contract AgentMigrator is Ownable, Pausable {

42:     constructor(address agentNft_) Ownable(_msgSender()) {

52:     ) external onlyOwner {

68:     ) public onlyOwner {

85:     ) public onlyOwner {

94:     function setImplementations(address token, address veToken, address dao) external onlyOwner {

183:     function pause() external onlyOwner {

187:     function unpause() external onlyOwner {

191:     function reset(uint256 id) external onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

76:     ) external onlyRole(DEFAULT_ADMIN_ROLE) {

94:     ) external onlyRole(MINTER_ROLE) returns (uint256) {

116:     function addCoreType(string memory label) public onlyRole(DEFAULT_ADMIN_ROLE) {

173:     function setTBA(uint256 virtualId, address tba) external onlyRole(MINTER_ROLE) {

234:     function setBlacklist(uint256 virtualId, bool value) public onlyRole(ADMIN_ROLE) {

239:     function migrateScoreFunctions() public onlyRole(ADMIN_ROLE) {

243:     function setEloCalculator(address eloCalculator) public onlyRole(ADMIN_ROLE) {

257:     ) public onlyRole(ADMIN_ROLE) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

```solidity
File: contracts/virtualPersona/AgentVeToken.sol

91:         require(IAccessControl(agentNft).hasRole(ADMIN_ROLE, _msgSender()), "Not admin");

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentVeToken.sol)

```solidity
File: contracts/virtualPersona/EloCalculator.sol

57:     function setK(uint256 k_) public onlyOwner {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/EloCalculator.sol)

### <a name="M-4"></a>[M-4] `_safeMint()` should be used rather than `_mint()` wherever possible

`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren't lost if they're minted to contracts that cannot transfer them back out.

Be careful however to respect the CEI pattern or add a re-entrancy guard as `_safeMint` adds a callback-check (`_checkOnERC721Received`) and a malicious `onERC721Received` could be exploited if not careful.

Reading material:

- <https://blocksecteam.medium.com/when-safemint-becomes-unsafe-lessons-from-the-hypebears-security-incident-2965209bda2a>
- <https://samczsun.com/the-dangers-of-surprising-code/>
- <https://github.com/KadenZipfel/smart-contract-attack-vectors/blob/master/vulnerabilities/unprotected-callback.md>

*Instances (3)*:

```solidity
File: contracts/contribution/ContributionNft.sol

82:         _mint(to, proposalId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

71:         _mint(info.tba, proposalId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

97:         _mint(to, virtualId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

### <a name="M-5"></a>[M-5] `increaseAllowance/decreaseAllowance` won't work on mainnet for USDT

On mainnet, the mitigation to be compatible with `increaseAllowance/decreaseAllowance` isn't applied: <https://etherscan.io/token/0xdac17f958d2ee523a2206206994597c13d831ec7#code>, meaning it reverts on setting a non-zero & non-max allowance, unless the allowance is already zero.

*Instances (2)*:

```solidity
File: contracts/virtualPersona/AgentToken.sol

562:     function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {

582:     function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="M-6"></a>[M-6] Direct `supportsInterface()` calls may cause caller to revert

Calling `supportsInterface()` on a contract that doesn't implement the ERC-165 standard will result in the call reverting. Even if the caller does support the function, the contract may be malicious and consume all of the transaction's available gas. Call it via a low-level [staticcall()](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/f959d7e4e6ee0b022b41e5b644c79369869d8411/contracts/utils/introspection/ERC165Checker.sol#L119), with a fixed amount of gas, and check the return code, or use OpenZeppelin's [`ERC165Checker.supportsInterface()`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/f959d7e4e6ee0b022b41e5b644c79369869d8411/contracts/utils/introspection/ERC165Checker.sol#L36-L39).

*Instances (4)*:

```solidity
File: contracts/contribution/ContributionNft.sol

131:         return super.supportsInterface(interfaceId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ContributionNft.sol)

```solidity
File: contracts/contribution/ServiceNft.sol

158:         return super.supportsInterface(interfaceId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/contribution/ServiceNft.sol)

```solidity
File: contracts/governance/VirtualGenesisDAO.sol

135:         return super.supportsInterface(interfaceId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/governance/VirtualGenesisDAO.sol)

```solidity
File: contracts/virtualPersona/AgentNftV2.sol

223:         return super.supportsInterface(interfaceId);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentNftV2.sol)

### <a name="M-7"></a>[M-7] Return values of `transfer()`/`transferFrom()` not checked

Not all `IERC20` implementations `revert()` when there's a failure in `transfer()`/`transferFrom()`. The function signature has a `boolean` return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

*Instances (6)*:

```solidity
File: contracts/fun/Bonding.sol

276:         token.transfer(msg.sender, token.balanceOf(address(this)));

417:                 agentToken.transferFrom(pairAddress, acc, balance);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/FGenesis.sol

78:             IERC20(params.virtualToken).transferFrom(msg.sender, params.feeAddr, params.feeAmt),

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

222:         IERC20(assetToken).transfer(token, initialAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

110:         IERC20(_assetToken).transferFrom(founder, token, initialAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

262:         IERC20(uniswapV2Pair).transfer(lpOwner, lpTokens);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

### <a name="M-8"></a>[M-8] Unsafe use of `transfer()`/`transferFrom()`/`approve()`/ with `IERC20`

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens.  For example Tether (USDT)'s `transfer()` and `transferFrom()` functions on L1 do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to `IERC20`, their [function signatures](https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca) do not match and therefore the calls made, revert (see [this](https://gist.github.com/IllIllI000/2b00a32e8f0559e8f386ea4f1800abc5) link for a test case). Use OpenZeppelin's `SafeERC20`'s `safeTransfer()`/`safeTransferFrom()` instead

*Instances (12)*:

```solidity
File: contracts/fun/Bonding.sol

276:         token.transfer(msg.sender, token.balanceOf(address(this)));

417:                 agentToken.transferFrom(pairAddress, acc, balance);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/fun/Bonding.sol)

```solidity
File: contracts/genesis/FGenesis.sol

78:             IERC20(params.virtualToken).transferFrom(msg.sender, params.feeAddr, params.feeAmt),

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/FGenesis.sol)

```solidity
File: contracts/genesis/Genesis.sol

246:             IERC20(virtualTokenAddress).approve(agentFactoryAddress, reserveAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

222:         IERC20(assetToken).transfer(token, initialAmount);

269:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

262:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

286:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

110:         IERC20(_assetToken).transferFrom(founder, token, initialAmount);

130:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

244:         IERC20(pairToken).approve(address(_uniswapRouter), type(uint256).max);

262:         IERC20(uniswapV2Pair).transfer(lpOwner, lpTokens);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)

## High Issues

| |Issue|Instances|
|-|:-|:-:|
| [H-1](#H-1) | IERC20.approve() will revert for USDT | 6 |

### <a name="H-1"></a>[H-1] IERC20.approve() will revert for USDT

Use forceApprove() from SafeERC20

*Instances (6)*:

```solidity
File: contracts/genesis/Genesis.sol

246:             IERC20(virtualTokenAddress).approve(agentFactoryAddress, reserveAmount);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/genesis/Genesis.sol)

```solidity
File: contracts/virtualPersona/AgentFactory.sol

269:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactory.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV3.sol

262:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV3.sol)

```solidity
File: contracts/virtualPersona/AgentFactoryV4.sol

286:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentFactoryV4.sol)

```solidity
File: contracts/virtualPersona/AgentMigrator.sol

130:         IERC20(lp).approve(veToken, type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentMigrator.sol)

```solidity
File: contracts/virtualPersona/AgentToken.sol

244:         IERC20(pairToken).approve(address(_uniswapRouter), type(uint256).max);

```

[Link to code](https://github.com/code-423n4/2025-04-virtuals-protocol/blob/main/contracts/virtualPersona/AgentToken.sol)
