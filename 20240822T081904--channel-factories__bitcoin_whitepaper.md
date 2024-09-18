---
title:      "channel-factories"
date:       2024-08-22T08:19:04-04:00
tags:       ["bitcoin", "whitepaper"]
identifier: "20240822T081904"
---

Bitcoin has a scalability problem
- does not allow for scaling to a worldwide payment system.

allow rapid changes of the allocation of funds to channels
and reduces the cost of opening new channels.

Instead of one transaction per channel, each user only needs one
transaction to enter a group of nodes
- within the group the user can create arbitrarily as many channels as he wants

If a node crashes or stops cooperating, the latest settlement transaction
can be submitted to the blockchain and enforce the last 
agreed-on state.

Payment channels will not appear in the blockchain, except in the 
case of disputes

Users will be able to enter the system with one blockchain transaction
and then open many channels without further blockchain contact.

atomic transfers / atomic swaps

Payment channels will not appear in the blockchain except in the case of 
disputes

The first commitment transaction is signed before the funding transaction
to ensure that no funds can be taken hostage by on party.

both parties can close the channel at any time by broadcasting the latest 
aggreed upon settelment/commitment transaction.

a commitment/settlement transaction can not be created without both
parties agreeing to it and signing it.

Users will be able to enter the system with one blockchain transaction 
and then create as many channels as he likes without further 
blockchain contact

Funds are committed to a group of users and can be moved between channels
with just a few messages inside the collaborating group

The first commitment is signed before the funding transaction is broadcast
- to ensure that both parties can get their money back

it is important to esure that the old version of the commitment transaction
cannot be used anymore.

# 2.3 Transaction replacement using timelocks

the first commitment transaction is created with a timelock of 100 days

the second commitment transaction is created with a timelock of 99 days
- so it will be valid first
- the outdated commitment transaction will never have a time where it
  can be broadcast since it's input is alread spent.
  
subsequent commitment transactions use lower timelocks

the commitment with the lowes timelock can be included in the blockchain
before the others.

the problem with that is that a payment channel has a limited lifetime.
- relative timelocks solve this problem.

Timelocks only start ticking as soon as the kickoff transaction is broadcast
- resulting in a potentially unlimited lifetime of a channel.

still there is a limit to how many commitment transactions you can make
because each susive commitment transaction needs a lower timelock.
eventually you will run out of commitment transcations you can create.

an invalidation tree solves this.
- only the path where all transaction have the lowest timelock can
  be broadcast.

- in this way many commitment transactions can be created before the
  timelocks get too low and the channel can not be updated anymore.

you can't cash in you commitment transaction untill you also submit
the kickoff transaction.

a time locks count can be relative to the inclusion of a kickoff transaction.
no counter starts until the kickoff transaction has been broadcast.

as soon as the latest transactions timelock elapsed the 
channel has to be closed.
- this limits the lifetime of the channel

the problem is that there are is a limit of how many transactions
you can create because you can only bring the timelock down so far.

invalidation trees solve this problem.



# 2.4 transaction replacement using punishment
instead of on commitment transaction, two are created for each 
party with embeds a penaltiy if someone tries to submitt a previous
transaction.

revocable transactios to replace the commitments
- revealing a secret which allows oppenent to punish cheating.

# Channel factories

funds are locked into a shared ownership between groups of nodes.

which we use to create multi-party micropayment channels which we call *channel factories* 
- which can quickly fund regular two-party channels.

Similar to reqular micropayment channels, milti-party channels can 
be implemented with either timelocks or punishments.

timelocks scales much better to larger participants

the reqular micropayment channels can be implemented using timelocks or
punishments independent from the implementation of the mult-party channels
implementation.

a new layer between the blockchain and the payment network,
giving a three-layered system
- better us the phrase off-chain protocol

better to just say another off-chain protocol. 

timelocks scales better to larger participant numbers

## Funding transaction ##

is a transaction with an *OP_CHECKMULTISIG* output.
- that is used to lock funds into a shared ownership between
  collaborating parties


## hook transaction ##

the hook transaction is the funding transaction of a multi-party channel.
- it locks the funds of many parties into a shared ownership.

## allocation transaction ## 

