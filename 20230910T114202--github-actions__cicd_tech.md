---
title:      "github-actions"
date:       2023-09-10T11:42:02-04:00
tags:       ["cicd", "tech"]
identifier: "20230910T114202"
---

setup custom software life cycle workflows in your GitHub repository. 

fully integrated into GitHub

they are event driven

Events, Jobs, Runners, Steps, Actions

# a workflow yaml file
.github/workflows/lean-github-actions.yml

``` yaml
# name of our workflow
name: learn-github-actions

# what trigggers our workflow
on: [push]  # events, push and pull_requst are the big ones

# jobs that our workflow runs
jobs:
  check-bats-version:  # name of the job
    needs: <job> # wait for other job before firing
    runs-on: ubuntu-latest # one job rus on a single server at a time, jobs run in parallel but you can cordinate them
	steps:  #steps, a job groups a set of actions called tasks
	  # actions
	  - uses: actions/checkout@v2  # checkout the code
	  - uses: actions/setup-node@v1 # setup nodejs, this is a custom action.  it has it's own repo for you to look at
	  - run: npm install -g bats  # install bats
	  - run: bats -v # get version
```

.github\workflows\deploy.yml

``` yaml
# event
on:
  push:
    branches:
	  - master
jobs:
  build:
    run-on: ubuntu-latest
	steps:
	  - uses: actions/checkout@v2
	  - uses: actions/setup-node@v1
	    with:
		  node-version: 12
	  - run: npm ci
	  - run: npm run test
	  - run: npm run build
	  - run: npm run deploy
```

.github\workflows\integrate.yml

``` yaml
name: Node Continuous Integration
on:
  pull_request:  # this runs as soon as a pull request is made, if there are any errors it will show
    branches:
	  - main

jobs:
  test_pull_request:
    runs-on: ubuntu-latest
	steps:
	  - uses: actions/checkout@v2 # checkout repo
	  - uses: actions/setup-node@v1 # setup nodejs
	    with:
		  node-version: 12
	  - run: npm ci # clean install
	  - run: npm test # run tests
	  - run: npm run build 
```

CI :: continues integration
- build, test, merge
- runs on a continues integration server and notifies user if actions failed

CD :: continuous deployment

event :: is a trigger for a workflow, that occurs at a repository

runner :: container environment where we will run out code
- ubuntu-latest, macos-latest, microsoftwindows-latest???


new workflows
- you can use predefined workflows

setup a secret
==============

settings/secrets

deploy.yml

``` yaml
name: Firebase Continuous Deployment

on: 
  push:
    branches:
	  - main
	  
jobs:
  deploy:
    runs-on: ubuntu-latest
	steps:
	  - uses: actions/checkout@master
	  - uses: actions/setup-node@master
	    with:
		  node-version: 12
	  - run: npm ci
	  - run: npm run build
	  - uses: w9jds/firebase-action@master
	    with: 
		  args: deploy --only hosting
	    env:
		  FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}  # use the firebase secret you setup
```

uses :: uses and action
run :: run a command line command

each job runs on a sepreate server,
you can specify a job to wait for another job

you can setup branch pushing rules so that
the only way to push to a branch is by a pull request

setting/branches
- setup branch protection rule 


Github Actions
==============

have some code run automatically when an even takes place.
- push, pull

github will spin up a container to your specifications to run your code
- for testing and deployment

first create folders `.github/workflows/` file

now create a yaml 

deploy.yaml

``` yaml
name: Run Tests

# on what event it should fire
on:
  push:
    branches: [master, main]
  pull_request:
    branches: [master, main]

env:
  SSH_PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}

# jobs you should run
jobs:
  build:
    runs-on: ubuntu-latest
	
	steps:
	  - uses: actions/checkout@v2  # checkout the code
	  - uses: actions/setup-node@v2  # seupt nodejs
	    with:
		  node-version: 18
	  - run: npm ci  # do a clean install
	  - run: npm run build
	  - run: npm run deploy
	  
```

