---
title:      "bun"
date:       2023-10-18T15:31:23-04:00
tags:       ["javascript", "lit", "tech"]
identifier: "20231018T153123"
---

run, test, bundle, transpile, package manager all in one.

install bun
`curl -fsSL https://bun.sh/install | bash`

drop-in replacement for nodejs

uses JavaScriptCore instead of V8

esm and commonjs works seamlessly 

transpiles typescript and JSX out of the box.
no configuration nessesary

has a good watch mode

it has workspaces

install node module in node_modules/, just like npm

make a binary lock file instead of a npm-lock.json.
- bun.lockd is faster to read

it uses optimized system calls for performance

global install cash
- npm modules and version are stored in a global cache so they are downloaded only once

provides type information for the Bun object
`bun add -d bun-types `

create a project from a template
`bun create <template> [<destination>]`

run a file in watch mode
`bun --watch run index.tsx`

run a script defined in package json
`bun run <script>`

you can just import text files
``` javascript
import text from "./text.txt";
console.log(text);
// => "Hello world!"
```

you can also just import json, and toml in they will return a a javascript object

``` javascript
import pkg from "./package.json";
import data from "./data.toml";
```

you can run wasm directly

``` javascript
bun ./my-wasm-app.wasm
```

# path mapping #

in tsconfig.json
``` json
{
  "compilerOptions": {
    "baseUrl": "./src",
    "paths": {
      "data": ["./data.ts"]
    }
  }
}
```

``` javascript
// resolves to ./src/components/Button.tsx
import { Button } from "components/Button.tsx";
```

``` javascript
// resolves to ./src/components/Button.tsx
import { Button } from "components/Button.tsx";
```

# JSX #

bun support .tsx and .jsx out of the box.
Buns internal transpiler converts JSX to javascript before execution.

Bun reads you tsconfig.json, to determan how transpile your jsx.

``` json
{
  "jsx": "react"
}

```

``` json
{
  "jsx": "react-jsx"
}

```

``` json
{
  "jsx": "react-jsxdev"
}
```

# environment variables #

Bun reads .env files

``` .env
FOO=world
BAR=hello$FOO
```

``` javascript
process.env.BAR; // => "helloworld"
```

or through the Bun object

``` javascript
Bun.env.API_TOKEN; // => "secret"
```

# Plugins #

Bun allows you to write plugs to extend the runtime and bundler.

Plugins intercept imports and applies loading logic to them

you register plugins in the bunfig.toml file
``` toml
preload = ["./myPlugin.ts"]
```
- bun automatically loads thes plugins before running a file


# Watch Mode #

bun support two types of reloading

`--watch`
- hard restart the bun process, when importing file changes

`--hot` 
- soft reload code, without restarting the process


`bun --watch index.tsx`
- when a change is detected, Bun restarts the process.

`bun --hot server.ts`
- bun does not restart the process when it detects changes.
- instead it update it's internal module cache with the new code
- variable states persist

to get hot reloading in the browser use bite


# Debugger #

`bun --inspect server.ts`
- starts a web server

`--inspect`
- starts a web socket server

`--inspect-brk`
- automaticaly injects a break point

`--inspect-wait`
- will not run code untill debuger attaches to it


# Github action #

https://github.com/oven-sh/setup-bun

# bun link #

`bun link`
- registers the current project as a linkable package

the packaged can now be linked to other projects
`bun link <package>`

`--save`
the save flag will add it to package.json

``` json
{
  "name": "my-app",
  "version": "1.0.0",
  "dependencies": {
    "cool-pkg": "link:cool-pkg"
  }
}
```

# bun pm #

give info about bun's package manager

`bun pm bin`
- show bin path for you local project

`bun pm bin -g`
- show bin path for global repository

`bun pm ls`
- list installed packages


# Global cache #

all packages are stored in the local cache.
`~/.bun/install/cache`

once packaged is installed it copies the package to the local node_module/ folder
using the fastest syscalls for the job.
- linux uses hardlinks
- mac uses clonefile

# workspaces #

tree
<root>
├── README.md
├── bun.lockb
├── package.json
├── tsconfig.json
└── packages
    ├── pkg-a
    │   ├── index.ts
    │   ├── package.json
    │   └── tsconfig.json
    ├── pkg-b
    │   ├── index.ts
    │   ├── package.json
    │   └── tsconfig.json
    └── pkg-c
        ├── index.ts
        ├── package.json
        └── tsconfig.json


``` json
{
  "name": "my-project",
  "version": "1.0.0",
  "workspaces": ["packages/*"],
  "devDependencies": {
    "example-package-in-monorepo": "workspace:*"
  }
}

```

# bundle #

`bun build ./index.tsx --outdir ./build`


``` javascript
await Bun.build({
  entrypoints: ['./index.tsx'],
  outdir: './build',
});
```

you can also run it in watch mode

``` javascript
bun build ./index.tsx --outdir ./out --watch
```

# bun test #

math.test.js
``` javascript
import { expect, test } from "bun:test";

test("2 + 2", () => {
  expect(2 + 2).toBe(4);
});

```
- to execute the above test run `bun test`

run only test that match the pattern 
`bun test --test-name-pattern addition`

set a timeout
`bun test --timeout 20`

watch for changes
`bun test --watch`


## mock ##

``` javascript
import { test, expect, mock } from "bun:test";

// create a mock function
const random = mock(() => Math.random());

test("random", async () => {
  const val = random();
  expect(val).toBeGreaterThan(0);
  
  // mock to see if it has been called
  expect(random).toHaveBeenCalled();
  
  // mock to see how many times has it been called
  expect(random).toHaveBeenCalledTimes(1);
});

```




``` javascript
import { mock } from "bun:test";
const random = mock((multiplier: number) => multiplier * Math.random());

random(2);
random(10);

random.mock.calls;
// [[ 2 ], [ 10 ]]

random.mock.results;
//  [
//    { type: "return", value: 0.6533907460954099 },
//    { type: "return", value: 0.6452713933037312 }
//  ]

```


spy on a method
``` javascript
import { test, expect, spyOn } from "bun:test";

const ringo = {
  name: "Ringo",
  sayHi() {
    console.log(`Hello I'm ${this.name}`);
  },
};

const spy = spyOn(ringo, "sayHi");

test("spyon", () => {
  expect(spy).toHaveBeenCalledTimes(0);
  ringo.sayHi();
  expect(spy).toHaveBeenCalledTimes(1);
});

```

## snapshoot ##

``` javascript
import { test, expect } from "bun:test";

test("snap", () => {
  expect("foo").toMatchSnapshot();
});

```
- the first time this test is run the result is snapshoot.
- second time result is compaired to snampshot

snap shoots are updated
`bun test --update-snapshots`


## dom testing ##

for writing headless tests for front end code and component use happy-dom, for simulating a browser environment

install the global registry
`bun add -d @happy-dom/global-registrator`

then create the following file

happydom.ts
``` javascript
import { GlobalRegistrator } from "@happy-dom/global-registrator";

GlobalRegistrator.register();

```

then preload this file before test

bunfig.toml
``` toml
[test]
preload = "./happydom.ts"

```


happy-dom is now global

``` javascript
import {test, expect} from 'bun:test';

test('dom test', () => {
  document.body.innerHTML = `<button>My button</button>`;
  const button = document.querySelector('button');
  expect(button?.innerText).toEqual('My button');
});

```



# bunx #

to run packages from npm
`bunx cowsay "Hello world!"`

packages are declared executable if the bin field is set

``` json
{
  // ... other fields
  "name": "my-cli",
  "bin": {
    "my-cli": "dist/index.js"
  }
}

```

the shebang indicates which program to use to run the script

``` javascript
#!/usr/bin/env node

console.log("Hello world!");

```

by default bun will respect the shebang, 
so to run the script in bun use the --bun flag

`bunx --bun my-cli`
