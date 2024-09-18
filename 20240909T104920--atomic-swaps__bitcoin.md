---
title:      "atomic-swaps"
date:       2024-09-09T10:49:20-04:00
tags:       ["bitcoin"]
identifier: "20240909T104920"
---

# 

There are a number of ways to make atomic swaps.
Hash Time Locked Contracts (HTLCs) or Point Time Locked Contracts (PTLCs)
* The latter depend on signature adaptors

The essential idea behind HTLC atomic swaps is that we set up two HTLCs, both with the same hash.

# ;alkdsjf

In essence, there are two transactions between two parties, say Alice and Bob,

For the swap to be atomic, we would like to make sure that either both transactions are completed or neither is.
- Never should one party receive one asset from the other without having paid for it by transferring the other asset completely as part of this process. - The swap should be complete or it shouldn’t take place at all.

An HTLC, within Bitcoin, is a specific type of Bitcoin script that governs the spending of a particular output on a transaction. 

The HTLC is defined by a hash and a time.

To transfer the amount of this output, as an input to another transaction, the pre-image of the hash (or secret) must be provided by the input script, signed by the receiver.

 If the cooperation of the two parties fails and the receiver does not claim the output with the secret, then after the given time, the sender may reclaim the amount of this output,
 
 When the secret is used in the happy path, the secret is revealed to the other party. 
 
 In all cases, both parties can see the hash and time of an HTLC,
 
# The first steps of an atomic swap 

Abstractly then Alice and Bob each set up an HTLC.

In HTLC-based atomic swaps, there is an asymmetry. 

Only one party knows the secret, say Alice. 

Both HTLCs will be using the same secret and hash.
- Let’s call these the swap secret and the swap hash.

An HTLC need not know the swap secret. 
However, once the swap secret is used to unlock the HTLC, then the swap secret must be revealed to the other party.

Alice creates an HTLC. In the happy path, Bob pays the amount to Alice when she provides the swap secret. Otherwise, it returns the amount to Bob after the expiration time. Of course, Alice already knows the secret. The catch is that the HTLC is not yet funded by Bob. Alice also shares the swap hash with Bob.

Bob, seeing that the amount and kind of asset in Alice’s HTLC is correct and the expiration time reasonable, creates another HTLC, using the swap hash and a time that would be reasonable to Alice. 

Alice checks to make sure that Bob’s HTLC has a matching hash and that its amount and asset are correct, its expiration time reasonable. Alice then funds Bob’s HTLC.

the concrete implementations of these abstract HTLCs differ. 

Let’s refer to the abstract HTLC as being *constructed* at that point where it can effectively be treated as an HTLC and note that in certain concrete implementations the abstract HTLC may take some time to become constructed after it is funded. Let’s call the event signalling this the ‘constructed event’

To clarify, in the happy path, let’s call the abstract HTLC *settled* once the receiver receives the amount after revealing the swap secret. 

After the abstract HTLC is settled, it may take some time for the swap secret to be revealed to the funder of the HTLC in certain implementations. Let’s call the the abstract HTLC *revealed* once the funder is given the swap secret after settlement. 

Once Bob’s HTLC is constructed, then Bob funds Alice’s HTLC. Since Alice already knows the swap secret, she can settle the HTLC once it’s engaged, automatically, without any additional steps. After Alice’s HTLC is settled, the swap secret is revealed to Bob. Bob can then settle his HTLC.

If Bob does not proceed to fund Alice’s HTLC, Alice waits for the expiration time on Bob’s HTLC and receives back her funds.

If Alice does not proceed to settle her HTLC, then both parties can wait for the HTLC expiration times and reclaim their funds.

# LN to LN atomic swap

# LN to Eth atomic swap

# Ordinal to LN atomic swap

The Ordinal-to-LN atomic swap is implemented in the same manner as the submarine atomic swap, except extra care is taken to ensure that the order of the inputs and outputs are correct for the proper transfer of the ordinal.

# Flipped and reverse atomic swaps

If the assets of the atomic swap need to go in the opposite direction, then any such atomic swap can be flipped by switching the roles of the two participants. The secret holder becomes the secret seeker, and vice versa. Let’s call that a ‘flipped atomic swap’.

If the implementations of each side of an atomic swap conform to the requirements of an abstract HTLC (AHTLC) as described, then the mechanism of the atomic swap can be changed by switching the roles of the funders of the two abstract HTLCs. Let’s call that a ‘reverse atomic swap’. To be clear, in the original atomic swap, the secret holder funds one type of AHTLC, say type1, and the secret seeker the other, say type2. Then in the reverse atom swap, the secret holder funds an AHTLC of type2, and the secret seeker an AHTLC of type1.

# Submarine atomic swap

