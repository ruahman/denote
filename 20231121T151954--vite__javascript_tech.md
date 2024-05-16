---
title:      "vite"
date:       2023-11-21T15:19:54-04:00
tags:       ["javascript", "tech"]
identifier: "20231121T151954"
---

Instant server start, no bundling required.

replacement for a bundler dev environment like, webpack, rollup, parsel

Lightning Fast HMR
- hot module replacement (HMR)

Out-of-the-box support for TypeScript, JSX

vite takes advantage of native ES modules in the browser
- that way we don't recompile one big bundle but ship only what changes
- this also make HMR to go faster


only bundle for production
- we use Rollup as the bundler
  * there is a rust port to rollup call Rolldown.
    - this might replace esbuild in the future
	
vite is in two major parts
- a dev server that implements Hot Module Replacement with ES modules
- a build command that bundles your code using Rollup

integration with frameworks is possible with plugins

create a vite project with bun
`bunx create-vite`

vite treats index.html as source code and part of the module graph


commands you can use vite for
``` json
{
  "scripts": {
    "dev": "vite", // start dev server, aliases: `vite dev`, `vite serve`
    "build": "vite build", // build for production
    "preview": "vite preview" // locally preview production build
  }
}
```
