---
title:      "plugins"
date:       2023-09-27T15:11:59-04:00
tags:       ["core-lightning", "lit", "tech"]
identifier: "20230927T151159"
---

you can write your own commands to cln


``` python
from lightning import Plugin, Millisatoshi

plugin = Plugin()

@plugin.method("hello", name="world")
def hello(plugin, name="world"):
  greeting = plugin.get_option('greeting')
  s = '{} {}'.format(greeting, name)
  plugin.log(s)
  return s
  
@plugin.method("balance")
def balance(plugin):
  funds = plugins.rpc.listfunds()
  return s
  
plugin.add_option('greeting', 'Hello', 'The greeting I should use')
```

