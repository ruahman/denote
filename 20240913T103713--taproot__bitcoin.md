---
title:      "taproot"
date:       2024-09-13T10:37:13-04:00
tags:       ["bitcoin"]
identifier: "20240913T103713"
---

Taproot enhances Bitcoin’s privacy and efficiency by introducing 
*Schnorr signatures* and *Tapscript*, allowing more private and flexible transactions.

a Taproot is the deepest, strongest part of a plant. 
It grows down way into the ground, branching out and expanding every which way

# Schnorr signatures 

Schnorr signatures enable multiple keys to be come into a single transaction, with one signature empowering them. 

Schnorr signatures are 64 bytes long instead of the 72 bytes used by current ECDSA signature scheme. 

# Tapscript

Tapscript, introduced in BIP 342, is a scripting language that supports advanced transactions on the Bitcoin network. It enables a payment type known as Pay-to-Taproot (P2TR), 
