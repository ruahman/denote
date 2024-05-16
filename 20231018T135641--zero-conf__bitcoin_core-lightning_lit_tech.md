---
title:      "zero-conf"
date:       2023-10-18T13:56:41-04:00
tags:       ["bitcoin", "core-lightning", "lit", "tech"]
identifier: "20231018T135641"
---

channels that are immediately available, that don't need to wait for confirmation 

they are also called turbo channels

single-funded, gives some or all their funds to the acceptor. 

the funder, funds the channel and pushes an amount to the receiver that they can spend right away 
without waiting for the channel to be confirmed.

the receiver can only spend the amount that was pushed on their end.
the can not receive anymore payments until the channel is confirmed.
