# Overview

> Source: https://help.shuffle.us/en/articles/11882152-overview
> Fetched: 2026-07-07 (verbatim mirror, converted to markdown)

## **How we ensure trust and fairness in our games**

Provably Fair is a concept in online gaming that ensures that the outcome of the game is random, fair, and not manipulated. This ensures that the odds of winning are the same for every player and the outcome of every play is completely random and unpredictable.

## **How is provably fair achieved**

Provably Fair is achieved through the use of a **commitment scheme** along with **cryptographic hashing (SHA-256 algorithm)**.

The commitment scheme ensures that the outcome of a game is random but verifiable when the game concludes, while the SHA-256 algorithm is used to securely hash the data involved in the commitments to prevent manipulation.

This combination allows for transparency and verifiability in online systems, ensuring that the results are fair and cannot be tampered with.

**Simplified representation:**
    
    
    Fair result (is a combination of) User input x Server Seed (hashed) x Nonce
