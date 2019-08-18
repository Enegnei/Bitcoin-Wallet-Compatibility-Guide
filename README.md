# A Wallet Compatibility Guide for Bitcoin Keys

> **Note: This document is still in the very early draft stages. Please focus on filling out the intro sections and formatting the comparison chart with necessary columns before trying to add wallets to it.**

### Purpose of the Guide
While great advances have been made in interoperability and recoverability, developers across the industry continue to build wallets that either:
+ Don't implement BIP standard(s)
+ Implement a BIP standard, but with important differences from other wallets that also implemented it
+ Implement a BIP standard, but one that has not been widely adopted (and perhaps only by them)

For non-technical users who do not have the knowledge or time to deeply study the code for every piece of software they use, it is often difficult for them to discern what standards any given wallet adheres to, and also to understand what (in)compatibility trade-offs they will be making by choosing this or that wallet for their bitcoin. This failure in understanding can lead to vendor lock-in, accidental address reuse, and unnecessary anxiety about why funds appear to be "lost" after importing keys to a different wallet than the one they were generated in.

The purpose of this guide is to aggregate information about how different wallets implement key generation / seed types and, in the case of hierarchical deterministic (HD) wallets, how they handle derivation paths.

### Wallet Types
First, an introduction to the various wallet types:

+ Keypool buffer
+ Sequential deterministic wallets
+ Hierarchical deterministic wallets

### What Are Derivation Paths?
In hierarchical deterministic wallets ([BIP-32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)), a derivation path is a sequence of fields or levels through which a wallet organizes coins in a multi-currency, multi-account, and multi-address system. According to [BIP-44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki), this hierarchy consists of five levels, in addition to the master extended key represented by `M` (uppercase is the public key or 'xpub') or `m` (lowercase is the private key or 'xpriv').

m / purpose' / coin_type' / account' / change / address_index

+ **Purpose:** This field, which was added through [BIP-43](https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki), indicates which standard the derivation path follows. Possibilities include `44` referring to the default [BIP-44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) P2PKH / '1' legacy addresses, `45` referring to [BIP-45](https://github.com/bitcoin/bips/blob/master/bip-0045.mediawiki) P2SH multi-party multi-signature wallets (proposed), `48` referring to hardware multisignature wallets (no BIP or standard proposal), `49` referring to [BIP-49](https://github.com/bitcoin/bips/blob/master/bip-0049.mediawiki) P2WPKH-nested-in-P2SH / '3' SegWit addresses, or `84` referring to [BIP-84](https://github.com/bitcoin/bips/blob/master/bip-0084.mediawiki) P2WPKH / 'bc1' native SegWit addresses. Some wallet services are supporting more than one (as in, they can display both legacy and SegWit accounts derived from the same seed).
+ **Coin Type:** This field indicates which cryptocurrency is being used in a multi-currency wallet. All coins, including testnet bitcoin, are assigned [a constant number](https://github.com/satoshilabs/slips/blob/master/slip-0044.md). For example, a derivation path for a Monero (XMR) account would be `m/44'/128'`. *Note that if finalized, BIP-45 would designate this level as the 'Cosigner Index' instead.*
+ **Account:** This field, in a multi-account wallet, indicates the identity or collection of addresses, which allow users to segregate funds for different things (ex. savings, donations). *Note that if finalized, BIP-45 would not include this field.*
+ **Change:** This field, if the constant `0` is present, indicates "external chain" (regular) addresses; if the constant `1`, indicates "internal chain" (change) addresses.
+ **Address Index:** This field is for indicating the specific address number in a sequence, within an account.

Note how many of the sequential fields start at zero (0), like how the first / ground floor is level zero in the U.K. and Europe. If a user has a wallet that displays them, the derivation path sequence for a **BIP-44** compliant **bitcoin** wallet, in which there is a **second change address** in the **third account**, would look like this: `m/44'/0'/2'/1/1`

The meaning of "public" / unhardened versus hardened derivation," indicated in the fields by apostrophes, is explained [here](https://wiki.trezor.io/Hardened_and_non-hardened_derivation) and [here](https://medium.com/@sevcsik/working-with-bitcoin-hd-wallets-ii-deriving-public-keys-c48341629388) and [here](https://bitcoin.stackexchange.com/questions/62533/key-derivation-in-hd-wallets-using-the-extended-private-key-vs-hardened-derivati?rq=1).

### Scope of the Guide
Since most Bitcoin wallets are now HD wallets, this guide will mostly focus on them.

## Wallet Comparison Charts

For simplicity's sake, this chart should only look at what is implemented in **_the current version of the wallet software_**. However, if it is known that a significant portion of that wallet's users are still using an outdated version with significant differences, feel free to suggest that it be included as well (listed as a new row under the row for the current version of the wallet). If a wallet's implementation of the standard has critical differences that may affect its compatibility with other wallets supporting the same standard, please mark this with (^#), which can be further expanded on in 'Notes.'

   Wallet    |   [BIP-39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)   |   [BIP-44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki)   |   [BIP-45](https://github.com/bitcoin/bips/blob/master/bip-0045.mediawiki)   |  48 (No BIP) |   [BIP-49](https://github.com/bitcoin/bips/blob/master/bip-0049.mediawiki)   |   [ BIP-84](https://github.com/bitcoin/bips/blob/master/bip-0084.mediawiki)  |
------------ | ---------- | ---------- | ---------- | ------------ | ---------- | ---------- |
   [Name]    | [Yes / No] | [Yes / No] | [Yes / No] |  [Yes / No]  | [Yes / No] | [Yes / No] |
   [Name]    | [Yes / No] | [Yes / No] | [Yes / No] |  [Yes / No]  | [Yes / No] | [Yes / No] |
   [Name]    | [Yes / No] | [Yes / No] | [Yes / No] |  [Yes / No]  | [Yes / No] | [Yes / No] |
   
#### If "Yes" To BIP-39

   Wallet    | Number of Seed Words | Wordlists |
------------ | -------------------- | --------- |
   [Name]    |       [number/s]     |           |
   [Name]    |       [number/s]     |           |

In the 'Number of Seed Words' column, possible answers include: `8`, `12`, `13`, `18`, and `24`. Keep in mind that `13` does not represent 12 words + passphrase. Some wallets do refer to passphrases as an "extension word" or the 13th / 25th word, but this is not really accurate. Please indicate the option to add a passphrase as (+P).

### Notes

1. General Note:
