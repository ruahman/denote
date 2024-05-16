---
title:      "using-lightning"
date:       2023-09-26T16:19:07-04:00
tags:       ["bitcoin", "core-lightning", "crypto", "lit", "tech"]
identifier: "20230926T161907"
---

create an invoice
`lightning-cli invoice <amount> <label> <description>`

> lightning-cli --testnet invoice 100000 joe-payment "The money you owe me for dinner"


you can decode an invoice
`lightning-cli decodepay <bolt11>`


Paying an Invoice
=================

pay bolt11
`lightning-cli pay <invoice>`


Closing a Channel
=================


to close a channel you need to get the ID of the channel
you can do that in two ways

## listfunds ##

`lightning-cli listfunds`

## listchannels ##

this list channels that are known to the node.
however it might return thousands of nodes

first you need to find you node ID
`nodeid=$(lightning-cli getinfo | jq .id)`

you can then search through channels
`lightning-cli --testnet listchannels | jq '.channels[] | select(.source == '$nodeid' or .destination == '$nodeid')'`

you can store it in a variable
`c$ nodeidremote=$(lightning-cli --testnet listchannels | jq '.channels[] | select(.source == '$nodeid' or .destination == '$nodeid') | .destination')`

Close a Channel
---------------

`lightning-cli close <id> <unilateraltimeout>`
- if unilateraltimeout is not 0 then when time runs out then it will attemt to
  close unilateraly, the bad way
  
a channel is either close cooperatively or unilateraly 
  

