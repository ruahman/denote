---
title:      "postcss"
date:       2023-08-05T18:19:08-04:00
tags:       ["lit", "tech"]
identifier: "20230805T181908"
---

transform styles throught javascript plugins

@mixins
nesting

convert css to a ast(abstract syntax tree) and apply plugings to it for transformation

autofrefixer
- provide vender prefixes

preset env
- modern css features


stylelint
- css linter???

postcss is used under the hood of vite and next.js
- used by twitter and wikipedia

can also work with sass

there are plugins that allow nesting and @mixins

postcss.config.cjs
``` javascript
module.exports = {
  plugins: [
	require('postcss-nested'),
	require('postcss-mixins'),
	require('postcss-preset-env'),
	require('autoprefixer'),
	require('stylelint'),
	require('cssnano')
  ]
}
```

`npm install postcss autoprefixer postcss-nested`

compile css
`postcss in.css -o out.css`
- don't need to do this with vite




you can use nesting, mixins, and variables through javascript plugins that transform the AST of the css.

it's really a transpiler

postcss-preset-env
- stage 2 are features that are most likely going to be in future css implementations

you can rewrite postcss to use es modules

it is a tool for transforming css

prefixing

postcss-import
- import css files together

autoprefixer

cssnano

postcss-mixins

postcss-nested
- sass style nesting

postcss-nesting
- future css nesting spec

postcss-preset-env
- group of plugins

this is one of the most downloaded npm packages
- it beates typescript

precss
- give sass like syntax

postcss-assets
- easier way to specify assets in css



