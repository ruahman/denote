---
title:      "vite"
date:       2023-11-21T15:19:54-04:00
tags:       ["javascript", "tech"]
identifier: "20231121T151954"
---
	
init vite
`npm init vite`

run dev server
`npm run dev`

build for production
`npm run build`

preview you production build
`npm run preview`

vite takes advantage of ES Modules and serves the directly to the browser.
- it's not rebundling everytime you make a change
- when there is a change only the module gets loaded
  
HMR (Hot Module Replacement)
	

Lightning Fast HMR
- hot module replacement (HMR)

Out-of-the-box support for TypeScript, JSX

vite takes advantage of native ES modules in the browser
- that way we don't recompile one big bundle but ship only what changes
- this also make HMR to go faster

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
