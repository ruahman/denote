---
title:      "webassembly"
date:       2023-07-26T09:31:56-04:00
tags:       ["tech"]
identifier: "20230726T093156"
---


it is a stack based virtual machine

it's not meant to replace javascript
- you still need javascript to host it on the computer
- all IO is handled through the host

https://esmbly.github.io/WebAssemblyStudio/

``` rust
#[no_mangle]
pub extern "C" fn add_one(x: i32) -> i32 {
    x + 1
}
```
- no_mangle means don't change the signature of the function durring compilation

``` javascript
etch('../out/main.wasm').then(response =>
  response.arrayBuffer()
).then(bytes => WebAssembly.instantiate(bytes)).then(results => {
  instance = results.instance;
  document.getElementById("container").textContent = instance.exports.add_one(41);
}).catch(console.error);

```

