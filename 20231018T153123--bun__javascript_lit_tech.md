---
title:      "bun"
date:       2023-10-18T15:31:23-04:00
tags:       ["javascript", "lit", "tech"]
identifier: "20231018T153123"
---

run, test, bundle, transpile, package manager all in one.

Elegant APIs. 
Bun provides a minimal set of highly-optimized APIs for performing common tasks, like starting an HTTP server and writing files.

Bun is designed as a drop-in replacement for Node.js

 It natively implements hundreds of Node.js and Web APIs, including fs, path, Buffer and more.
 
Web-standard APIs

Bun implements the Web-standard APIs you know and love, including fetch, ReadableStream, Request, Response, WebSocket, and FormData.

Watch mode

The bun run CLI provides a smart --watch flag that automatically restarts the process when any imported file changes.


JSX just works. 
Bun internally transpiles JSX syntax to vanilla JavaScript. 
Like TypeScript itself, Bun assumes React by default but respects custom JSX transforms defined in tsconfig.json.

Cross-platform shell scripts

The Bun.$ API implements a cross-platform bash-like interpreter, shell, and coreutils.
This makes it easy to run shell scripts from JavaScript for devops tasks.

Crazy fast

Bun uses the fastest system calls available on each operating system to make installs faster than you'd think possible.

Workspaces

Workspaces are supported out of the box. Bun reads the workspaces key from your package.json and installs dependencies for your whole monorepo.

Security by default

Unlike other package managers, Bun doesn't execute postinstall scripts by default. Popular packages are automatically allow-listed; others can be added to the trustedDependencies in your package.json.

Binary lockfile

After installation, Bun creates a binary bun.lockb lockfile with the resolved versions of each dependency. The binary format makes reading and parsing much faster than JSON- or Yaml-based lockfiles.



install bun
`curl -fsSL https://bun.sh/install | bash`


To install the TypeScript definitions for Bun's built-in APIs, install @types/bun.

``` shell
bun add -d @types/bun
```


`bun init`

`bun create <template> [<destination>]`

`bun --watch run index.tsx`

# text files
text file can be imported as string

``` typescript
import text from "./text.txt";
console.log(text);
// => "Hello world!"
```

for to use bun if shebang points to nodejs
`bun run --bun vite`

# JSON and TOML

``` typescript
import pkg from "./package.json";
import data from "./data.toml";
```

# WASI
bun has experimental support for WASI
`bun ./my-wasm-app.wasm`

# SQLite
you can import sqlite directly into your code
``` typescript
import db from "./my.db" with { type: "sqlite" };
console.log(db.query("select * from users LIMIT 1").get());
```


# path

``` json
{
  "compilerOptions": {
    "paths": {
      "config": ["./config.ts"],         // map specifier to file
      "components/*": ["components/*"],  // wildcard matching
    }
  }
}
```

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

``` typescript
// resolves to ./src/components/Button.tsx
import { Button } from "components/Button.tsx";
```

``` typescript
import { foo } from "data";
console.log(foo); // => "Hello world!"
```


# environment variables

bun reads your .env files automatically

so that we have type completion for Bun.Env
``` typescript
declare module "bun" {
  interface Env {
    AWESOME: string;
  }
}
```

# Watch mode

bun supports two kinds of automatic reloading
--watch
--hot

`bun --watch index.tsx`

When a change is detected, Bun restarts the process,


`bun --hot server.ts`

This is distinct from --watch mode in that Bun does not hard-restart the entire process. Instead, it detects code changes and updates its internal module cache with the new code.

 To get hot reloading in the browser, use a framework like Vite.
 

# debugger

Bun speaks the WebKit Inspector Protocol, so you can debug your code with an interactive debugger.

``` shell
bun --inspect server.ts
```
- To enable debugging when running code with Bun, use the --inspect flag. This automatically starts a WebSocket server on an available port that can be used to introspect the running Bun process

 
The --inspect-brk flag behaves identically to --inspect, except it automatically injects a breakpoint at the first line of the executed script
 
The --inspect-wait flag behaves identically to --inspect, except the code will not execute until a debugger has attached to the running process.

debug.bun.sh
Bun hosts a web-based debugger at debug.bun.sh. It is a modified version of WebKit's Web Inspector Interface,

# workspaces

``` json
{
  "name": "my-app",
  "version": "1.0.0",
  "workspaces": ["packages/*"],
  "dependencies": {
    "preact": "^10.5.13"
  }
}
```

Workspaces make it easy to develop complex software as a monorepo consisting of several independent packages.

 the "workspaces" key is used to indicate which subdirectories should be considered packages/workspaces within the monorepo. 

Each workspace has it's own package.json. 
When referencing other packages in the monorepo  workspace:* can be used as in your package.json.

``` json
{
  "name": "pkg-a",
  "version": "1.0.0",
  "dependencies": {
    "pkg-b": "workspace:*"
  }
}
```

Dependencies can be de-duplicated. If a and b share a common dependency, it will be hoisted to the root node_modules directory. 



# CI/CD

``` yaml
name: bun-types
jobs:
  build:
    name: build-app
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4
      - name: Install bun
        uses: oven-sh/setup-bun@v1
      - name: Install dependencies
        run: bun install
      - name: Build app
        run: bun run build
```

``` yaml
jobs:
  build:
    name: build-app
    runs-on: ubuntu-latest
    steps:
      - name: Install bun
        uses: oven-sh/setup-bun
      - name: Install dependencies # (assuming your project has dependencies)
        run: bun install # You can use npm/yarn/pnpm instead if you prefer
      - name: Run tests
        run: bun test
```

# filter

allows you to execute scripts in multiple packages
`bun --filter <pattern> <script>`

`bun --filter '*' dev`


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