the allocation is one transaction or a number of sequential transactions
that take the locked funds from a multi-party channel as an input 
and fund many mult-party channels with their outputs

the allocation effectively replaces the funding transaction of a number
of two-party channels.

## commitment ##

a commitment is a transaction or a number of transaction that
return the funds of a two-party channel

a channel is constructed by first creating all transactions of the 
intial state,  then sign all except the hook.
and then  broadcasting the hook.

the hook transaction is a simple blockchain transaction which takes
inputs from all users and creates one n-of-n *OP_CHECKMULTISIG* output
- which can be spent only with the signatures of all parties.

hooks can only be spent with the signatures of all parties

# 3.1 replaceable allocations ????

replaceable transaction with many parties can be implemented 
similarly to two-party channels based on timelocks with an 
invalidation tree and a kickoff transaction at the root, 
which starts the timers when broadcast to the blockchain.

the leaves of the invalidation tree create the two-party shared accounts.



timelocks with an invalidation tree and a kickoff transaction at the root,
which starts the timmers when broadcast to the blockchain

the leaves of the invalidation tree create the two-party shared accounts.

while a reallocation is in progress, new commitments can be made 
on the subchannels.
- to ensure that they are valid indeifferent of whether the new
  allocation succeeds, commitments should be made on bothe the old
  and new subchannels



# 3.2 settlement 

when the involved parties cooperatively decide to close a channel factory,
they can create and broadcast a settlement transaction.
which pays out the current stake of each party directly from the shared
account
- the outputs are to the individuals

this only two transactions appear on the blockchain, the hook and the settlement

I one node decides to close the channel factory, it broadcast this
decision to all other nodes. everyone stop updating the subchannels and 
broadcasts the sum of his current stake.  this is enough information
for each node to create and sign the settlement transaction and broadcast 
the signature.
- lying does not help since if any node gives a number to hight the
  total sum would exceed the locked-in funds and the settlement transacttion
  would be invalid

# 3.3 Moving funds

a channel factory can be use to rebalance channels that have become one sided.

a new allocation is setup which replaces every channel with a balanced new one.

- fund can also be moved between channels, 
- new channels can be created
- or old ones removed.

# 3.4 Including a cold wallet in a channel

When the owner wants to move money from or into the channel,
the cold wallet can be brought online to create a new allocation.
- updates to the subchannels do not need the cold wallet

# 3.5 Leaving a group

there may be situations where some nodes wants to leave,
however the others would like to continue the channel
- instead of closing the channel factory and opening a new one,
  both actions can be combined into one trasaction
  + just create a new hook transaction,
    the leaving node gets it money while the other can still
	participate in the channel factory.

the allocation was replaced with a new hook.

after bordcasting the new hook, the leaving node
can spend its money on the blockchain and the others
can continue

# 3.6 Coordination of allocation updates

when a new allocation is created, the members of a channel factory
need to coordinate the creation of a new allocation transaction
and all transactions to make the new subchannels of this new allocation.

this could take a long time 

this is not a problem, as normal channel operations can be continued
as long as care is taken to make changes to the subchannels of both
the old and the new allocation.

i. a member decides that an update of the allocation is necessary and
   broadcast to all nodes in the group that a new allocation should be
   created.
- e.g, it wants to move funds to another channel

ii.  as soon as someone receives the allocation update,
   he will issue a request to all his subchannel partners
   to use the current channel state as the base for new allocation

iii. the two cooperating parties decide on a starting state

iv.  each node creates the new allocation transaction.
     these should all be identical
	 
after receiving all signatures on the new allocation, a node can stop
updating the subchannels based on the old allocations.

normal channel operations can be continued as long as care is taken to make 
changes to the subchannels of both the old and the new allocation.

# 3.8 Risks

the number of parties that can stop cooperating and close the channel rises,
as anyone involved in the multi-party channel can broadcast the allocation
transaction to the blockchain.
- afterwards the subchannels can still be used,
  as the funds are now locked in the two-party accounts,
  but the option to move funds between channels is lost.
  
there is no personal advantage in unilaterally closing a channel,
A selfish user should always prefer a settlement solution in
comparison to broadcasting the current path of the invalidation tree.






